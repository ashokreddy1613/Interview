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