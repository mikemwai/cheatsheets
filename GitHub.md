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
- Initialize a new repository and set the default branch to main:
```sh
  git init -b main
```
`NOTE:` Feel free to change `main` into the branch name of your choice
- Add files to the staging area:
```sh
   git add .
```
   `NOTE`: Works for new files that have not been added to Git.
- Unstaging a specific file:
```sh
  git reset filename
```
- Commit the files to Git:
```sh
   git commit -m "<message>"
```
   `NOTE`: Replace `<message>` with your own message. Ensure the commit message is less than 50 words.
- Add and commit modified files together:
```sh
   git commit -a -m "<message>"
```
- Updating your last Git commit:
```sh
   git commit --amend -m 'message'
```
- Making a commit after modifing a committed file:
```sh
  git commit -m "<message>" filename
```
- Pushing changes to the repo:
```sh
  git push -u origin main
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
  `NOTE:` `git checkout --filename` can also be used to revert a file to it's last committed state.
  
- Recover a deleted file after accidentally running `git rm`:
```sh
  git reset HEAD filename
  git checkout -- filename
```
`NOTE:` `git checkout` updates your files with the present one in the git index while `git reset` unstages deleted files.
- Revert a Commit:
```sh
  git reset --hard HEAD^
```
- Revert a commit without adding a commit message:
```sh
  git revert --no-edit HEAD
```

## 4) Collaborating with other developers
- Clone the project
- After modifing a file and committing it, make a pull request to the original repo:
```sh
  git request-pull -p origin/main .
```
- Create a remote on your local machine:
```sh
  git remote add remote-username repo_url_path
```
- Merge pull request changes into original repo:
```sh
  git pull remote-username main
```

- Merge changes into the current branch `main`:
```sh
  git pull 
```

## 5) Git Branches
- Create a new branch:
```sh
  git branch branchname
```
- Create a new branch and switch to it:
```sh
  git checkout -b branchname
```
- Switch to another branch:
```sh
  git checkout branchname
```
- List the available branches:
```sh
  git branch -a
```
- Rename a branch:
```sh
  git branch -m old_branchname new_branchname
```
- Delete a branch:
   - Safe Deletion (Checks for merge):
     
   ```sh
     git branch -d branchname
   ```
   - Force Deletion (Doesn't check for merge):
     
   ```sh
     git branch -D branchname
   ```

- Merge a specific branch such as `branch1` into the current branch such as `main`:
```sh
  git merge branch1
```

- Restore the current branch `main` before the attempted merge:
```sh
  git merge --abort
```

- Return back to the original state of the current branch `main` before the completed merge:
```sh
  git reset --hard
```

## 6) Stashing changes
- Stash changes:
```sh
   git stash
```
   `NOTE:` `git stash` helps in temporarily saving changes that are not ready to be committed (Incomplete work) which helps in switching branches.

- Recovering stashed changes after switching back to the original branch:
```sh
  git stash pop
```

- Listing stashes:
```sh
  git stash list
```

- Cleaning up the stash:
```sh
  git stash clear
```

## 7) Reverting Git commits
-  Revert a specific git commit:
```sh
   git revert commitHash
```
   `NOTE:` `commitHash` refers to the `commit ID` of the git commits. Replace it with the correct `commitHash`.

- List commitHashes of git commits made:
```sh
  git reflog
```

## 8) Resetting Git commits
- Soft Reset:
```sh
  git reset --soft HEAD^
```
  `NOTE:` It easily allows to backtrack last commit but preserves the changes in the staging area.
  
- Mixed Reset:
```sh
  git reset --mixed HEAD^
```
  `NOTE:` It uncommits the last commit and removes it's changes from the staging area.
  
- Hard Reset:
```sh
  git reset --hard HEAD^
```
  `NOTE:` It completely erases the last commit along with associated changes from the Git history.

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
