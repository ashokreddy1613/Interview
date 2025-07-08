# Git Interview Questions and Answers

## Q1: What are the git commands used in day to day activities?
As a DevOps engineer, I use Git daily for version control, CI/CD workflows, infrastructure automation, and collaboration.
```bash
git clone <repo-url>
git pull origin <branch>
git checkout -b feature/xyz
git checkout main
git branch -d old-branch
git push origin <branch>
git merge <branch>
git rebase main
git status
git diff
git log --oneline --graph
```

## Q2: What is the difference between git rebase and git merge?
Git merge:
```bash
git checkout feature
git merge main
```
Merges `main` into `feature`, adds a merge commit, and keeps both histories.

Git Rebase:
```bash
git checkout feature
git rebase main
```
Replays feature commits on top of main, creating a clean linear history.

`git merge` is safer and preserves history with a merge commit, making it ideal for shared branches. `git rebase` creates a cleaner, linear history by rewriting commits — perfect for feature branches before merging via pull request. I typically use rebase in local development and merge during collaborative work or CI/CD integration.

## Q3: What is the difference between git push --force-with-lease vs --force?
Both git push --force and git push --force-with-lease are used to forcefully update a remote branch, typically after rewriting history (e.g., rebasing or squashing commits).
The key difference is safety.

`git push --force`
This command blindly overwrites the remote branch, regardless of whether someone else has pushed changes after your last pull.

⚠️ Risk:
It can overwrite others' work if changes were pushed by teammates after your last pull — destructive in team environments.

git push --force-with-lease ✅ Recommended
This command is safer. It tells Git:

"Only force-push if the remote branch hasn't changed since I last fetched it."

If someone else has pushed to the branch after your last fetch/pull, the push fails.
Protects you from accidentally deleting someone else's commits.

## Q4: How can you delete the last 2 git commits?
```bash
git reset --soft HEAD~2    # Deletes commits, keeps changes staged
git reset HEAD~2          # Deletes commits, keeps changes unstaged
git reset --hard HEAD~2   # Deletes commits and all changes permanently
```

## Q5: What is git stash?
git stash is a Git command used to temporarily save (stash) changes in your working directory that are not yet committed, so you can switch branches or perform other operations without losing your progress.

## Q6: What is the difference between git fetch and git pull?
Both git fetch and git pull are used to update your local repository from a remote, but they behave differently:

1. git fetch
   git fetch downloads commits, files, and references from the remote, but it does not merge them into your current branch

2. git pull
   git pull is essentially git fetch + git merge.
   It downloads new commits and immediately tries to merge them into your current branch.

## Q7: How and from where to clone repo, is there any local repo you are using and then transferring from local to remote or how?
In my workflow, I usually clone repositories directly from a remote Git server such as GitHub, GitLab, or Azure Repos, depending on the organization.

## Q8: What is PAT (Personal Access Token)?
PAT (Personal Access Token) is a secure, token-based alternative to passwords used to authenticate with Git hosting platforms like GitHub, GitLab etc.

## Q9: How to handle merge conflict in git? If 2 people working on same file and did the commit and got conflict error, in how many ways can it be solved?
Merge conflicts occur when git can't automatically resolve changes to the same part of file. I usually resolve them manually or using a merge tool, and then staging and committing the resolved file.

There are multiple ways to resolve:
- Keep current (ours)
- Keep incoming (theirs)
- Merge both manually
- Use a visual merge tool

## Q10: What is the use of Git tags?
1. **Marking Releases**
   - Git tags are commonly used to mark stable versions of code

2. **Versioning**
   - Helps associate a Git commit with a semantic version

Git tags are used to mark specific commits, typically for versioned releases like v1.0.0. Unlike branches, tags don't change and are ideal for pointing to stable snapshots of code.

We use tags in our CI/CD pipelines to:
- Trigger deployments
- Track production versions
- Simplify rollbacks

I prefer annotated tags for releases since they include metadata like author and message.

## Q11: What are the different types of branches in Git?
Usually, we have:
- **Long live branches**:
  - Master/main
  - Develop
