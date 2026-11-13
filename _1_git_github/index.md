## Git and GitHub

### 1. What is version control and why is Git used?

Version control is a system that tracks changes in files over time.
Git is used to:

- Track code changes
- Collaborate with others
- Roll back to previous versions
- Manage different versions using branches

### 2. Difference between Git and GitHub?

Git
- Tool/software
- Runs locally
- Tracks versions
- Command-line based

GitHub
- Online platform
- Runs on cloud
- Hosts Git repositories
- Web UI + collaboration

### 3. What is a repository?

A repository stores the project history, objects and branch/tag references. Git is distributed, so each clone can have its own repository. A normal repo has a working folder and usually a .git folder. A bare repo has no working folder.

Use `git init` to start one and `git clone` to copy an existing one.

### 4. What is a commit?

- A commit is a snapshot of the project at a point in time, along with a message describing what changed.

### 5. What is a branch?

- A branch is a separate line of development used to work on features or fixes independently from the main code.

### 6. What is HEAD in Git?

HEAD normally points to the branch currently checked out. In detached HEAD state, it points directly to a commit.

### 7. Difference between git fetch and git pull?

`git fetch` downloads changes and updates remote-tracking branches. It does not add those changes to the current branch.

`git pull` fetches and then adds the changes using merge, rebase or fast-forward-only, based on the options and configuration.

```bash
git pull --ff-only
git pull --rebase
git pull --no-rebase
```

### 8. What is a merge conflict?

A merge conflict happens when Git cannot combine changes automatically. It can happen with edits, renames or a file deleted on one branch and changed on another.

Check the files, resolve the conflicts, stage them, run tests and continue. To cancel the merge, use abort.

```bash
git status
git add FILE
git merge --continue
# To cancel the merge instead:
git merge --abort
```

### 9. What is .gitignore?

.gitignore tells Git which untracked files to ignore, like .env or build files. It does not stop tracking a file already committed.

To keep .env locally but remove it from the next commit:

```bash
git rm --cached .env
# Add .env to .gitignore, then commit both changes.
```

If it contains a real secret, rotate the secret too. Ignoring the file does not remove its old copies.

### 10. Difference between clone and fork?

- Clone → local copy of a repo.

- Fork → copy of someone else's repo on your GitHub account.

### 11. Command -

Check `git status` and save important work before undo commands. Practise destructive commands in a test repo. Use the actual branch name; `main` below is only an example.

```bash
# Start a repo
git init
# Stage one file
git add file.txt
# Stage changes below the current folder
git add .
# Stage changes across the whole repo
git add -A
# Commit and inspect
git commit -m "message"
git status
git log
# Create and switch branch
git checkout -b feature
# Delete a local branch
git branch -d feature
# Push and pull, using an example main branch
git push origin main
git pull --ff-only origin main
# Undo the last unpublished commit, keeping changes staged
git reset --soft HEAD~1
# Undo it, keeping files but clearing staging
git reset --mixed HEAD~1
# Destructive: also resets working files; can lose local work
git reset --hard HEAD~1
# Restore an unstaged deletion
git restore -- FILE
# If deletion was staged, unstage first, then restore
git restore --staged -- FILE
git restore -- FILE
```

`HEAD~1` means the first parent of HEAD. For a bad commit already shared with the team, use `git revert COMMIT` instead of removing it from history.

### 12. You committed a secret key — what do you do?

Revoke or rotate the secret first. Then remove it from the code and stop tracking the secret file. Add the ignore rule and check where the secret was exposed.

For pushed history, coordinate with the team and follow GitHub's cleanup steps using git-filter-repo. A local reset does not clean remote copies, clones or forks. Deleting the file does not make the leaked secret safe.

### 13. Two people changed the same line — what happens?

A conflict may happen when the branches are combined. It is not guaranteed just because both people changed the same line. Identical changes can merge cleanly.

Resolve the conflict, stage the files, run tests and continue the merge or rebase. Use abort to cancel it.

### 14. When use rebase instead of merge?

Rebase is useful for cleaning up a feature branch before sharing it. It reapplies commits on a new base and usually gives them new commit IDs. Do not rebase shared commits without team agreement.

Merge keeps the existing commits. A fast-forward merge only moves the branch pointer.

After a rebase conflict, fix the files, stage them and run `git rebase --continue`. Use `git rebase --abort` to cancel.

### 15. Keep fork updated

Add the original repo as upstream, fetch its changes and merge its actual default branch. For example, if that branch is master:

```bash
git remote add upstream <original-repo-url>
git fetch upstream
git switch master
git merge upstream/master
```

Save local work first. Do not assume every repo uses main.

### 16. Working tree, staging area and commit: what is the difference?

The working tree is where files are edited. The staging area, or index, holds the next snapshot to commit. A commit saves that snapshot with its details.

If a file is changed again after `git add`, that new change must be staged again to include it.

### 17. How do you inspect unstaged and staged changes?

`git diff` shows unstaged changes. `git diff --staged` shows staged changes compared with HEAD. `git diff HEAD` compares tracked working files with HEAD. These commands do not show newly untracked files.

