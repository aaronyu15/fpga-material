# Complete Git Workflow and Troubleshooting Guide

## Basic Git Workflow

### 1. Initial Setup
```bash
# Configure Git (do this once)
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# Clone a repository
git clone https://github.com/username/repo.git
cd repo
```

### 2. Daily Workflow
```bash
# Check status of your files
git status

# Pull latest changes from remote
git pull origin main

# Make your changes to files...

# See what you've changed
git diff                    # See unstaged changes
git diff --staged          # See staged changes

# Stage files for commit
git add filename.txt       # Stage specific file
git add .                  # Stage all changes
git add -p                 # Stage changes interactively

# Commit your changes
git commit -m "Your commit message"

# Push to remote repository
git push origin main
```

### 3. Branch Workflow
```bash
# Create and switch to new branch
git checkout -b feature-branch
# or (newer syntax)
git switch -c feature-branch

# Work on your branch...
git add .
git commit -m "Feature changes"

# Switch back to main
git checkout main
# or
git switch main

# Merge your feature branch
git merge feature-branch

# Delete the feature branch
git branch -d feature-branch

# Push changes
git push origin main
```

## Essential Git Commands

### Information Commands
```bash
git status                 # Show working directory status
git log                    # Show commit history
git log --oneline         # Compact commit history
git log --graph           # Visual commit history
git show                  # Show details of last commit
git show HEAD~1           # Show details of previous commit
git diff                  # Show unstaged changes
git diff --staged         # Show staged changes
git diff HEAD~1           # Compare with previous commit
```

### File Operations
```bash
git add filename          # Stage a specific file
git add .                 # Stage all changes
git add -A                # Stage all changes including deletions
git add -p                # Interactive staging
git reset filename        # Unstage a file
git reset                 # Unstage all files
git rm filename           # Remove file from Git and filesystem
git rm --cached filename  # Remove from Git but keep locally
git mv oldname newname    # Rename/move file
```

### Commit Operations
```bash
git commit -m "message"           # Commit with message
git commit -am "message"          # Stage and commit all tracked files
git commit --amend                # Modify last commit
git commit --amend --no-edit      # Modify last commit without changing message
```

### Branch Operations
```bash
git branch                        # List branches
git branch branch-name           # Create new branch
git checkout branch-name         # Switch to branch
git checkout -b branch-name      # Create and switch to branch
git switch branch-name           # Switch to branch (newer syntax)
git switch -c branch-name        # Create and switch (newer syntax)
git branch -d branch-name        # Delete branch
git branch -D branch-name        # Force delete branch
```

### Remote Operations
```bash
git remote -v                    # Show remote repositories
git remote add origin url        # Add remote repository
git fetch origin                 # Download changes without merging
git pull origin main             # Fetch and merge
git push origin main             # Push to remote
git push -u origin branch-name   # Push and set upstream
git push --force-with-lease      # Safe force push
```

## Common Git Scenarios and Solutions

### Scenario 1: Made changes but want to discard them
```bash
# Discard unstaged changes to specific file
git checkout -- filename

# Discard all unstaged changes
git checkout .

# Remove untracked files
git clean -f

# Remove untracked files and directories
git clean -fd
```

### Scenario 2: Wrong commit message
```bash
# Change last commit message
git commit --amend -m "New message"
```

### Scenario 3: Forgot to add files to last commit
```bash
# Add forgotten files and amend commit
git add forgotten-file.txt
git commit --amend --no-edit
```

### Scenario 4: Want to uncommit last commit but keep changes
```bash
# Soft reset - keeps changes staged
git reset --soft HEAD~1

# Mixed reset - keeps changes unstaged (default)
git reset HEAD~1

# Hard reset - discards changes (BE CAREFUL!)
git reset --hard HEAD~1
```

## Troubleshooting Common Git Problems

### Problem: Repository size too large (like your current issue)

**Symptoms:** `pack exceeds maximum allowed size` or `size-pack` > 2GB

**Solutions:**
1. **Find large files:**
   ```bash
   # Find largest objects in repo
   git rev-list --objects --all | git cat-file --batch-check='%(objecttype) %(objectname) %(objectsize) %(rest)' | sed -n 's/^blob //p' | sort --numeric-sort --key=2 | tail -10
   
   # Check current large files
   find . -type f -size +50M -not -path "./.git/*"
   ```

