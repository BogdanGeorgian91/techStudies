# Changes Branch

Explains the changes made between the current branch and main (or a specified branch).

## Usage

```bash
claude changes-branch [target-branch]
```

- `target-branch`: Optional. The branch to compare against (defaults to 'main')

## Examples

```bash
# Compare current branch with main
claude changes-branch

# Compare current branch with develop
claude changes-branch develop
```

## Implementation

```bash
#!/bin/bash

TARGET_BRANCH=${1:-main}
CURRENT_BRANCH=$(git branch --show-current)

if [ "$CURRENT_BRANCH" = "$TARGET_BRANCH" ]; then
    echo "You are currently on the target branch ($TARGET_BRANCH). No changes to compare."
    exit 0
fi

echo "Analyzing changes between '$CURRENT_BRANCH' and '$TARGET_BRANCH'..."
echo

# Get the merge base to find where branches diverged
MERGE_BASE=$(git merge-base HEAD "$TARGET_BRANCH" 2>/dev/null)

if [ -z "$MERGE_BASE" ]; then
    echo "Could not find common ancestor between '$CURRENT_BRANCH' and '$TARGET_BRANCH'"
    exit 1
fi

# Show commit summary (only commits directly on this branch)
echo "=== Commit Summary ==="
git log --oneline "$MERGE_BASE".."$CURRENT_BRANCH" --first-parent
echo

# Show file changes summary (only direct changes, not merged)
echo "=== Files Changed ==="
git diff --name-status "$MERGE_BASE".."$CURRENT_BRANCH"
echo

# Show detailed changes (only direct changes, not merged)
echo "=== Detailed Changes ==="
git diff "$MERGE_BASE".."$CURRENT_BRANCH"
```