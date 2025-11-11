
  1. Find Your Current Session ID

  Claude Code transcripts are stored here:
  ~/.claude/projects/-Users-bogdan-DEV-payshare/

  To find your current session ID:

  # List all session files, most recent first
  ```
  ls -lt ~/.claude/projects/-Users-bogdan-DEV-payshare/*.jsonl | head -5
  ```

  The newest file is your current session. The filename IS the session ID:
  88b218a9-d592-4879-8b82-8d99bc2c961f.jsonl
  └─────────────────┬─────────────────┘
                Session ID

  ---
  2. View Live Streaming in Another Terminal

  Open a second terminal window and run:

 ```
  tail -f ~/.claude/projects/-Users-bogdan-DEV-payshare/88b218a9-d592-4879-8b82-8d99bc2c961f.jsonl
 ```

  Replace 88b218a9... with your actual session ID.

  This shows:
  - Raw JSONL format (one JSON object per line)
  - Updates in real-time as you chat with Claude

  To stop: Press Ctrl+C

  ---
  3. View Formatted/Readable Output

  The raw JSONL is hard to read. To see it formatted:

  # Pretty print the last 10 entries
  ```
  tail -10 ~/.claude/projects/-Users-bogdan-DEV-payshare/88b218a9-d592-4879-8b82-8d99bc2c961f.jsonl | jq '.'
  ```

  To see specific parts:

  # Show only user messages
  ```
  cat ~/.claude/projects/-Users-bogdan-DEV-payshare/SESSION_ID.jsonl | jq 'select(.type == "user") | .message.content'
  ```

  # Show only tool uses
 ```
  cat ~/.claude/projects/-Users-bogdan-DEV-payshare/SESSION_ID.jsonl | jq 'select(.type == "assistant") | .message.content[] | select(.type == "tool_use") |
  {tool: .name, input}'
 ```

  # Show Claude's text responses
  ```
  cat ~/.claude/projects/-Users-bogdan-DEV-payshare/SESSION_ID.jsonl | jq 'select(.type == "assistant") | .message.content[] | select(.type == "text") |
  .text'
  ```

  ---
  4. Quick Helper Commands

  Add these to your ~/.zshrc for easy access:

  # Show current Claude session ID
  ```
  alias claude-session='ls -t ~/.claude/projects/-Users-bogdan-DEV-payshare/*.jsonl | head -1 | xargs basename | sed "s/.jsonl//"'
  ```

  # Watch current session live
  ```
  alias claude-watch='tail -f ~/.claude/projects/-Users-bogdan-DEV-payshare/$(ls -t ~/.claude/projects/-Users-bogdan-DEV-payshare/*.jsonl | head -1 | xargs
  basename)'
  ```

  # Show current session formatted
 ```
  alias claude-show='tail -50 ~/.claude/projects/-Users-bogdan-DEV-payshare/$(ls -t ~/.claude/projects/-Users-bogdan-DEV-payshare/*.jsonl | head -1 | xargs
  basename) | jq "."'
 ```

  Then reload: source ~/.zshrc

  Usage:
  claude-session    # Shows: 88b218a9-d592-4879-8b82-8d99bc2c961f
  claude-watch      # Live stream current session
  claude-show       # Show last 50 entries formatted

  ---
  Summary

  Session files location:
  ~/.claude/projects/-Users-bogdan-DEV-payshare/<session-id>.jsonl

  Quick start/stop workflow:
  1. Open second terminal
  2. Run: tail -f ~/.claude/projects/-Users-bogdan-DEV-payshare/<SESSION_ID>.jsonl
  3. Watch the conversation stream
  4. Press Ctrl+C when done

  No automatic logging, just manual viewing when you want it! 🎉