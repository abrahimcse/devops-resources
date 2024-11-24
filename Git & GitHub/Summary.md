# Commands Summary

### 1. Initialize a new Git repository
```
git init my-repo
cd my-repo
```
### 2. Create an initial commit
```
echo "Initial commit" > README.md
git add README.md
git commit -m "Initial commit"
```
### 3. Configure Git user details
```
git config --global user.name "Your Name"
git config --global user.email "youremail@example.com"
```
### 4. Check Git configuration
```
git config --list
```
### 5. Check repository status
```
git status
```
### 6. Modify file and stage the change
```
echo "Some changes" > changes.txt
git add changes.txt
git status
```
### 7. View the diff of staged changes
```
git diff --staged
```
### 8. Commit staged changes
```
git commit -m "Added changes.txt"
```
### 9. Add a remote repository
```
git remote add origin https://github.com/user/my-repo.git
```
### 10. view current remote repo
```
git remote -v
```
### 11. Push the changes to the remote repository
```
git push -u origin develop
```
### 12. Fetch changes from the remote repository (without merging)
```
git fetch origin
```
### 13. Pull changes (fetch + merge)
```
git pull origin develop
```
### 14. View commit history
```
git log --oneline --graph --all
```
### 15. List all objects in the repository with their hashes
```
git rev-list --objects --all
```
### 16. Check the type of an object
```
git cat-file -t e69de29bb2d1d6434b8b29ae775ad8c2e48c5391
```
### 17. View the content of an object
```
git cat-file -p e69de29bb2d1d6434b8b29ae775ad8c2e48c5391
```
### 18. Show the contents of a specific tree object
```
git ls-tree 7e9bd8b9f4698ae24b5b4f1f6532515d5a1b2e77
```
### 19. Generate a hash of a file without adding to the repository
```
git hash-object README.md
```
### 20. Generate a hash and write it to the .git/objects/ directory
```
git hash-object -w README.md
```
### 21. View files and directories in the latest commit
```
git ls-tree -r HEAD
```
### 22. View the details of the latest commit
```
git show HEAD
```
### 23. View the SHA-1 hash of the files currently in the index
```
git ls-files --stage
```
### 24. Check the repository integrity
```
git fsck
```

# Git Cheat Sheet: Branching, Merging, and Collaboration

### 1. Initialize a New Git Repository
```
git init
```
### 2. Create a New Branch
```
git branch <branch-name> # Example: git branch feature/login-page
```
### 3. Create and Switch to a New Branch
```
git checkout -b <branch-name> # Example: git checkout -b feature/login-page
```
### 4. Switch to an Existing Branch
```
git checkout <branch-name> # Example: git checkout feature/login-page
```
### 5. View All Branches
```
git branch
```
### 6. Delete a Branch and Force delete a branch
```
git branch -d <branch-name> # Example: git branch -d feature/login-page

```
### Force delete a branch
```
git branch -D <branch-name> # Example: git branch -D feature/login-page
```
### 7. Merge a Branch into the Current Branch (e.g., main)
```
git checkout main
git merge <branch-name> # Example: git merge feature/login-page
```
### 8. Resolve Merge Conflicts
**Edit conflicting files to resolve conflicts.**
```
git add <file-with-conflict> # Example: git add index.html
git commit -m "Resolved merge conflict in <file-name>"
```
### 9. Rebase the Current Branch onto Another Branch (e.g., main)
```
git checkout feature/login-page
git rebase main
```
### Continue rebase after resolving conflicts
```
git rebase --continue
```
### Abort an ongoing rebase
```
git rebase --abort
```
### 10. Push Changes to a Remote Repository
```
git push origin <branch-name> # Example: git push origin feature/login-page
```
### 11. Pull the Latest Changes into Your Local Branch
```
git checkout main
git pull origin main
```
### 12. Fetch and Merge Changes from Remote
```
git fetch origin
git merge origin/main
```
### 13. View Repository Status
```
git status
```
### 14. View Commit History
```
git log
```
### 15. Pull the Latest Changes
```
git pull origin main
```
**Merge Conflict Example
Conflict markers:**
```
# <<<<<<< HEAD
# <h1>Welcome to My Awesome Website</h1>

# <h1>Welcome to My Awesome Website</h1>
# =======
# <h1>Welcome to Our Branded Page</h1>
# >>>>>>> feature/branding
# Resolve by editing, then:
```
**After resolve conflict than fire a command**
```
git add index.html
git commit -m "Resolved merge conflict in index.html"
```
### 16. Best Practices
**Pull frequently to stay updated**
```
git pull origin main
```
### Rebase for cleaner commit history
```
git checkout feature/login-page
git rebase main
```