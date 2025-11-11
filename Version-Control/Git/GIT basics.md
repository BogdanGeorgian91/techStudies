Understanding Git comprehensively means building your knowledge like constructing a solid house—you need a strong foundation before adding the sophisticated features. Let me guide you through this journey from the essential basics to senior-level mastery, explaining not just what commands to use, but why they work and when to apply them.

## Foundational Mental Models: Understanding Git's Core Philosophy

Before you learn any commands, you need to understand how Git thinks about your code. Imagine Git as a sophisticated photographer who takes snapshots of your entire project at different moments in time. Unlike a simple backup system that just copies files, Git creates a complete history where you can see exactly what changed, when it changed, and why it changed.

Git operates on the principle that every version of your project is valuable and should be preserved. When you make a commit, Git doesn't just save your current files—it creates a snapshot that includes references to all files in your project, even ones that didn't change. This approach means Git can quickly show you the complete state of your project at any point in history.

The distributed nature of Git means that every copy of your repository contains the complete history. Think of this like having a complete library in every team member's office, rather than everyone sharing access to a single central library. This design makes Git incredibly robust because losing one copy doesn't destroy your project's history, and it allows teams to work effectively even when network connections are unreliable.

Understanding Git's three working areas forms the foundation for all operations. Your working directory contains the files you're currently editing—this is like your desk where you spread out papers while working on them. The staging area, also called the index, holds changes you've prepared for your next snapshot—imagine this as a special folder where you organize documents before filing them permanently. The repository contains all your committed snapshots—like a filing cabinet that preserves every important version of your work.

## Essential Daily Operations: Building Your Git Vocabulary

Every Git journey begins with repository creation and basic file tracking. When you initialize a new repository, you're telling Git to start paying attention to a particular folder and everything inside it. This creates a hidden `.git` directory that contains all of Git's internal data structures for tracking your project's evolution.

Setting up Git with your identity information ensures that every snapshot you create is properly attributed to you. This becomes crucial when working with teams because it allows everyone to see who made which changes and when.

```bash
# Initialize a new Git repository in your current directory
git init

# Configure your identity for this repository
git config user.name "Your Name"
git config user.email "your.email@example.com"

# Set these globally to avoid repeating for every repository
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

The basic workflow of tracking changes follows a deliberate pattern that encourages thoughtful development. First, you modify files in your working directory as you develop features or fix bugs. Then, you review your changes and decide which ones should be included in your next snapshot. Finally, you create the snapshot with a descriptive message explaining what you accomplished.

```bash
# Check what files have changed since your last commit
git status

# See exactly what changes you've made to tracked files
git diff

# Add specific files to the staging area
git add filename.js

# Add all changed files to staging area
git add .

# Add all changes including deletions
git add -A

# Remove files from staging area (undo git add)
git reset filename.js

# Create a snapshot of everything in your staging area
git commit -m "Add user authentication feature"

# Combine adding and committing for tracked files
git commit -am "Fix bug in login validation"
```

Understanding the difference between these commands helps you maintain clean, purposeful commits. The `git add` command lets you carefully choose which changes belong together in a single logical snapshot, while `git commit` creates the permanent record with an explanation of what the changes accomplish.

Viewing your project's history teaches you how your project evolved and helps you understand the story of your development process. Each commit contains not just the changes, but also metadata about when the changes were made, who made them, and how they connect to previous commits.

```bash
# View commit history with detailed information
git log

# Compact one-line view of recent commits
git log --oneline

# See what files changed in each commit
git log --name-only

# Visual representation of branch structure
git log --graph --oneline

# Show changes introduced by each commit
git log -p

# Limit to recent commits
git log -5

# Search commits by message content
git log --grep="authentication"

# Show commits by specific author
git log --author="Your Name"
```

## Branching Fundamentals: Parallel Development Paths

Branching represents one of Git's most powerful features, allowing you to create parallel versions of your project for different purposes. Think of branches like alternate timelines where you can experiment with new features or try different approaches without affecting your main development line.

Creating and switching between branches becomes second nature once you understand that branches are simply movable labels pointing to specific commits. When you create a new branch, Git doesn't copy all your files—it just creates a new pointer that initially points to the same commit as your current branch.

```bash
# Create a new branch from your current location
git branch feature-user-profiles

# Switch to the new branch
git checkout feature-user-profiles

# Create and switch to new branch in one command
git checkout -b feature-user-profiles

# In newer Git versions, you can use switch instead of checkout
git switch feature-user-profiles
git switch -c feature-user-profiles

# List all branches and see which one is active
git branch

# List both local and remote branches
git branch -a

# Delete a branch after merging it
git branch -d feature-user-profiles

