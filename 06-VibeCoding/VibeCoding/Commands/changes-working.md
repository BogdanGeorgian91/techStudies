# Changes Working

Explains the changes made between the last commit and the working changes. If no commits exist on the current branch, compares working changes to where the branch diverged from main.

## Usage

```bash
claude changes-working
```

## Examples

```bash
# Show working changes since last commit
claude changes-working
```

## Implementation

```bash
#!/bin/bash

CURRENT_BRANCH=$(git branch --show-current)

# Check if we have any commits on this branch
COMMIT_COUNT=$(git rev-list --count HEAD 2>/dev/null || echo "0")

if [ "$COMMIT_COUNT" = "0" ]; then
    echo "No commits found. Comparing working changes to initial state..."
    echo
    
    echo "=== Working Changes ==="
    git status --porcelain
    echo
    
    echo "=== Detailed Changes ==="
    git diff
else
    # Check if we're on main branch
    if [ "$CURRENT_BRANCH" = "main" ] || [ "$CURRENT_BRANCH" = "master" ]; then
        # On main branch, compare with last commit
        echo "Analyzing working changes since last commit on '$CURRENT_BRANCH'..."
        COMPARE_POINT="HEAD"
    else
        # On feature branch, check if we have commits beyond the merge base
        MERGE_BASE=$(git merge-base HEAD main 2>/dev/null)
        if [ -z "$MERGE_BASE" ]; then
            MERGE_BASE=$(git merge-base HEAD master 2>/dev/null)
        fi
        
        if [ -z "$MERGE_BASE" ]; then
            echo "Could not find merge base. Comparing with last commit..."
            COMPARE_POINT="HEAD"
        else
            # Check if we have commits on this branch beyond the merge base
            BRANCH_COMMITS=$(git rev-list --count "$MERGE_BASE"..HEAD 2>/dev/null || echo "0")
            
            if [ "$BRANCH_COMMITS" = "0" ]; then
                echo "No commits on this branch yet. Comparing working changes to where branch diverged from main..."
                COMPARE_POINT="$MERGE_BASE"
            else
                echo "Analyzing working changes since last commit on '$CURRENT_BRANCH'..."
                COMPARE_POINT="HEAD"
            fi
        fi
    fi
    
    echo
    
    # Show status of working changes
    echo "=== Working Status ==="
    git status --porcelain
    echo
    
    # Show staged changes
    STAGED_CHANGES=$(git diff --cached --name-only)
    if [ -n "$STAGED_CHANGES" ]; then
        echo "=== Staged Changes ==="
        git diff --cached --name-status
        echo
        
        echo "=== Staged Diff ==="
        git diff --cached
        echo
    fi
    
    # Show unstaged changes
    UNSTAGED_CHANGES=$(git diff --name-only)
    if [ -n "$UNSTAGED_CHANGES" ]; then
        echo "=== Unstaged Changes ==="
        git diff --name-status
        echo
        
        echo "=== Unstaged Diff ==="
        git diff
    fi
    
    # If no working changes
    if [ -z "$STAGED_CHANGES" ] && [ -z "$UNSTAGED_CHANGES" ]; then
        echo "No working changes detected."
    fi
fi
```