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

A Git repository stores versioned objects and refs. Git is distributed, so a repository is not inherently centralized. A normal repository usually has metadata in .git and a working tree beside it; a bare repository has no working tree.

Initialize one using `git init`, or copy an existing repository using `git clone`.

### 4. What is a commit?

- A commit is a snapshot of the project at a point in time, along with a message describing what changed.

### 5. What is a branch?

- A branch is a separate line of development used to work on features or fixes independently from the main code.

### 6. What is HEAD in Git?

HEAD normally refers to the checked-out branch. In detached HEAD state, it points directly to a commit instead.

### 7. Difference between git fetch and git pull?

`git fetch` downloads objects and updates remote-tracking refs without integrating them into the current branch.

`git pull` fetches and then integrates according to options/configuration: merge, rebase or fast-forward-only. Make the choice explicit.

```bash
git pull --ff-only
git pull --rebase
git pull --no-rebase
```

### 8. What is a merge conflict?

A merge conflict occurs during integration when Git cannot reconcile changes automatically. It can involve competing edits, delete/modify cases or renames. Inspect `git status`, resolve files, stage the resolutions, test and continue.

```bash
git status
git add FILE
git merge --continue
# To abandon the in-progress merge instead:
git merge --abort
```

### 9. What is .gitignore?

The .gitignore file specifies intentionally untracked files Git should ignore, such as local .env files or build outputs. It does not stop tracking files already committed.

To keep a local .env but remove it from the next committed snapshot:

```bash
git rm --cached .env
# Add .env to .gitignore, then commit both changes.
```

If it held a real secret, rotate/revoke it too; ignoring it does not remove historical exposure.

### 10. Difference between clone and fork?

- Clone → local copy of a repo.

- Fork → copy of someone else's repo on your GitHub account.

### 11. Command -

Practice undo/recovery commands in a disposable repository. Inspect status and save important work first. `main`, `feature`, `FILE` and `COMMIT` are examples; use your actual branch names. This notes snapshot uses `master`.

```bash
# Initialize, stage, commit and inspect
git init
git add file.txt
# Stage changes below the current directory:
git add .
# Stage across the whole working tree:
git add -A
git commit -m "message"
git status
git log

# Branch operations
git checkout -b feature
git branch -d feature

# Remote operations, with an example main branch
git push origin main
git pull --ff-only origin main

# Undo an unpublished tip commit; HEAD~1 is the first parent
# Keep staged content and working files:
git reset --soft HEAD~1
# Keep working files, reset staging:
git reset --mixed HEAD~1
# DESTRUCTIVE: resets index and working tree; may destroy work
git reset --hard HEAD~1

# Restore an unstaged deletion of a tracked file:
git restore -- FILE
# If deletion was staged, unstage first, then restore:
git restore --staged -- FILE
git restore -- FILE
```

Prefer `git revert COMMIT` for a faulty normal commit already shared with the team. Reset moves branch history; it is not the same as a safe shared-history undo.

### 12. You committed a secret key — what do you do?

Revoke or rotate the credential FIRST. Remove it from current code and stop tracking secret files. Add suitable ignore rules, assess exposure and coordinate with collaborators.

If historical cleanup is needed, follow GitHub guidance using git-filter-repo. A local reset is not complete cleanup of pushed history, clones, forks or cached views. Do not use filter-branch as the default response. Never assume deleting the file removes the leaked credential.

### 13. Two people changed the same line — what happens?

Conflicting changes may cause a conflict when the branches are integrated, but editing the same line does not guarantee one. Identical edits can merge cleanly. Resolve and stage conflicts, test, then continue the merge or rebase; abort if the integration is unwanted.

### 14. When use rebase instead of merge?

Use rebase to reapply your own feature commits on a new base and get a cleaner linear history. It usually creates new commit IDs. Do not rewrite shared commits without team agreement.

Merge combines histories while preserving existing commits; a fast-forward merge only advances the branch pointer.

For a rebase conflict, resolve files, stage them and use `git rebase --continue`; use `git rebase --abort` to return to the pre-rebase state.

### 15. Keep fork updated

Use the original repository's actual default branch rather than assuming main. For example, if the original branch is master:

```bash
git remote add upstream <original-repo-url>
git fetch upstream
git switch master
git merge upstream/master
```

Inspect local changes and branch state before integrating; use your team's merge/rebase policy.

### 16. Working tree, staging area and commit: what is the difference?

The working tree is the files I edit. The staging area (index) holds the exact next snapshot I plan to commit. A commit records that snapshot and metadata. If I edit a file again after git add, the later edit is not included until I stage it again.

### 17. How do you inspect unstaged and staged changes?

git diff compares working-tree changes with the index. git diff --staged compares the index with HEAD. git diff HEAD shows the difference between tracked working-tree content and HEAD. Newly untracked files are not shown by these ordinary diff forms.

```bash
git status
git diff
git diff --staged
```

### 18. How do you unstage a file without deleting your edits?

Use git restore --staged. This resets that index entry to HEAD while leaving the working file unchanged. By contrast, git restore FILE normally discards unstaged edits by restoring from the index.

