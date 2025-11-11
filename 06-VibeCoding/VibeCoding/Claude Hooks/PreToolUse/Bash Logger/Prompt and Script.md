## Project-Specific Installation

**1. Create hook directory in your project:**

```bash
mkdir -p .claude/hooks
```

**2. Save the bash-logger.sh script:**

```bash
# Save the script content to:
.claude/hooks/bash-logger.sh

# Make it executable:
chmod +x .claude/hooks/bash-logger.sh
```

**3. Create/edit `.claude/settings.json` in your project root:**

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": ".claude/hooks/bash-logger.sh",
            "timeout": 5
          }
        ]
      }
    ]
  }
}
```

**4. Add to `.gitignore`:**

```
claude-commands*.log*
```

**5. Test it:** Ask Claude Code to run any bash command - the log should appear as `claude-commands.log` in your project root.

**Done.** The hook will only work for this specific project, not globally.

---
# Script
```
#!/usr/bin/env bash

################################################################################
# bash-logger.sh - Claude Code Bash Command Logger
#
# Purpose:
#   This PreToolUse hook automatically logs all shell commands executed by
#   Claude Code to a standardized audit file in the project root directory.
#
# Hook Event: PreToolUse
# Matcher: Bash
# Timeout: 5 seconds
#
# Input (via stdin):
#   JSON object containing PreToolUse event data with the following structure:
#   {
#     "session_id": "string",
#     "transcript_path": "string",
#     "cwd": "string",
#     "hook_event_name": "PreToolUse",
#     "tool_name": "Bash",
#     "tool_input": {
#       "command": "string",
#       "description": "string (optional)"
#     }
#   }
#
# Output:
#   - Log entry appended to claude-commands.log in project root
#   - Success/error message to stdout
#   - Error details to stderr (if applicable)
#
# Exit Codes:
#   0 - Success (command execution proceeds)
#   1 - Non-blocking error (command execution still proceeds)
#
# Error Handling:
#   All errors are non-blocking. The hook will always exit with code 0 or 1
#   to ensure the original bash command execution is not prevented.
################################################################################

set -euo pipefail

# Configuration
LOG_FILENAME="claude-commands.log"
LOG_PATH="${CLAUDE_PROJECT_DIR}/${LOG_FILENAME}"
LOCK_TIMEOUT="${BASH_LOGGER_LOCK_TIMEOUT:-5}"  # Default 5 seconds, configurable via env var

# Rotation Configuration
MAX_SIZE_MB="${BASH_LOGGER_MAX_SIZE_MB:-10}"              # Default 10 MB
MAX_SIZE_BYTES=$((MAX_SIZE_MB * 1024 * 1024))             # Convert to bytes
ROTATE_COUNT="${BASH_LOGGER_ROTATE_COUNT:-10}"             # Keep last 5 rotated logs
COMPRESS_OLD="${BASH_LOGGER_COMPRESS_OLD:-false}"         # Compress rotated logs

# Masking Configuration
# BASH_LOGGER_MASK_SECRETS - Enable/disable secret masking (default: true)
#   Set to "false" to disable masking entirely
# BASH_LOGGER_MASK_PATTERNS - Additional regex patterns to mask (colon-separated)
#   Example: "custom_key_[0-9]+:secret_token_[a-z]+"

# Color codes for output (optional, for better readability in terminals)
readonly RED='\033[0;31m'
readonly GREEN='\033[0;32m'
readonly YELLOW='\033[1;33m'
readonly NC='\033[0m' # No Color

################################################################################
# Function: log_error
# Description: Outputs error message to stderr
# Arguments:
#   $1 - Error message
################################################################################
log_error() {
    echo -e "${RED}[bash-logger ERROR]${NC} $1" >&2
}

################################################################################
# Function: log_success
# Description: Outputs success message to stdout
# Arguments:
#   $1 - Success message
################################################################################
log_success() {
    echo -e "${GREEN}[bash-logger]${NC} $1"
}

################################################################################
# Function: log_warning
# Description: Outputs warning message to stderr
# Arguments:
#   $1 - Warning message
################################################################################
log_warning() {
    echo -e "${YELLOW}[bash-logger WARNING]${NC} $1" >&2
}