- **Short live branches**:
  - Feature
  - Hotfixes

## Q12: How will you resolve the git conflict automatically?
To resolve git conflicts automatically, you can use:
1. Git's built-in merge strategies
2. Automated merge tools
3. CI/CD pipeline conflict resolution scripts
4. Git hooks for pre-merge conflict detection

## Git submodules, what is their purpose
Git submodules are a way to include and manage one Git repository as a subdirectory within another Git repository. They allow you to track an external project at a specific commit, making it easier to reuse shared code or libraries across multiple projects without copying or duplicating."

## what is Stale branch.
A stale branch in Git refers to a branch that hasn't been updated or used in a long time. It usually lags behind the main development branch (like main or master) and may contain outdated or obsolete code. Stale branches are often candidates for cleanup to keep the repository clean and maintainable.

## git cherrypick
git cherry-pick is used to apply one or more specific commits from one branch onto your current branch — without merging the whole branch.

***git cherry-pick <commit-hash>***

## git stash
git stash is used to temporarily set aside your uncommitted changes (both staged and unstaged) so you can work on something else (e.g., switch branches), and then bring them back later.
```bash
git stash  # Saves all modifications and restores your working directory to the last commit

git stash apply # To apply and remove the stash in one step
```
## git fork vs git clone

***git clone*** creates a local copy of an existing remote repository.

- Downloads the entire history and branches
- You can push to it only if you have write access
- Used for personal or team repos you already have access to

A ***fork*** is a server-side copy of a repository made under your account (on GitHub, GitLab, etc.)

You do this by clicking "Fork" on GitHub

Creates a new remote repo: https://github.com/your-username/original-repo

## What is the purpose of the .git folder in a Git repository?
The .git folder contains all the metadata, configuration, and object database Git needs to manage version control. It is what transforms an ordinary directory into a Git repository.
Why It’s Important:
Without the .git folder, your project is no longer a Git repository.
If you delete or corrupt it, Git can no longer track changes.
If you copy the .git folder into another directory, you essentially clone the repo without using a remote.

## Git squash
Git squash is a way to combine (or "squash") several commits into a single commit. It helps you keep your commit history clean and meaningful, especially before merging a feature branch.

Use interactive rebase:

```bash

git checkout feature-branch
git rebase -i main
```
Mark commits as squash or s except for the first one.

## How do you remove a remote branch in Git?

Answer: Use git push <remote-name> --delete <branch-name>.

```bash

git push origin --delete feature-branch
```

## Explain git stash apply vs git stash pop.

git stash apply re-applies stashed changes but keeps them in the stash, while git stash pop applies and removes them from the stash.

## What is the difference between git reset and git revert?

git reset moves the branch pointer and changes history, while git revert creates a new commit to undo changes.

## Explain shallow cloning in Git.

Shallow cloning (-depth=1) clones only the latest history.
```bash

git clone --depth=1 https://github.com/repo.git
```
## How do you fix a commit with the wrong message after pushing?
 ```bash
 
 git commit --amend -m "New message"
 git push --force
 
 ```
 ## How do you squash commits using Git?

Use interactive rebase with git rebase -i HEAD~n and mark commits as squash.

## Scenario 1: Undoing a Pushed Commit Without Affecting Other Users' History

You pushed a faulty commit to a shared branch. How can you fix the mistake without rewriting history?

Use git revert to create a new commit that negates the previous one.

Example:
```bash

git revert <commit-hash>
git push origin main
```
git revert creates a new commit that undoes the changes of the specified commit. This approach is safe for shared branches because it preserves the history.

## Scenario 2: Accidental Deletion of a Branch

You accidentally deleted a local branch. How do you recover it?

If the branch has been pushed to the remote, you can fetch it. If not, find the commit hash in the reflog and recreate it.

Example:
```bash

git reflog
git checkout -b recovered-branch <commit-hash>
```

## Scenario 3: Resolving a Conflict While Rebasing

During a rebase, you encounter conflicts. How do you resolve them and continue the rebase?
```bash

git status  # Shows conflicting files
git add resolved-file.txt
git rebase --continue
```