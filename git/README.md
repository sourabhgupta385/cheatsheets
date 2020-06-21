## Git Config

| Command                              | Description             |
| ------------------------------------ | ----------------------- |
| git config -- global user.name NAME  | set user name globally  |
| git config --global user.email EMAIL | set user email globally |

## Creating repo

| Command                   | Description                                                    |
| ------------------------- | -------------------------------------------------------------- |
| git init                  | creates a git repository in the directory currently in Staging |
| git status                | to check status , if staged or unstaged                        |
| git add FILE_NAME         | to add a file to staging area                                  |
| git rm --cached FILE_NAME | to remove a file from staging area                             |
| git add .                 | to add all files in project to staging area                    |

## Commiting

| Command                               | Description                                        |
| ------------------------------------- | -------------------------------------------------- |
| git commit -m "Specific Changes Made" | commits the staging area giving them a specific id |
| git log                               | shows all the commits with details                 |
| git log --oneline                     | shows all the commits in one line each             |

## Git Stash

| Command                          | Description                                                                                       |
| -------------------------------- | ------------------------------------------------------------------------------------------------- |
| git stash                        | clears the changes to the initial state (last commit) & creates a unique id for the current state |
| git stash apply                  | brings back the current state                                                                     |
| git stash list                   | shows all the stash (States) with their ID                                                        |
| git stash apply ID               | ID will be the number , which state you want to go back to                                        |
| git stash push -m "Your message" | used to give description to stash                                                                 |
| git stash drop ID                | used to remove a stash saved                                                                      |
| git stash pop ID                 | applies the specific stash and removes it from history                                            |
| git stash clear                  | removes all the stash history                                                                     |

## Reverting & Reset

| Command                    | Description                                                                                                                                                                                                                                                  |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| git log --oneline          | To see the commit_ID to change to                                                                                                                                                                                                                            |
| git checkout commit_ID     | to just check the commit id entered , see it in read only ... changes will not be saved                                                                                                                                                                      |
| git checkout master        | to come back to original commit (As checkout removes us from master branch)                                                                                                                                                                                  |
| git revert commit_ID       | to remove the changes of the provided commit (will add a new revert commit and remove the changes of the specific commit)                                                                                                                                    |
| git reset commit_ID        | will remove all the commits after the provided id , but the files in local directory will not be touched (therefore you can still commit to original state after doing changes as needed) ... might take you to vim editor (type ":wq" then "Enter" to exit) |
| git reset commit_ID --hard | will remove all the commits after the provided id and even delete all the files and lines from local directory too                                                                                                                                           |

## Branches

| Command                     | Description                                         |
| --------------------------- | --------------------------------------------------- |
| git branch branch_name      | to create a new branch                              |
| git branch -a               | to list all the branches                            |
| git checkout branch_name    | to shift to the other branch                        |
| git branch -d branch_name   | to delete the branch only when it has been merged   |
| git branch -D branch_name   | to delete the branch (even if not merged to master) |
| git checkout -b branch_name | to create and shift to a new branch at once         |

## Merging branches

| Command                        | Description                                                                           |
| ------------------------------ | ------------------------------------------------------------------------------------- |
| git merge branch_name          | this will merge the branch to master (all commits show in master)                     | automatic |
| git merge --squash branch_name | this will merge the branch to master (only the commit after merge is shown in master) | manual |
