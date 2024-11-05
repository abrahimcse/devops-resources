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
![tracked]()

Not all folders are meant to be tracked by git. Here we can see that all green folders are projects are getting tracked by git but red ones are not.

### 4. Commit
commit is a way to save your changes to your repository.
- `Usual flow looks like this:`