2. **Remove large files from history:**
   ```bash
   # Using BFG (recommended)
   java -jar bfg.jar --strip-blobs-bigger-than 50M
   git reflog expire --expire=now --all && git gc --prune=now --aggressive
   
   # Or remove specific files
   git filter-branch --force --index-filter 'git rm --cached --ignore-unmatch large-file.zip' --prune-empty --tag-name-filter cat -- --all
   ```

3. **Use Git LFS for legitimate large files:**
   ```bash
   git lfs install
   git lfs track "*.zip" "*.exe" "*.bin"
   git add .gitattributes
   ```

### Problem: Merge conflicts

**Symptoms:** Git shows conflict markers in files after merge/pull

**Solution:**
1. Open conflicted files and look for markers:
   ```
   <<<<<<< HEAD
   Your changes
   =======
   Their changes
   >>>>>>> branch-name
   ```

2. Edit files to resolve conflicts (remove markers, keep desired changes)

3. Stage and commit:
   ```bash
   git add resolved-file.txt
   git commit -m "Resolve merge conflict"
   ```

### Problem: Accidentally committed sensitive information

**Solutions:**
1. **If not yet pushed:**
   ```bash
   # Remove from last commit
   git reset --soft HEAD~1
   git reset HEAD sensitive-file.txt
   echo "sensitive-file.txt" >> .gitignore
   git add .
   git commit -m "Remove sensitive file"
   ```

2. **If already pushed:**
   ```bash
   # Remove from entire history
   git filter-branch --force --index-filter 'git rm --cached --ignore-unmatch sensitive-file.txt' --prune-empty --tag-name-filter cat -- --all
   git push --force-with-lease origin main
   ```

### Problem: Detached HEAD state

**Symptoms:** Git says you're in "detached HEAD" state

**Solution:**
```bash
# Create a branch from current state
git checkout -b recovery-branch

# Or go back to main branch
git checkout main
```

### Problem: Can't switch branches due to uncommitted changes

**Solutions:**
1. **Commit the changes:**
   ```bash
   git add .
   git commit -m "WIP: temporary commit"
   ```

2. **Stash the changes:**
   ```bash
   git stash
   git checkout other-branch
   # Later, when back on original branch:
   git stash pop
   ```

### Problem: Wrong files in .gitignore still being tracked

**Solution:**
```bash
# Remove from tracking but keep locally
git rm --cached filename
git commit -m "Remove tracked file now in .gitignore"
```

### Problem: Want to undo a pushed commit

**Solutions:**
1. **Create a revert commit (safe for shared repos):**
   ```bash
   git revert HEAD
   git push origin main
   ```

2. **Reset and force push (dangerous for shared repos):**
   ```bash
   git reset --hard HEAD~1
   git push --force-with-lease origin main
   ```

### Problem: Lost commits after reset

**Solution:**
```bash
# Find lost commits
git reflog

# Recover lost commit (replace abc1234 with actual hash)
git checkout abc1234
git checkout -b recovery-branch
```

## Emergency Commands

### When everything goes wrong:
```bash
# See what Git has been doing
git reflog

# Go back to any previous state
git reset --hard HEAD@{2}  # Go back 2 steps in reflog

# Make a backup branch of current state
git branch backup-$(date +%Y%m%d-%H%M%S)
```

### Before doing anything destructive:
```bash
# Always create a backup branch first
git branch backup-before-cleanup

# Check repository size
git count-objects -vH

# See what will be affected
git log --oneline -10  # Last 10 commits
```

## Best Practices

1. **Commit often** with clear, descriptive messages
2. **Pull before pushing** to avoid conflicts
3. **Use branches** for features and experiments
4. **Review changes** with `git diff` before committing
5. **Add large files to .gitignore** or use Git LFS
6. **Create backup branches** before doing destructive operations
7. **Test your code** before committing
8. **Use meaningful commit messages** (not just "fix" or "update")

## Quick Reference Card

| Task | Command |
|------|---------|
| Check status | `git status` |
| Stage all changes | `git add .` |
| Commit | `git commit -m "message"` |
| Push | `git push origin main` |
| Pull latest | `git pull origin main` |
| Undo last commit (keep changes) | `git reset --soft HEAD~1` |
| Unstage all files | `git reset` |
| Discard all changes | `git checkout .` |
| Create branch | `git checkout -b branch-name` |
| Switch branch | `git checkout branch-name` |
| See history | `git log --oneline` |
| See what changed | `git diff` |

Remember: Git is powerful but forgiving. Most mistakes can be undone, and when in doubt, create a backup branch first!