################################################################################
# Function: parse_json
# Description: Parses JSON input and extracts required fields
# Arguments:
#   $1 - JSON string
#   $2 - JSON path (e.g., ".tool_input.command")
# Returns:
#   Extracted value or "N/A" if not found
################################################################################
parse_json() {
    local json="$1"
    local path="$2"
    local result

    # Try using jq if available
    if command -v jq &> /dev/null; then
        result=$(echo "$json" | jq -r "$path // \"N/A\"" 2>/dev/null || echo "N/A")
    # Try using Python if available
    elif command -v python3 &> /dev/null; then
        # Python fallback: Better JSON parsing than grep/sed but slower than jq
        # Note: $path is hardcoded in main() - do not modify to accept dynamic input
        result=$(echo "$json" | python3 -c "
import json, sys
try:
    data = json.loads(sys.stdin.read())
    path = '$path'

    # Parse based on path
    if path == '.tool_input.command':
        result = data.get('tool_input', {}).get('command', 'N/A')
    elif path == '.tool_input.description':
        result = data.get('tool_input', {}).get('description', 'N/A')
    elif path == '.cwd':
        result = data.get('cwd', 'N/A')
    elif path == '.session_id':
        result = data.get('session_id', 'N/A')
    else:
        result = 'N/A'

    # Convert None/non-string to N/A (ensures consistency with jq behavior)
    if result is None or not isinstance(result, str):
        print('N/A')
    else:
        print(result)

except (json.JSONDecodeError, KeyError, TypeError, AttributeError, ValueError):
    print('N/A')
" 2>/dev/null || echo "N/A")
    else
        # Fallback to grep/sed for basic JSON parsing
        # This is a simple implementation and may not handle all edge cases
        case "$path" in
            ".tool_input.command")
                result=$(echo "$json" | grep -o '"command"[[:space:]]*:[[:space:]]*"[^"]*"' | sed 's/.*"command"[[:space:]]*:[[:space:]]*"\(.*\)".*/\1/' || echo "N/A")
                ;;
            ".tool_input.description")
                result=$(echo "$json" | grep -o '"description"[[:space:]]*:[[:space:]]*"[^"]*"' | sed 's/.*"description"[[:space:]]*:[[:space:]]*"\(.*\)".*/\1/' || echo "N/A")
                ;;
            ".cwd")
                result=$(echo "$json" | grep -o '"cwd"[[:space:]]*:[[:space:]]*"[^"]*"' | sed 's/.*"cwd"[[:space:]]*:[[:space:]]*"\(.*\)".*/\1/' || echo "N/A")
                ;;
            ".session_id")
                result=$(echo "$json" | grep -o '"session_id"[[:space:]]*:[[:space:]]*"[^"]*"' | sed 's/.*"session_id"[[:space:]]*:[[:space:]]*"\(.*\)".*/\1/' || echo "N/A")
                ;;
            *)
                result="N/A"
                ;;
        esac
    fi

    echo "$result"
}