# Force delete a branch even if not merged
git branch -D experimental-feature
```

Understanding when to create branches helps you organize your development workflow effectively. Create new branches for each feature, bug fix, or experiment you work on. This isolation means you can switch between different tasks without your incomplete work interfering with each other.

Merging branches brings changes from one branch into another, combining the development work done in parallel paths. Git offers different merge strategies depending on your project's needs and your team's preferences for how history should be preserved.

```bash
# Switch to the branch you want to merge changes into
git checkout main

# Merge another branch into your current branch
git merge feature-user-profiles

# Create a merge commit even if fast-forward is possible
git merge --no-ff feature-user-profiles

# Only merge if it can be done as a fast-forward
git merge --ff-only feature-user-profiles

# Abort a merge if conflicts seem too complex
git merge --abort
```

When Git encounters conflicts during merging, it's asking for your help in deciding how to combine changes that modified the same parts of files. Rather than making potentially incorrect assumptions, Git preserves both versions and asks you to make the intelligent decision about which changes to keep.

```bash
# When merge conflicts occur, Git will show you which files need attention
git status

# See the conflicted content with conflict markers
git diff

# After manually resolving conflicts in your editor, mark files as resolved
git add resolved-file.js

# Complete the merge after resolving all conflicts
git commit
```

## Remote Repository Collaboration: Connecting with Teams

Working with remote repositories extends Git's power from individual development to team collaboration. Remote repositories serve as central coordination points where team members can share their work and stay synchronized with each other's changes.

Setting up remote connections establishes links between your local repository and repositories hosted on services like GitHub, GitLab, or your company's internal Git servers. These connections allow you to push your changes for others to see and pull changes that others have made.

```bash
# Add a remote repository connection
git remote add origin https://github.com/username/project.git

# View configured remote repositories
git remote -v

# Change the URL for an existing remote
git remote set-url origin https://github.com/username/new-project.git

# Remove a remote connection
git remote remove origin

# Rename a remote
git remote rename origin upstream
```

Pushing and pulling changes coordinates your local work with the shared repository. When you push, you're uploading your local commits to the remote repository so others can access them. When you pull, you're downloading commits that others have made and integrating them into your local repository.

```bash
# Upload your commits to the remote repository
git push origin main

# Set upstream tracking for easier future pushes
git push -u origin feature-branch

# Download changes from remote repository
git pull origin main

# Fetch changes without automatically merging them
git fetch origin

# Push a new branch to remote repository
git push -u origin feature-new-dashboard

# Delete a branch on the remote repository
git push origin --delete old-feature-branch
```

Understanding the difference between `git fetch` and `git pull` helps you maintain better control over when changes get integrated into your work. Fetching downloads remote changes but doesn't modify your working files, allowing you to review incoming changes before merging them. Pulling combines fetching and merging into a single operation, which is convenient but gives you less control over the integration process.

## Undoing Changes: Recovery and Correction Techniques

Git provides multiple ways to undo changes depending on where those changes exist and how permanent you want the correction to be. Understanding these options helps you recover from mistakes gracefully without losing valuable work.

Modifying recent commits allows you to correct small mistakes without cluttering your history with "fix typo" commits. This capability is particularly valuable during feature development when you discover small improvements or corrections that logically belong with your previous commit.

```bash
# Add changes to the most recent commit without changing the message
git commit --amend --no-edit

# Add changes and update the commit message
git commit --amend -m "Updated commit message"

# Undo the last commit but keep the changes in your working directory
git reset --soft HEAD~1

# Undo the last commit and unstage the changes
git reset HEAD~1

# Undo the last commit and discard all changes (dangerous!)
git reset --hard HEAD~1

# Discard changes in working directory for specific files
git checkout -- filename.js

# Discard all changes in working directory (dangerous!)
git checkout -- .
```

The `git reset` command comes in three flavors that affect different areas of Git's three-tree architecture. Soft reset moves the branch pointer but leaves your staging area and working directory unchanged, effectively "uncommitting" your last snapshot while preserving all your changes. Mixed reset (the default) also clears the staging area, leaving changes in your working directory but unstaged. Hard reset clears everything, returning your entire repository to exactly the state of the specified commit.

Recovering lost commits becomes possible through Git's reflog, which maintains a history of where your branches have pointed over time. Even if you accidentally delete commits, they usually remain accessible through the reflog for a period of time before Git's garbage collection removes them.

```bash
# View the reflog to see recent branch movements
git reflog

# Recover a lost commit by creating a new branch
git checkout -b recovery-branch HEAD@{3}

