### git commands used in day to day activities
As a DevOps engineer, I use Git daily for version control, CI/CD workflows, infrastructure automation, and collaboration.
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

### Difference between git rebase and git merge
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

### What is the difference between git push --force-with-lease vs --force?
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
    Protects you from accidentally deleting someone else’s commits.
### How can you delete the last 2 git commits?

git reset --soft HEAD~2	Deletes commits, keeps changes staged
git reset HEAD~2	    Deletes commits, keeps changes unstaged
git reset --hard HEAD~2	Deletes commits and all changes permanently

### Git stash
git stash is a Git command used to temporarily save (stash) changes in your working directory that are not yet committed, so you can switch branches or perform other operations without losing your progress.

### difference between git fetch and git pull
Both git fetch and git pull are used to update your local repository from a remote, but they behave differently
1. git fetch
 git fetch downloads commits, files, and references from the remote, but it does not merge them into your current branch
2. git pull
    git pull is essentially git fetch + git merge.

    It downloads new commits and immediately tries to merge them into your current branch.
How will you resolve the git conflict automatically?