################################################################################
# Function: mask_sensitive_data
# Description: Masks sensitive data patterns in command strings
# Arguments:
#   $1 - Command string to mask
# Returns:
#   Masked command string with secrets replaced by ***MASKED***
################################################################################
mask_sensitive_data() {
    local command="$1"
    local masked="$command"

    # Check if masking is disabled
    if [[ "${BASH_LOGGER_MASK_SECRETS:-true}" == "false" ]]; then
        echo "$command"
        return 0
    fi

    # Pattern 1: GitHub tokens (ghp_, gho_, ghu_, ghs_, ghr_, ghc_)
    # Matches: ghp_abc123, gho_xyz789 (GitHub Personal Access Tokens, OAuth tokens, etc.)
    masked=$(echo "$masked" | sed -E 's/gh[pousr]_[A-Za-z0-9]{16,}/***MASKED***/g')

    # Pattern 2: API keys (api_key=..., api-key: ..., apikey=..., API_KEY=...)
    # Matches: api_key=abc123, api-key:"xyz789", apikey='key123', API_KEY='sk_test...'
    # Case-insensitive for keyword - handles single quotes, double quotes, and unquoted
    # Pattern 2a: Single-quoted values (API_KEY='value')
    masked=$(echo "$masked" | sed -E "s/([Aa][Pp][Ii]_?[Kk][Ee][Yy][=:][[:space:]]*)'([^']{8,})'/\1'***MASKED***'/g")
    # Pattern 2b: Double-quoted values (API_KEY="value")
    masked=$(echo "$masked" | sed -E 's/([Aa][Pp][Ii]_?[Kk][Ee][Yy][=:][[:space:]]*)\"([^\"]{8,})\"/\1"***MASKED***"/g')
    # Pattern 2c: Unquoted values (API_KEY=value)
    masked=$(echo "$masked" | sed -E 's/([Aa][Pp][Ii]_?[Kk][Ee][Yy][=:][[:space:]]*)([A-Za-z0-9_-]{8,})([[:space:]]|$)/\1***MASKED***\3/g')
    # Pattern 2d: API keys in single-quoted headers (e.g., 'api_key: sk_...')
    # Matches: curl -H 'api_key: sk_test_abc123' or curl -H 'Authorization: Bearer token123'
    masked=$(echo "$masked" | sed -E "s/'([Aa][Pp][Ii]_?[Kk][Ee][Yy][=:][[:space:]]*)([^']{8,})'/'\1***MASKED***'/g")

    # Pattern 3: Bearer tokens (case-insensitive)
    # Matches: Bearer abc123xyz, bearer token123, BEARER xyz (minimum 8 chars after Bearer)
    masked=$(echo "$masked" | sed -E 's/([Bb][Ee][Aa][Rr][Ee][Rr][[:space:]]+)([A-Za-z0-9_\.-]{8,})/\1***MASKED***/g')

    # Pattern 4: AWS access keys (AKIA...)
    # Matches: AKIAIOSFODNN7EXAMPLE
    masked=$(echo "$masked" | sed -E 's/AKIA[0-9A-Z]{16}/***MASKED***/g')

    # Pattern 5: URL-embedded passwords (user:password@host, -u user:password)
    # Matches: oauth2:token123@github.com, user:pass@example.com, -u user:password123
    masked=$(echo "$masked" | sed -E 's/(:)([^:@[:space:]]{8,})(@)/\1***MASKED***\3/g')
    masked=$(echo "$masked" | sed -E 's/(-u[[:space:]]+[^:[:space:]]+:)([^[:space:]]{8,})/\1***MASKED***/g')

    # Pattern 6: Generic secrets (password=..., token=..., secret=..., PASSWORD=...)
    # Matches: password=secret123, token:"abc", auth=xyz, CLIENT_SECRET=xyz, PASSWORD="secret123"
    # Case-insensitive for keywords - handles quotes and spaces
    # First handle double-quoted values (preserve quotes)
    masked=$(echo "$masked" | sed -E 's/([Pp][Aa][Ss][Ss][Ww][Oo][Rr][Dd]|[Tt][Oo][Kk][Ee][Nn]|[Ss][Ee][Cc][Rr][Ee][Tt]|[Aa][Uu][Tt][Hh])([=:])[[:space:]]*\"([^\"]{8,})\"/\1\2"***MASKED***"/g')
    # Then handle single-quoted values (preserve quotes)
    masked=$(echo "$masked" | sed -E "s/([Pp][Aa][Ss][Ss][Ww][Oo][Rr][Dd]|[Tt][Oo][Kk][Ee][Nn]|[Ss][Ee][Cc][Rr][Ee][Tt]|[Aa][Uu][Tt][Hh])([=:])[[:space:]]*'([^']{8,})'/\1\2'***MASKED***'/g")
    # Finally handle unquoted values
    masked=$(echo "$masked" | sed -E 's/([Pp][Aa][Ss][Ss][Ww][Oo][Rr][Dd]|[Tt][Oo][Kk][Ee][Nn]|[Ss][Ee][Cc][Rr][Ee][Tt]|[Aa][Uu][Tt][Hh])([=:])[[:space:]]*([^[:space:]"\047&]{8,})/\1\2***MASKED***/g')

    # Pattern 7: Command-line password flags (-p"password", -ppassword, --password password)
    masked=$(echo "$masked" | sed -E 's/-p["\047]?([^[:space:]"\047]{8,})["\047]?/-p***MASKED***/g')
    masked=$(echo "$masked" | sed -E 's/--password[[:space:]]+["\047]?([^[:space:]"\047]{8,})["\047]?/--password ***MASKED***/g')

    # Pattern 8: Private keys (handles both literal \n and actual newlines)
    # Matches: -----BEGIN RSA PRIVATE KEY-----\n...\n-----END RSA PRIVATE KEY-----
    # First pass: literal \n sequences (as-is in JSON)
    masked=$(echo "$masked" | sed -E 's/-----BEGIN[[:space:]]+(RSA[[:space:]]+|OPENSSH[[:space:]]+)?PRIVATE[[:space:]]+KEY-----[^-]*-----END[[:space:]]+(RSA[[:space:]]+|OPENSSH[[:space:]]+)?PRIVATE[[:space:]]+KEY-----/***MASKED_PRIVATE_KEY***/g')

    # Second pass: actual newlines (after shell expansion) - use perl for multiline support
    if command -v perl &> /dev/null && echo "$masked" | grep -q "BEGIN.*PRIVATE.*KEY"; then
        # Use perl with /s modifier for multiline matching
        masked=$(echo "$masked" | perl -0pe 's/-----BEGIN\s+(RSA\s+|OPENSSH\s+)?PRIVATE\s+KEY-----.*?-----END\s+(RSA\s+|OPENSSH\s+)?PRIVATE\s+KEY-----/***MASKED_PRIVATE_KEY***/gs')
    fi

    # Custom patterns from environment variable (colon-separated)
    if [[ -n "${BASH_LOGGER_MASK_PATTERNS:-}" ]]; then
        IFS=':' read -ra PATTERNS <<< "$BASH_LOGGER_MASK_PATTERNS"
        for pattern in "${PATTERNS[@]}"; do
            if [[ -n "$pattern" ]]; then
                # Use subshell to isolate sed errors
                masked=$(echo "$masked" | sed -E "s/${pattern}/***MASKED***/g" 2>/dev/null || echo "$masked")
            fi
        done
    fi

    echo "$masked"
}

