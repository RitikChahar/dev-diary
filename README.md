# Git - Global Information Tracker

## Downloading and Installing Git
### Download Git from the following URL and install it with default settings
```
https://git-scm.com/downloads
```
## Checking the installed version of Git
```
git --version
```
## Configuring the username and email
### To configure username
```
git config --global user.name "name"
```
### To configure email
```
git config --global user.email "email"
```
### To configure username and email directly in the config file
```
git config --global --edit
```
1. Press "i" to edit the config file.
2. After making the changes Press "ESC" to save the changes.
3. Type ":wq" to exit the editor.
## Checking the configured username and email
### Configured username
```
git config --global user.name 
```
### Configured email
```
git config --global user.email
```
## Creating a git repository 
### To create a git repository in existing directory
```
git init
```
### To create a new directory and git repository
```
git init name
```
## Checking the status of the branch
```
git status
```
## Adding files to the staging area
### To add a file to the staging area
```
git add fileName
```
### To add all the files to the staging area
```
git add .
```
## Commiting changes 
```
git commit -m "message"
```
## Checking commit log
```
git log
```
## Checking branches
```
git branch
```
## Creating a new branch
### To create a new branch
```
git branch branchName
```
### To create a new branch and switching to it directly
```
git checkout -b branchName
```
## Switching to a different branch
```
git checkout branchName
```
## Switching to a different version
```
git checkout hashCode
```
## Merging a branch
```
git merge branchName
```
## Deleting a branch
```
git branch --delete branchName
```
## Cloning a repository
```
git clone https://github.com/RitikChahar/Git-GitHub.git
```
## Pulling changes from a repository
```
git pull https://github.com/RitikChahar/Git-GitHub.git
```
## Creating .gitignore file
```
touch .gitignore
```
### To add subdirectories to .gitignore
```
**/directory/subdirectory
```
## Pushing a repository to GitHub
### 1. Create a new repository on GitHub.
### 2. To add origin to the git repository
```
git remote add origin https://github.com/RitikChahar/Git-GitHub.git 
```
### 3. To check origin
```
git remote -v
```
### 4. To push code to GitHub
```
git branch -M master
git push
```
