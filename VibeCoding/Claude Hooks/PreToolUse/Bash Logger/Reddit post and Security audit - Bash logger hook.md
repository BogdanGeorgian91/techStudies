https://github.com/ChrisSc/bash-logger-hook

In collaboration with claude code, I built a command auditor for Claude Code that logs every bash command with secrets masking. I've been using Claude Code heavily and wanted visibility into exactly what commands it's running in my projects. So I built a **PreToolUse** hook that automatically logs every bash command to an audit file before execution.

**Here are the highlights:**

- Automatic Logging: Every bash command logged before execution
- Secret Masking: Auto-redacts API keys, passwords, tokens (8 core patterns)
- Log Rotation: Automatic rotation when files exceed 10MB (configurable)
- Non-Blocking: Logging failures never prevent command execution
- Zero Configuration: Works immediately after installation
- Concurrent Safe: File locking prevents log corruption
- Structured Format: ISO 8601 timestamps, session tracking, working directory
- jq Optional: Works with or without jq (better parsing with jq)

You can install via Claude's plugins system:

`# Add the marketplace`  
`/plugin marketplace add ChrisSc/bash-logger-hook`  
`# Install the plugin`  
`/plugin install bash-command-logger@bash-logger-hook`

The plugin system automatically:

- Installs the hook script
- Configures the hook in Claude Code settings
- Enables the hook across all projects

After installation, restart Claude Code and the hook will be active.

This addresses an observability gap, right in your project.

---
---
---
Security Analysis Complete

  I've successfully set up the bash-logger hook in your project. Here's my comprehensive security assessment:

  ✅ SAFE - No External Data Transmission

  After thorough analysis, this script does NOT transmit any data externally. Here's what I verified:

  What the Script Does:

  - Logs all bash commands executed by Claude Code to a local file (claude-commands.log)
  - Writes only to your project directory (/Users/bogdan/DEV/payshareLanding/)
  - Includes command details: command, description, working directory, session ID, timestamp

  Security Verification:

  ✅ No network activity - No curl, wget, nc, or any network commands✅ No external APIs - All operations are local filesystem only✅ No remote logging - Data
   never leaves your machine✅ No eval/dynamic execution - No command injection vulnerabilities✅ Proper input sanitization - Uses safe JSON parsing
  (jq/python3/grep)✅ Non-blocking errors - Won't prevent your commands from running

  Built-in Security Features:

  The script includes extensive secret masking to protect:
  - GitHub tokens (ghp_, gho_, etc.)
  - API keys and Bearer tokens
  - AWS access keys
  - Passwords in URLs and command flags
  - Private keys (SSH/RSA)
  - Generic secrets (password=, token=, auth=, etc.)

  ⚠️ Important Considerations:

  What gets logged:
  - Command patterns and execution history
  - Working directory paths (reveals project structure)
  - Session IDs (for correlation)
  - Even masked commands show structure

  Best Practices:
  1. File permissions: Logs are readable - secure them if needed:
  chmod 600 claude-commands.log*
  2. Periodic cleanup: Review and delete old logs containing sensitive patterns:
  rm claude-commands-*.log*
  3. Masking limitations:
    - Best-effort, may not catch custom/unusual secret formats
    - Add custom patterns via BASH_LOGGER_MASK_PATTERNS env var if needed
  4. Git safety: ✅ Already added to .gitignore - logs won't be committed
  5. Lock files: Stale .lock directories may accumulate on crashes - clean manually if needed

  Configuration:

  Your hook is now active in .claude/settings.json:
  - Triggers on all Bash commands
  - 5-second timeout
  - Non-blocking (commands run even if logging fails)

  Files Created:

  - ✅ .claude/hooks/bash-logger.sh (executable)
  - ✅ .claude/settings.json (hook configuration)
  - ✅ .gitignore (updated with claude-commands*.log*)

  Test It:

  Next time I run a bash command, you'll see a claude-commands.log file appear in your project root with the logged command.

  Verdict: This is a safe, local-only audit logging tool. No data leaves your machine.