################################################################################
# Function: format_log_entry
# Description: Formats a log entry with timestamp and command details
# Arguments:
#   $1 - Command
#   $2 - Description
#   $3 - Working directory
#   $4 - Session ID
# Returns:
#   Formatted log entry string
################################################################################
format_log_entry() {
    local command="$1"
    local description="$2"
    local working_dir="$3"
    local session_id="$4"

    # Generate ISO 8601 timestamp with milliseconds
    local timestamp
    if date --version &>/dev/null; then
        # GNU date (Linux)
        timestamp=$(date -u +"%Y-%m-%dT%H:%M:%S.%3NZ" 2>/dev/null || date -u +"%Y-%m-%dT%H:%M:%SZ")
    else
        # BSD date (macOS)
        timestamp=$(date -u +"%Y-%m-%dT%H:%M:%SZ")
    fi

    # Format the log entry
    cat <<EOF
[${timestamp}] COMMAND: ${command}
  DESCRIPTION: ${description}
  WORKING_DIR: ${working_dir}
  SESSION_ID: ${session_id}
---
EOF
}

################################################################################
# Function: acquire_lock
# Description: Acquires an exclusive lock using directory creation (atomic operation)
# Arguments:
#   $1 - Lock file path
#   $2 - Timeout in seconds
# Returns:
#   0 on success, 1 on failure (timeout or error)
################################################################################
acquire_lock() {
    local lockfile="$1"
    local timeout="$2"
    local elapsed=0
    local sleep_interval=0.1
    local max_attempts=$((timeout * 10))  # 0.1s intervals = 10 attempts per second

    while ! mkdir "$lockfile" 2>/dev/null; do
        sleep "$sleep_interval"
        elapsed=$((elapsed + 1))

        if [ $elapsed -ge $max_attempts ]; then
            log_error "Failed to acquire lock after ${timeout}s (possible deadlock or high contention)"
            return 1
        fi
    done

    return 0
}

################################################################################
# Function: release_lock
# Description: Releases the lock by removing the lock directory
# Arguments:
#   $1 - Lock file path
# Returns:
#   0 on success, 1 on failure
################################################################################
release_lock() {
    local lockfile="$1"

    if [ -d "$lockfile" ]; then
        if rmdir "$lockfile" 2>/dev/null; then
            return 0
        else
            log_warning "Failed to release lock: ${lockfile}"
            return 1
        fi
    fi

    return 0
}