```bash
git restore --staged -- FILE
```

### 19. When do you use stash? apply vs pop?

Stash temporarily saves tracked local changes. Use -u to include untracked files; ignored files need -a. apply restores a stash and keeps it. pop restores and drops it only if application succeeds; on conflict the stash remains. --index attempts to restore the original staged state too.

```bash
git stash push -u -m "WIP before hotfix"
git stash list
git stash apply
```

### 20. What is a remote? origin vs upstream vs origin/main?

A remote is a named repository URL. origin is a convention, normally created by clone. In a fork workflow upstream often names the original repo. origin/main is a local remote-tracking ref reflecting the last fetched state, not a live view. A branch's configured upstream is a tracking relationship and need not be a remote named upstream.

### 21. How do you publish a new branch and set tracking?

Push the branch with -u to establish its upstream relationship. Later pull/push behavior can use that configuration; inspect it with git branch -vv. The branch need not have the same name as the default branch.

```bash
git push -u origin feature
git branch -vv
```

### 22. Reset, restore or revert: how do you choose?

Use restore for file content or staging changes. Use reset when moving your own branch tip or resetting index entries. Use revert to create a new commit that reverses an earlier commit without removing it from history. For a normal bad commit on shared main, revert is usually the safer choice.

```bash
git revert COMMIT
```

### 23. What is reflog? How do you recover a lost commit?

Reflog records local ref movements, including branch switches in HEAD's reflog. Find the old commit, inspect it and create a recovery branch before changing anything else. It is local, not a remote backup; entries expire and unreachable objects can be pruned, so recovery is not guaranteed.

```bash
git reflog
git show COMMIT
git branch recovered COMMIT
```

### 24. What is detached HEAD, and how do you keep work made there?

HEAD points to a commit directly rather than to a branch. You can commit, but no branch advances to retain those commits. While still at the work you want to keep, create a branch with git switch -c keep-work. If you already left, look up the commit in reflog and create a branch at it.

### 25. What does cherry-pick do?

It applies the change introduced by a selected commit onto the current branch, normally recording a new commit. Use it to bring one isolated hotfix to a release branch without merging all feature work. It can conflict; resolve and stage the files, then continue, or abort.

```bash
git cherry-pick COMMIT
git cherry-pick --continue
# Or: git cherry-pick --abort
```
### 26. How do you change the last commit?

Use commit --amend for a message change or to include newly staged content. It replaces the tip commit with a new one. It is suitable for your own unpublished commit; avoid changing already-shared history without agreement.

```bash
git commit --amend -m "Clearer commit message"
# After staging additional changes:
git commit --amend --no-edit
```

### 27. Why is a push rejected as non-fast-forward?

The remote has commits not included in my branch. I fetch, inspect both histories and integrate with merge or rebase according to team policy, test, then push. I do not immediately force-push. A rejection may protect a teammate's work.

### 28. --force vs --force-with-lease: what is safer?

--force can overwrite remote history without the usual ancestry protection. --force-with-lease requires the remote ref to match an expected value and rejects otherwise. It is safer, not harmless: implicit expectations can be changed by background fetches. Use only on an agreed rewritten branch; an explicit expected commit gives a stronger check.

### 29. What is a tag, and how is it different from a branch?

A branch tip normally advances as I commit. A tag normally stays at a named version. A lightweight tag is a ref; an annotated tag has a tag object with tagger, date and message and can be signed. Use annotated tags for releases; push the specific tag when ready.

```bash
git tag -a v1.0.0 -m "Release 1.0.0"
git push origin v1.0.0
```

### 30. How do you combine small WIP commits before review?

For my own unpublished commits, interactive rebase lets me reorder, reword and squash/fixup commits. For the last three commits, start with git rebase -i HEAD~3, keep the first as pick, then squash or fixup later commits. This rewrites commit IDs; do not apply it casually to shared history.

### 31. A bug appeared across many commits. How do you find its introduction?

Use bisect with a known bad commit and a known good commit. Git checks candidate commits using binary search; run the relevant test and mark each good or bad. Repeat until the introducing commit is identified, then end the bisect session. The test must be reliable.

```bash
git bisect start
git bisect bad
git bisect good GOOD_COMMIT
# Test candidates; mark: git bisect good / bad
git bisect reset
```

### 32. What should a safe feature-to-review workflow look like?

Start with a current base, create a feature branch, make focused commits, inspect staged diffs, run tests and push the feature branch. Open a pull request for review and checks. Integrate main/master changes using the team's merge/rebase policy. Do not commit secrets or rewrite teammates' work. A pull request is a hosting-platform review workflow, not a Git commit type.

### 33. Does git branch -d delete the remote branch too?

No. It deletes a local branch, normally with a merged-work safety check. Remote branch deletion is a separate push operation. git fetch --prune removes stale local remote-tracking refs for deleted remote branches; it does not delete your ordinary local branches.

```bash
git branch -d feature
# Remote deletion is separate; confirm team intent:
git push origin --delete feature
```