# Reset to a previous state shown in reflog
git reset --hard HEAD@{2}
```

## File Management and History Exploration

Git's file tracking capabilities extend beyond simple version control to provide powerful tools for understanding how your project evolves over time. These capabilities become essential when debugging issues, understanding code changes, or preparing for code reviews.

Removing files from Git tracking requires understanding the difference between deleting files from your working directory and removing them from Git's version control. Sometimes you want to keep files locally but stop tracking them in Git, while other times you want to remove files entirely from the project.

```bash
# Remove file from both Git and working directory
git rm filename.js

# Remove file from Git but keep it in working directory
git rm --cached filename.js

# Remove directory and all its contents
git rm -r directory-name/

# Move or rename files while preserving history
git mv old-name.js new-name.js
```

Exploring file history helps you understand when specific changes were introduced and why they were made. This investigation capability becomes invaluable when debugging issues or when you need to understand the reasoning behind certain code decisions.

```bash
# See the history of changes to a specific file
git log filename.js

# See what changes were made to a file in each commit
git log -p filename.js

# Find out who last modified each line of a file
git blame filename.js

# See the content of a file at a specific commit
git show commit-hash:filename.js

# Compare different versions of a file
git diff HEAD~2 HEAD filename.js
```

Using `.gitignore` files properly prevents unnecessary files from cluttering your repository. Understanding ignore patterns helps you keep your repository focused on source code and documentation while excluding build artifacts, temporary files, and sensitive configuration data.

```bash
# Common .gitignore patterns for React Native projects
node_modules/          # Dependencies should be reinstalled, not tracked
*.log                  # Log files are temporary
.env                   # Environment files may contain secrets
build/                 # Build artifacts can be regenerated
.DS_Store             # macOS system files
*.tmp                 # Temporary files
.idea/                # IDE-specific files
coverage/             # Test coverage reports

# Ignore files that are already tracked
git rm --cached file-to-ignore.js
echo "file-to-ignore.js" >> .gitignore
git add .gitignore
git commit -m "Stop tracking configuration file"
```

## Intermediate Workflows: Streamlining Development Processes

As your Git skills develop, you'll discover workflows that make common development tasks more efficient and reduce the likelihood of mistakes. These intermediate techniques bridge the gap between basic operations and advanced Git mastery.

Stashing provides a temporary storage area for work-in-progress changes when you need to quickly switch contexts. Instead of making a commit with incomplete work, you can stash your changes, handle the interruption, and then restore your work exactly as you left it.

```bash
# Save current changes to stash and clean working directory
git stash

# Save with a descriptive message
git stash save "Work in progress on user authentication"

# List all stashes
git stash list

# Apply the most recent stash
git stash apply

# Apply a specific stash
git stash apply stash@{2}

# Apply stash and remove it from stash list
git stash pop

# Drop a specific stash without applying it
git stash drop stash@{1}

# Show what changes are in a stash
git stash show -p stash@{0}
```

Understanding stash workflows helps you handle interruptions gracefully. When you receive an urgent bug report while working on a feature, you can stash your feature work, create a hotfix branch, resolve the issue, and then return to your feature work exactly where you left off.

Cherry-picking allows you to apply specific commits from one branch to another without merging entire branches. This technique becomes valuable when you need to apply a bug fix to multiple release branches or when you want to extract a specific improvement from an experimental branch.

```bash
# Apply a specific commit to your current branch
git cherry-pick commit-hash

# Cherry-pick multiple commits
git cherry-pick commit1 commit2 commit3

# Cherry-pick a range of commits
git cherry-pick start-commit..end-commit

# Cherry-pick without automatically committing
git cherry-pick -n commit-hash

# Edit the commit message during cherry-pick
git cherry-pick --edit commit-hash
```

## Advanced History Management: Rewriting and Reorganizing

Advanced Git techniques allow you to craft clean, understandable project histories that tell clear stories about your development process. These skills become essential when preparing code for review or when maintaining professional repositories.

Interactive rebasing provides the most powerful tool for rewriting commit history. Through interactive rebase, you can combine related commits, improve commit messages, reorder commits for logical flow, and remove commits that don't add value to your project's story.

```bash
# Start interactive rebase for the last 5 commits
git rebase -i HEAD~5

# Interactive rebase onto another branch
git rebase -i main

# In the interactive editor, you can:
# pick = use commit as-is
# reword = change commit message
# edit = stop and modify commit
# squash = combine with previous commit
# fixup = combine with previous commit and discard message
# drop = remove commit entirely
```

Understanding when and how to rewrite history requires balancing the benefits of clean history against the dangers of changing shared commits. The golden rule states that you should never rebase commits that have been pushed to shared repositories where other developers might have based their work on them.

Rebase versus merge represents a fundamental choice in how you want your project history to appear. Rebasing creates linear history that's easy to follow and understand, while merging preserves the actual timeline of development including parallel work streams.

```bash
# Rebase your feature branch onto the latest main
git checkout feature-branch
git rebase main