################################################################################
# Function: rotate_log_if_needed
# Description: Rotates log file if it exceeds MAX_SIZE_BYTES
# Arguments:
#   $1 - Log file path
# Returns:
#   0 on success or no rotation needed, 1 on error (non-blocking)
################################################################################
rotate_log_if_needed() {
    local log_path="$1"

    # Check if file exists
    if [[ ! -f "$log_path" ]]; then
        return 0  # No file, no rotation needed
    fi

    # Get file size in bytes (portable across macOS and Linux)
    local file_size
    if stat --version &>/dev/null; then
        # GNU stat (Linux)
        file_size=$(stat -c%s "$log_path" 2>/dev/null || echo "0")
    else
        # BSD stat (macOS)
        file_size=$(stat -f%z "$log_path" 2>/dev/null || echo "0")
    fi

    # Check if rotation is needed
    if [[ "$file_size" -lt "$MAX_SIZE_BYTES" ]]; then
        return 0  # File size below threshold
    fi

    # Generate timestamp for rotation filename (UTC)
    local timestamp
    timestamp=$(date -u +"%Y%m%d-%H%M%S" 2>/dev/null)
    if [[ -z "$timestamp" ]]; then
        log_warning "Failed to generate timestamp for rotation"
        return 1
    fi

    # Generate rotation filename
    local log_dir log_base
    log_dir=$(dirname "$log_path")
    log_base=$(basename "$log_path" .log)
    local rotated_path="${log_dir}/${log_base}-${timestamp}.log"

    # Rename current log file
    if ! mv "$log_path" "$rotated_path" 2>/dev/null; then
        log_warning "Failed to rotate log file: ${log_path} -> ${rotated_path}"
        return 1
    fi

    log_success "Rotated log file: $(basename "$rotated_path") (size: $((file_size / 1024 / 1024))MB)"

    # Optional: Compress rotated file
    if [[ "$COMPRESS_OLD" == "true" ]] && command -v gzip &>/dev/null; then
        if gzip "$rotated_path" 2>/dev/null; then
            log_success "Compressed rotated log: $(basename "$rotated_path").gz"
        else
            log_warning "Failed to compress rotated log: $(basename "$rotated_path")"
        fi
    fi

    # Cleanup old rotations - keep only last ROTATE_COUNT files
    # List all rotated logs sorted by name (newest first due to timestamp format)
    local rotated_logs
    rotated_logs=$(ls -1t "${log_dir}/${log_base}"-[0-9]*.log* 2>/dev/null || true)

    if [[ -n "$rotated_logs" ]]; then
        local count=0
        while IFS= read -r rotated_file; do
            count=$((count + 1))
            if [[ $count -gt $ROTATE_COUNT ]]; then
                if rm -f "$rotated_file" 2>/dev/null; then
                    log_success "Removed old rotation: $(basename "$rotated_file")"
                else
                    log_warning "Failed to remove old rotation: $(basename "$rotated_file")"
                fi
            fi
        done <<< "$rotated_logs"
    fi

    return 0
}

################################################################################
# Function: append_log_entry
# Description: Appends a log entry to the log file with file locking
# Arguments:
#   $1 - Log entry content
# Returns:
#   0 on success, 1 on failure
################################################################################
append_log_entry() {
    local entry="$1"
    local lockfile="${LOG_PATH}.lock"

    # Acquire lock with timeout
    if ! acquire_lock "$lockfile" "$LOCK_TIMEOUT"; then
        log_warning "Proceeding without lock (non-blocking mode)"
        # Attempt rotation and write anyway (non-blocking behavior)
        rotate_log_if_needed "$LOG_PATH" || true  # Ignore rotation failures
        if echo "$entry" >> "$LOG_PATH" 2>/dev/null; then
            return 0
        else
            return 1
        fi
    fi

    # With lock held: check and rotate BEFORE writing
    rotate_log_if_needed "$LOG_PATH" || true  # Ignore rotation failures (non-blocking)

    # Write with lock held
    local write_result=0
    if ! echo "$entry" >> "$LOG_PATH" 2>/dev/null; then
        write_result=1
    fi

    # Always release lock, even if write failed
    release_lock "$lockfile"

    return $write_result
}

################################################################################
# Main execution
################################################################################
main() {
    # Read JSON input from stdin
    local json_input
    if ! json_input=$(cat); then
        log_error "Failed to read JSON input from stdin"
        exit 1
    fi

    # Validate that we received input
    if [[ -z "$json_input" ]]; then
        log_error "Received empty JSON input"
        exit 1
    fi

    # Parse JSON fields
    local command description working_dir session_id

    command=$(parse_json "$json_input" ".tool_input.command")
    description=$(parse_json "$json_input" ".tool_input.description")
    working_dir=$(parse_json "$json_input" ".cwd")
    session_id=$(parse_json "$json_input" ".session_id")

    # Validate that we at least have a command
    if [[ "$command" == "N/A" || -z "$command" ]]; then
        log_error "Failed to extract command from JSON input"
        log_error "Input sample: ${json_input:0:200}"
        exit 1
    fi

    # Mask sensitive data in command
    command=$(mask_sensitive_data "$command")

    # Format log entry
    local log_entry
    log_entry=$(format_log_entry "$command" "$description" "$working_dir" "$session_id")

    # Attempt to append log entry
    if append_log_entry "$log_entry"; then
        log_success "Command logged to ${LOG_FILENAME}"
        exit 0
    else
        log_error "Failed to write to log file: ${LOG_PATH}"
        log_error "Check file permissions and disk space"

        # Try fallback: log to current directory
        local fallback_path="./${LOG_FILENAME}"
        if echo "$log_entry" >> "$fallback_path" 2>/dev/null; then
            log_warning "Logged to fallback location: ${fallback_path}"
            exit 1
        else
            log_error "Fallback logging also failed"
            exit 1
        fi
    fi
}

# Execute main function
main
```