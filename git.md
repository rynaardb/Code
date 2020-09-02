# Git

## Generate a new SSH key

```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"

pbcopy < ~/.ssh/id_rsa.pub
```

## Basics

```bash
# Show where git configs get defined
git config --show-origin -l

# Set username and email (global - for all repos)
git config --global user.name "Rynaard Burger"
git config --global user.email "ryno.burger@gmail.com"

# Set username and email (global - for current repo)
git config user.name "Rynaard Burger
git config user.email "ryno.burger@gmail.com

# Initialize a new repository
git init

# Add a remote
git remote add origin git@github.com:rynaardb/code.git

# Get log
git log

# Get status
git status

# Create new branch
git checkout -b

# New branch without git history & files
git checkout --orphan

# Checkout branch
git checkout <branch>

# List remote branches
git branch -a

# Staging files
git add . 
git add FILENAME

# Pushing changes
git push 
git push origin <branch>

# Fetching changes
git fetch
git fetch origin <branch>

# Pulling changes
git pull 
git pull origin <branch>

# Pull from master when working with forks:
git pull upstream <branch>
```

## Commit

```bash
# Commit with message
git commit -m "Commit message"

# Rename most recent commit
git commit --amend

# Undo last commit
git reset --soft HEAD~1

# Amend last commit message
git commit --amend -m "New commit message"
```

## Stashing

```bash
# Stash current changes
git stash

# Pop the last stashed changes
git stash pop
```

## Reset

```bash
# Undo last commit but don't throw away changes
git reset --soft HEAD^

# Reset to previous commit
git reset HEAD~

# Hard reset to a specific commit
reset --hard <commit hash>
# Typically followed by a forced push:
push --force origin <branch>
```

## Delete

```bash
# Delete branch
git branch -d

# Delete remote branch
git push origin --delete

# Delete commit from local repo
git reset --hard HEAD~1

# Delete commit from remote repo (can get commit using git log)
git push -f origin last_known_good_commit:branch_name
```

## Tags

```bash
# List all tags
git tag

# Tag a commit
git tag -a 1.0 -m "version 1.0"
```

## Force

```bash
# Force overwrite git repo
git push -f <remote> <branch>

# Force push overwrite
git push --force origin master
```

## Cherry Picking

```bash
# Cherry pick a specific commit
git cherry-pick <commit hash>

# Add some commits to the top of the current branch
git cherry-pick <commit a hash> <commit b hash>
```

## Create a diff patch

```bash
git diff > myDiffPatch.patch

# Generate a patch between two tags
git diff tag1..tag2 > myDiffPatch.patch

# Only shows the changes, does not apply the actual changes
git apply --stat myDiffPatch.patch

# Apply the patch
git apply myDiffPatch.patch
```

## Quickly pull down PRs

```bash
git config --global --add alias.pr '!f() { git fetch -fu ${2:-upstream} refs/pull/$1/head:pr/$1 && git checkout pr/$1; }; f'

git config --global --add alias.pr-clean '!git checkout master ; git for-each-ref refs/heads/pr/* --format="%(refname)" | while read ref ; do branch=${ref#refs/heads/} ; git branch -D $branch ; done'
```

## Misc

```bash
# Clean cache
git clean -dfx

# Find something in the history
git grep "someString"

# Show changes between commits. Where f5352 and be73 are unique commit hashes.
git diff f5352 be73

# Disable SSL verification
git -c http.sslVerify=false
```

