# 1. What is version control and why is Git used?

Version control is a system that tracks changes in files over time.
Git is used to:

- Track code changes
- Collaborate with others
- Roll back to previous versions
- Manage different versions using branches

# 2. Difference between Git and GitHub?

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

# 3. What is a repository?

- A repository is a centralized storage location for project files, code, data, Git history (commits, branches, config), and other assets.

- It is initialized using git init or downloaded using git clone.

# 4. What is a commit?

- A commit is a snapshot of the project at a point in time, along with a message describing what changed.

# 5. What is a branch?

- A branch is a separate line of development used to work on features or fixes independently from the main code.

# 6. What is HEAD in Git?

- HEAD points to the current branch and the latest commit you are working on.

# 7. Difference between git fetch and git pull?

- git fetch → downloads changes but does not merge.

- git pull → downloads and merges into your branch.

# 8. What is a merge conflict?

- A merge conflict in version control (like Git) happens when two different branches have conflicting changes to the same part of a file, and the system can't automatically decide which change to keep, requiring a developer to manually resolve it.

# 9. What is .gitignore?

- The .gitignore file is a plain text configuration file used by Git, A file that tells Git which files or folders should NOT be tracked (like node_modules, .env).

# 10. Difference between clone and fork?

- Clone → local copy of a repo.

- Fork → copy of someone else's repo on your GitHub account.

# 11. Command -

- Initialize a new Git repo -> git init
- Add a file to staging -> git add file.txt or git add . (all)
- Commit changes -> git commit -m "messages"
- Check current status -> git status
- View commit history - git log
- Create and switch to new branch -> git checkout -b feature
- Delete a branch -> git branch -d feature
- Push code to GitHub -> git push origin main
- Pull latest changes -> git pull origin main
- Undo last commit ->
  - Keep changes: git reset --soft HEAD1
  - Remove changes: git reset --hard HEAD1
- Recover a deleted file -> git checkout HEAD -- filename

# 12. You committed a secret key — what do you do?

- Remove it from code
- Add to .gitignore
- Rewrite history using git filter-branch or git reset
- Rotate the secret immediately

# 13. Two people changed the same line — what happens?

- Git raises a merge conflict and asks you to manually resolve it.

# 14. When use rebase instead of merge?

- Use rebase for a cleaner, linear history before merging a feature branch. 

# 15. Keep fork updated

- git remote add upstream <original-repo>
- git fetch upstream
- git merge upstream/main