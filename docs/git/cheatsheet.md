# Git Cheat Sheet

A comprehensive guide to common Git scenarios and commands.

## Table of Contents
- [Basic Setup](#basic-setup)
- [Common Workflows](#common-workflows)
- [Branching](#branching)
- [Viewing Changes with git diff](#viewing-changes-with-git-diff)
- [Staging and Committing](#staging-and-committing)
- [Undoing Changes](#undoing-changes)
- [Remote Operations](#remote-operations)
- [Stashing](#stashing)
- [History and Logs](#history-and-logs)
- [Advanced Scenarios](#advanced-scenarios)

---

## Basic Setup

### Configure Git
```bash
# Set your name and email
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# View all config
git config --list

# View specific config
git config user.name
```

### Initialize a Repository
```bash
# Create a new repository
git init

# Clone an existing repository
git clone <url>
git clone <url> <directory-name>
```

---

## Common Workflows

### Starting a New Feature
```bash
# Create and switch to a new branch
git checkout -b feature/my-feature

# Make changes, then stage and commit
git add .
git commit -m "Add new feature"

# Push to remote
git push -u origin feature/my-feature
```

### Updating Your Branch with Latest Changes
```bash
# From main/master branch
git checkout main
git pull origin main

# Switch back to your branch and merge
git checkout feature/my-feature
git merge main

# Or use rebase (cleaner history)
git checkout feature/my-feature
git rebase main
```

### Quick Save and Push
```bash
# Stage all changes, commit, and push
git add .
git commit -m "Your commit message"
git push
```

---

## Branching

### Create and Switch Branches
```bash
# Create a new branch
git branch feature/new-feature

# Switch to a branch
git checkout feature/new-feature

# Create and switch in one command
git checkout -b feature/new-feature

# Using newer syntax (Git 2.23+)
git switch feature/new-feature
git switch -c feature/new-feature
```

### List and Delete Branches
```bash
# List all local branches
git branch

# List all remote branches
git branch -r

# List all branches (local and remote)
git branch -a

# Delete a local branch
git branch -d feature/old-feature

# Force delete a local branch
git branch -D feature/old-feature

# Delete a remote branch
git push origin --delete feature/old-feature
```

### Rename a Branch
```bash
# Rename current branch
git branch -m new-branch-name

# Rename a specific branch
git branch -m old-name new-name
```

---

## Viewing Changes with git diff

### Basic Diff Commands
```bash
# Show unstaged changes in working directory
git diff

# Show staged changes (what will be committed)
git diff --staged
# or
git diff --cached

# Show both staged and unstaged changes
git diff HEAD
```

### Comparing Branches and Commits
```bash
# Compare two branches
git diff branch1..branch2
git diff branch1...branch2  # Shows changes since common ancestor

# Compare current branch with another
git diff main

# Compare specific commits
git diff commit1 commit2
git diff abc123 def456

# Compare with a previous commit
git diff HEAD~1  # Compare with 1 commit ago
git diff HEAD~3  # Compare with 3 commits ago
```

### Diff Specific Files
```bash
# Show changes in a specific file
git diff path/to/file

# Show changes in a specific file between commits
git diff commit1 commit2 -- path/to/file

# Show changes in a directory
git diff path/to/directory/
```

### Diff Output Options
```bash
# Show only file names that changed
git diff --name-only

# Show file names with status (modified, added, deleted)
git diff --name-status

# Show word-level diff instead of line-level
git diff --word-diff

# Show statistics (insertions/deletions)
git diff --stat

# Show compact summary
git diff --compact-summary

# Ignore whitespace changes
git diff -w
# or
git diff --ignore-all-space
```

### Diff with Remote Branches
```bash
# Compare local branch with remote
git diff origin/main

# Compare local main with remote main
git diff main origin/main

# Fetch latest and compare
git fetch
git diff origin/main
```

### Advanced Diff Usage
```bash
# Show diff with context lines (default is 3)
git diff -U10  # Show 10 lines of context

# Show diff of specific commit
git diff commit-hash^!

# Show changes introduced by a merge commit
git diff commit-hash^1 commit-hash

# Compare stashed changes
git diff stash@{0}
```

---

## Staging and Committing

### Staging Files
```bash
# Stage a specific file
git add path/to/file

# Stage all changes
git add .
git add -A
git add --all

# Stage only modified and deleted files (not new files)
git add -u

# Stage interactively (choose what to stage)
git add -p
```

### Committing Changes
```bash
# Commit with message
git commit -m "Your commit message"

# Commit with multi-line message
git commit -m "Title" -m "Description line 1" -m "Description line 2"

# Stage and commit in one command
git commit -am "Your message"

# Amend the last commit
git commit --amend -m "Updated message"

# Amend without changing message
git commit --amend --no-edit
```

---

## Undoing Changes

### Discard Changes
```bash
# Discard changes in a specific file
git checkout -- path/to/file
# or (Git 2.23+)
git restore path/to/file

# Discard all changes in working directory
git checkout -- .
git restore .

# Unstage a file (keep changes in working directory)
git reset HEAD path/to/file
# or (Git 2.23+)
git restore --staged path/to/file
```

### Reset Commits
```bash
# Undo last commit, keep changes staged
git reset --soft HEAD~1

# Undo last commit, keep changes unstaged
git reset HEAD~1
git reset --mixed HEAD~1

# Undo last commit, discard changes
git reset --hard HEAD~1

# Reset to a specific commit
git reset --hard commit-hash
```

### Revert Commits
```bash
# Create a new commit that undoes a previous commit
git revert commit-hash

# Revert the last commit
git revert HEAD

# Revert multiple commits
git revert HEAD~3..HEAD
```

---

## Remote Operations

### Working with Remotes
```bash
# List remotes
git remote -v

# Add a remote
git remote add origin <url>

# Change remote URL
git remote set-url origin <new-url>

# Remove a remote
git remote remove origin
```

### Fetch, Pull, and Push
```bash
# Fetch changes from remote (doesn't merge)
git fetch origin

# Fetch a specific branch
git fetch origin branch-name

# Pull changes (fetch + merge)
git pull origin main

# Pull with rebase
git pull --rebase origin main

# Push to remote
git push origin branch-name

# Push and set upstream
git push -u origin branch-name

# Force push (use with caution!)
git push --force origin branch-name
# Safer force push
git push --force-with-lease origin branch-name
```

### Tracking Branches
```bash
# Set upstream branch
git branch --set-upstream-to=origin/branch-name

# Push and set upstream
git push -u origin branch-name

# See tracking status
git branch -vv
```

---

## Stashing

### Basic Stashing
```bash
# Stash current changes
git stash

# Stash with a message
git stash save "Work in progress on feature X"

# List all stashes
git stash list

# Apply most recent stash
git stash apply

# Apply a specific stash
git stash apply stash@{2}

# Apply and remove stash
git stash pop

# Remove a specific stash
git stash drop stash@{0}

# Clear all stashes
git stash clear
```

### Advanced Stashing
```bash
# Stash including untracked files
git stash -u
git stash --include-untracked

# Stash only unstaged changes
git stash --keep-index

# Create a branch from stash
git stash branch new-branch-name

# Show stash contents
git stash show
git stash show -p  # Show full diff
```

---

## History and Logs

### Viewing History
```bash
# Show commit history
git log

# Show compact history
git log --oneline

# Show history with graph
git log --graph --oneline --all

# Show last N commits
git log -n 5

# Show commits by author
git log --author="John Doe"

# Show commits in date range
git log --since="2 weeks ago"
git log --after="2024-01-01" --before="2024-12-31"
```

### Searching History
```bash
# Search commits by message
git log --grep="bug fix"

# Search commits by code changes
git log -S "function_name"

# Show commits that modified a file
git log -- path/to/file

# Show changes in a file over time
git log -p path/to/file
```

### Viewing Specific Commits
```bash
# Show details of a specific commit
git show commit-hash

# Show files changed in a commit
git show --name-only commit-hash

# Show stats for a commit
git show --stat commit-hash
```

---

## Advanced Scenarios

### Cherry-Pick Commits
```bash
# Apply a specific commit to current branch
git cherry-pick commit-hash

# Cherry-pick without committing
git cherry-pick -n commit-hash

# Cherry-pick a range of commits
git cherry-pick commit1^..commit2
```

### Interactive Rebase
```bash
# Rebase last 3 commits interactively
git rebase -i HEAD~3

# Rebase onto another branch
git rebase -i main

# Continue after resolving conflicts
git rebase --continue

# Abort rebase
git rebase --abort
```

### Working with Tags
```bash
# List all tags
git tag

# Create a lightweight tag
git tag v1.0.0

# Create an annotated tag
git tag -a v1.0.0 -m "Version 1.0.0"

# Tag a specific commit
git tag -a v1.0.0 commit-hash -m "Version 1.0.0"

# Push tags to remote
git push origin v1.0.0
git push origin --tags  # Push all tags

# Delete a tag
git tag -d v1.0.0
git push origin --delete v1.0.0  # Delete from remote
```

### Bisect (Find Bug Introduction)
```bash
# Start bisect session
git bisect start

# Mark current commit as bad
git bisect bad

# Mark a known good commit
git bisect good commit-hash

# Mark current as good/bad during bisect
git bisect good
git bisect bad

# End bisect session
git bisect reset
```

### Cleaning Up
```bash
# Remove untracked files (dry run)
git clean -n

# Remove untracked files
git clean -f

# Remove untracked files and directories
git clean -fd

# Remove ignored files as well
git clean -fdx
```

### Submodules
```bash
# Add a submodule
git submodule add <url> path/to/submodule

# Initialize submodules after cloning
git submodule init
git submodule update

# Clone with submodules
git clone --recursive <url>

# Update all submodules
git submodule update --remote
```

### Resolve Merge Conflicts
```bash
# See conflicted files
git status

# After manually resolving conflicts
git add path/to/resolved/file

# Continue merge/rebase
git merge --continue
git rebase --continue

# Abort merge/rebase
git merge --abort
git rebase --abort

# Use theirs or ours for conflicts
git checkout --ours path/to/file
git checkout --theirs path/to/file
```

### Finding Who Changed What
```bash
# Show who last modified each line
git blame path/to/file

# Blame with line range
git blame -L 10,20 path/to/file

# Show blame ignoring whitespace
git blame -w path/to/file
```

---

## Quick Reference

### Status and Info
```bash
git status                  # Show working tree status
git log                     # Show commit history
git diff                    # Show changes
git branch                  # List branches
git remote -v              # List remotes
```

### Common Operations
```bash
git add <file>             # Stage file
git commit -m "msg"        # Commit changes
git push                   # Push to remote
git pull                   # Pull from remote
git checkout <branch>      # Switch branch
git merge <branch>         # Merge branch
```

### Emergency Commands
```bash
git reflog                 # Show all actions (recover lost commits)
git reset --hard HEAD      # Discard all local changes
git clean -fd              # Remove untracked files
git stash                  # Save work temporarily
```

---

## Tips and Best Practices

1. **Commit Often**: Make small, logical commits
2. **Write Good Commit Messages**: Clear, concise, descriptive
3. **Pull Before Push**: Always pull latest changes before pushing
4. **Use Branches**: Never commit directly to main/master
5. **Review Before Commit**: Use `git diff --staged` before committing
6. **Use .gitignore**: Keep sensitive and generated files out of repo
7. **Backup Before Force Operations**: Force push/reset can lose data
8. **Use Meaningful Branch Names**: feature/, bugfix/, hotfix/ prefixes
