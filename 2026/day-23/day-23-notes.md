# Day 23 – Git Branching & Working with GitHub

---

## Task 1: Understanding Branches

### 1. What is a branch in Git?

A branch in Git is a lightweight pointer to a commit.  
It represents an independent line of development with its own commit history. Branches are used to:
- Develop new features safely
- Fix bugs without affecting stable code
- Allow multiple developers to work in parallel
- Experiment and discard changes if needed

**Common commands:**
bash
git branch branch-name        # Create a new branch
git switch branch-name        # Switch between branches
git checkout -b branch-name   # Create and switch to a branch
2. Why do we use branches instead of committing everything to main?

The main branch contains stable, production-ready code.
Using branches allows developers to:

Work on features without breaking production

Test code safely

Review changes before merging

Keep the main branch clean and stable

Typical workflow:

Create a branch from main

Develop and test changes

Push the branch to GitHub

Merge into main after review

3. What is HEAD in Git?

HEAD is a pointer to the latest commit of the currently active branch.

Example:

HEAD → feature-1 → commit-id

When you create a new commit, HEAD automatically moves to that commit.

Detached HEAD

A detached HEAD happens when you checkout a specific commit instead of a branch.
This is useful for:

Debugging old versions

Testing previous commits

Finding when a bug was introduced

No branch is updated in this state.

4. What happens to your files when you switch branches?

When switching branches:

Tracked files update to match the target branch

Files unique to the previous branch disappear

Common files remain unchanged

Committed changes stay safe

Uncommitted changes may block switching if there are conflicts

Untracked files usually remain unless there's a name conflict

Git ensures committed data is never lost.

Task 2: Branching Commands — Hands-On

Commands performed in the repository:

git branch                  # List all branches
git branch feature-1         # Create feature-1
git switch feature-1         # Switch to feature-1
git checkout -b feature-2    # Create and switch to feature-2
Difference between git checkout and git switch
Feature	git checkout	git switch
Purpose	Multiple operations	Switch branches only
Clarity	Can be confusing	Clear and simple
Safety	Less safe	Safer
Create branch	checkout -b	switch -c
Restore files	Yes	No
Git version	All versions	Git 2.23+

Delete a branch:

git branch -d branch-name
git branch -D branch-name
Task 3: Push to GitHub
Difference between origin and upstream

Origin

Points to your own GitHub repository

Used for pushing your changes

Upstream

Points to the original repository you forked from

Used to fetch updates

Commands:

git push origin main
git fetch upstream
Task 4: Pull from GitHub

Steps:

Edited a file directly on GitHub

Pulled the change locally:

git pull origin main
Difference between git fetch and git pull

git fetch

Downloads changes only

Does not modify working files

Safer and more controlled

git pull

Downloads and merges changes

May cause merge conflicts

Example workflow:

git fetch origin main
git diff main origin/main
git merge origin/main
Task 5: Clone vs Fork
Difference between clone and fork

Fork

Creates a copy of a repository on your GitHub account

Used when contributing to another project

Clone

Copies a repository to your local machine

Used for local development

When to use clone vs fork

Clone your own repository

Fork someone else’s repository before contributing

Keeping a fork in sync with the original repository
- git clone https://github.com/your-username/project.git
- git remote add upstream https://github.com/original-owner/project.git
- git fetch upstream
- git merge upstream/main
- git push origin main
