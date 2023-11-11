# GitHub 
## Basic Git Commands
## 1) Git Repository
- Cloning a Git repository:
```sh
   git clone https://github.com/username/the_repo_name.git
```
   `NOTE`: Make sure to copy the url of the repo you want to clone.
- Initialize a Git repository:
```sh
   git init 
```
- Add files to the staging area:
```sh
   git add .
```
   `NOTE`: Works for new files that have not been added to Git.
- Commit the files to Git:
```sh
   git commit -m "<message>"
```
   `NOTE`: Replace `<message>` with your own message.
- Commit modified files:
```sh
   git commit -a -m "<message>"
```
- Making a commit after modifing a committed file:
```sh
  git commit -m "<message>" filename
```
- Check status of the working tree:
```sh
  git status
```
- Check more information about previous commits:
```sh
  git log
```
- Check information about previous commits in a more concise listing:
```sh
  git log --oneline
```
- Check information on the latest commit made:
```sh
  git log -n1
```
- Track changes made on a file:
```sh
   git diff
```

## 2) Configure Git 
- Enter your GitHub username:
```sh
  git config --global user.name "<USER_NAME>"
```
 `NOTE`: Replace `<USER_NAME>` with your username.
  
- Enter your email address used in GitHub:
```sh
  git config --global user.email "<USER_EMAIL>"
```
 `NOTE`: Replace `<USER_EMAIL>` with your email address.
- Check the changes made:
```sh
  git config --list
```

## 3) Fix Simple Mistakes
- Ever been in a situation where you forgot to add a file after making a commit? Here is a simple way to fix that:
```sh
  git commit --amend --no-edit
```
- Recover a deleted file using `git checkout`:
```sh
  git checkout  -- filename
```
- Recover a deleted file after accidentally running `git rm`:
```sh
  git reset HEAD filename
  git checkout -- filename
```
- Revert a Commit:
```sh
  git reset --hard HEAD^
```


- - -  
## Other Git Commands
- Check Git version installed:
```sh
  git --version
```
- Get help from Git:
```sh
  git --help
```
- Deleting files in Git:
```sh
  git rm filename
```