```bash
git status
git diff
git diff --staged
```

### 18. How do you unstage a file without deleting your edits?

Use `git restore --staged`. It removes the change from staging but keeps the working file.

```bash
git restore --staged -- FILE
```

Be careful: `git restore FILE` without --staged normally discards unstaged changes by restoring the file from the index.

### 19. When do you use stash? apply vs pop?

Stash saves local work temporarily. By default, it saves tracked changes. Use -u for untracked files and -a for ignored files too.

`apply` restores the changes and keeps the stash. `pop` restores and removes it only if that succeeds. On a conflict, the stash stays. `--index` also tries to restore what was staged.

```bash
git stash push -u -m "WIP before hotfix"
git stash list
git stash apply
```

### 20. What is a remote? origin vs upstream vs origin/main?

A remote is a saved name for a repo URL. `origin` is normally created by clone. In a fork, `upstream` often means the original repo.

`origin/main` is a local record of the remote branch from the last fetch. It is not a live view. A branch's upstream tracking setting is separate from a remote named upstream.

### 21. How do you publish a new branch and set tracking?

Use push with -u. It sets the tracking branch for later push/pull commands. Check it with `git branch -vv`.

```bash
git push -u origin feature
git branch -vv
```

### 22. Reset, restore or revert: how do you choose?

Use restore for files or staging. Use reset to move a branch tip or reset staged entries. Use revert to make a new commit that reverses an old commit.

For a normal bad commit already shared with the team, revert keeps the history and is usually the safer choice.

```bash
git revert COMMIT
```

### 23. What is reflog? How do you recover a lost commit?

Reflog records local branch and HEAD movements. Find the old commit, inspect it and make a recovery branch.

```bash
git reflog
git show COMMIT
git branch recovered COMMIT
```

Reflog is local, not a backup. Entries expire and old objects can be removed, so recovery is not always possible.

### 24. What is detached HEAD, and how do you keep work made there?

Detached HEAD means HEAD points directly to a commit instead of a branch. New commits can be made, but no branch moves to keep them.

Before leaving, run `git switch -c keep-work`. If the commit was already left behind, find it in reflog and create a branch at that commit.

### 25. What does cherry-pick do?

Cherry-pick applies one selected commit's change to the current branch, normally as a new commit. It is useful for taking one hotfix without merging the full feature branch.

```bash
git cherry-pick COMMIT
# After resolving and staging conflicts:
git cherry-pick --continue
# Or cancel:
git cherry-pick --abort
```

### 26. How do you change the last commit?

Use amend to change the last commit's message or add staged changes. It replaces that commit with a new one. Use it for unpublished work; do not change shared history without agreement.

```bash
git commit --amend -m "Clearer commit message"
# After staging extra changes:
git commit --amend --no-edit
```

### 27. Why is a push rejected as non-fast-forward?

The remote branch has commits missing from the local branch. Fetch, inspect the changes and merge or rebase using the team's workflow. Then test and push. Do not force-push just to remove the error.

### 28. --force vs --force-with-lease: what is safer?

`--force` can overwrite remote history. `--force-with-lease` checks that the remote branch still matches the expected commit before updating it.

It is safer, but it can still replace work. Background fetches can change the expected local tracking value. Use it only for an agreed history rewrite; giving the exact expected commit makes the check stronger.

### 29. What is a tag, and how is it different from a branch?

A branch normally moves as new commits are made. A tag normally stays on one version. A lightweight tag is a reference. An annotated tag also stores a message, tagger and date, and can be signed.

```bash
git tag -a v1.0.0 -m "Release 1.0.0"
git push origin v1.0.0
```

### 30. How do you combine small WIP commits before review?

Use interactive rebase for unpublished commits. For the last three commits, run `git rebase -i HEAD~3`. Keep the first as pick and mark later commits as squash or fixup.

This changes commit IDs. Do not use it on shared history without agreement.

### 31. A bug appeared across many commits. How do you find its introduction?

Use bisect with a known good and bad commit. Git checks commits in between. Run the test and mark each one good or bad until the first bad commit is found. The test must give a reliable result.

```bash
git bisect start
git bisect bad
git bisect good GOOD_COMMIT
# Test each commit, then run git bisect good or git bisect bad
git bisect reset
```

### 32. What should a safe feature-to-review workflow look like?

Start from an updated base and create a feature branch. Make focused commits, check staged changes, run tests and push. Open a pull request for review and checks.

Bring in main/master changes using the team's merge or rebase workflow. Keep secrets out of commits and do not rewrite teammates' work. A pull request is a hosting-platform review step, not a Git commit type.

### 33. Does git branch -d delete the remote branch too?

No. `git branch -d` deletes a local branch and normally checks that its work was merged. Remote deletion is a separate command.

```bash
git branch -d feature
# Delete the remote branch only after team agreement:
git push origin --delete feature
```

`git fetch --prune` removes stale local remote-tracking references. It does not delete ordinary local branches.