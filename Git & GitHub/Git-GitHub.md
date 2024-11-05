# Git and Github
- `Git` is a version control system that is used to track changes to your files. It is a free and open-source software that is available for Windows, macOS, and Linux. Remember, GIT is a software and can be installed on your computer.

- `Github `is a web-based hosting service for Git repositories. Github is an online platform that allows you to store and share your code with others. It is a popular platform for developers to collaborate on projects and to share code. It is not that Github is the only provider of Git repositories, but it is one of the most popular ones.

**Git is the tool you use for tracking changes to your code on your own machine.**

**GitHub is a service for hosting your Git repositories remotely and collaborating with others.**

## Install Git

To install Git, you can use command line or you can visit official website and download the installer for your operating system. Git is available for Windows, macOS, and Linux and is available at [Here](https://git-scm.com/downloads).

## Terminology

### 1. Check your git version

```
git --version
```
### 2. Your config settings
There are some global settings and some repository specific settings.
```
git config --global user.email "your-email@example.com"
git config --global user.name "Your Name"
```
Now you can check your config settings:

```
git config --list
```

### 3. Repositories
- `Creating a repository `
```
git init
```
- `Check status`

```bash
git status
```
![](https://github.com/abrahimcse/devops-resources/blob/main/Git%20%26%20GitHub/Images/repo.png)

Not all folders are meant to be tracked by git. Here we can see that all green folders are projects are getting tracked by git but red ones are not.

### 4. Commit
commit is a way to save your changes to your repository.
- `Usual flow looks like this:`

![](https://github.com/abrahimcse/devops-resources/blob/main/Git%20%26%20GitHub/Images/commit.png)

- `Complete git flow`

![](https://github.com/abrahimcse/devops-resources/blob/main/Git%20%26%20GitHub/Images/gitflow.png)

When you want to track a new folder, you first use `init` command to create a new repository. Then you can use `add` command to add the folder to the repository. After that you can use `commit` command to save the changes. Finally you can use `push` command to push the changes to github. Of course there is more to it but this is the basic flow.

### 5. Stage
Stage is a way to tell git to track a particular file or folder.
```
git init
git add <file> <file2>
git status
```
### 6. Commit
The `-m` flag is used to add a message to the commit.
```
git commit -m "commit message"
git status
```
### 7. Logs
```
git log
```
You can use the `--oneline` flag to show only the commit message
```
git log --oneline
```
### 8. Change default code editor
You can change the default code editor in your system to `VScode`.
```
git config --global core.editor "code --wait"
```
**Here's a breakdown:**

- `git config --global`: This sets a Git configuration option globally for your user account.
- `core.editor`: This is the Git option that specifies which editor to use for commit messages and other text inputs.
- `"code --wait"`: This specifies VS Code as the editor. The `--wait` flag tells Git to wait until you close the VS Code window before proceeding (e.g., when you write a commit message).

### 8.Gitignore
Gitignore is a file that tells git which files and folders to ignore. It is a way to prevent git from tracking certain files or folders.
```
node_modules
.env
.vscode
```
When you run the `git status` command, it will not show the `node_modules` , `.env` and `.vscode` folders as being tracked by git.

## Branched in git