# Merge main into your feature branch instead
git checkout feature-branch
git merge main

# After rebasing, merge will be fast-forward
git checkout main
git merge feature-branch  # This will be a clean fast-forward merge
```

## Repository Maintenance and Optimization

Understanding repository maintenance helps you keep Git repositories running efficiently and prevents common issues that can slow down development or cause confusion for team members.

Repository cleanup removes unnecessary data and optimizes Git's internal storage structures. While Git handles most maintenance automatically, understanding these operations helps you troubleshoot performance issues and maintain healthy repositories over time.

```bash
# Perform garbage collection to optimize repository
git gc

# More aggressive cleanup (use sparingly)
git gc --aggressive

# Check repository integrity
git fsck

# Show repository size and object count
git count-objects -v

# Remove untracked files and directories
git clean -fd

# Show what clean would remove without actually removing
git clean -n
```

Managing large repositories requires understanding how Git stores data and how to minimize repository size while preserving necessary history. This knowledge becomes particularly important for React Native projects that might accumulate large binary assets over time.

## Collaboration Patterns and Team Workflows

Professional Git usage involves understanding workflows that support effective team collaboration while maintaining code quality and project stability. These patterns represent distilled wisdom from successful development teams across many projects and organizations.

Feature branch workflows provide isolation for development work while maintaining integration points that prevent teams from diverging too far from shared goals. Understanding how to structure feature branches, when to integrate changes, and how to handle conflicts helps you contribute effectively to team projects.

```bash
# Standard feature branch workflow
git checkout main
git pull origin main                    # Start with latest main
git checkout -b feature/user-notifications
# Develop your feature with multiple commits
git push -u origin feature/user-notifications
# Create pull request for code review
# After approval, merge or rebase onto main
```

Code review integration requires understanding how to structure commits for easy review, how to respond to feedback effectively, and how to maintain clean history throughout the review process. Preparing branches for review often involves cleaning up commit history to tell a clear story about what the feature accomplishes.

```bash
# Prepare branch for review by cleaning up history
git rebase -i main
# Squash related commits, improve commit messages
git push --force-with-lease origin feature-branch

# Update branch based on review feedback
git add updated-files
git commit --fixup HEAD~2  # Mark as fix for specific commit
git rebase -i --autosquash main  # Automatically apply fixups
git push --force-with-lease origin feature-branch
```

Understanding different team workflows helps you adapt to various project requirements and team preferences. Small teams might prefer simple feature branch workflows with minimal process overhead, while larger teams might require more structured approaches with mandatory code review and automated testing integration.

## Troubleshooting and Recovery Strategies

Developing Git troubleshooting skills prepares you for the inevitable moments when something goes wrong or when you need to recover from mistakes. These skills distinguish developers who can solve their own problems from those who get blocked by Git issues.

Understanding common Git problems and their solutions helps you maintain productivity when facing challenging situations. Most Git problems fall into predictable categories related to merge conflicts, incorrect history modifications, or misunderstanding of Git's operational model.

```bash
# Common problem: Accidentally committed to wrong branch
git log  # Find the commit hash
git reset --hard HEAD~1  # Remove commit from current branch
git checkout correct-branch
git cherry-pick commit-hash  # Apply commit to correct branch

# Common problem: Need to combine commits that are spread out
git rebase -i HEAD~10  # Interactive rebase with larger range
# Reorder commits and squash related ones together

# Common problem: Merge conflict seems too complex
git merge --abort  # Cancel the merge
git rebase main  # Try rebasing instead for linear history
# Resolve conflicts one commit at a time
```

Recovery strategies provide safety nets when Git operations don't go as expected. Understanding tools like reflog, reset options, and branch recovery helps you approach Git operations with confidence, knowing that most mistakes can be undone.

The progression from basic Git operations to advanced techniques mirrors the journey from individual developer to senior team member. Each level of Git mastery opens new possibilities for effective collaboration, clean code management, and professional development practices. As you build these skills, you'll find that Git transforms from a necessary tool into a powerful enabler that supports sophisticated development workflows and team coordination strategies.

This comprehensive foundation prepares you not just for technical interviews, but for the real-world challenges of modern software development where version control skills often determine the difference between smooth project delivery and chaotic development experiences. The investment in deep Git understanding pays dividends throughout your career as you encounter increasingly complex projects and collaboration requirements.