## Basic command lines

1. `git init` : create a new git repo. Before we can do anything git-related, we must initialize a repo first.

2. `git status` : gives information on the current status of a git repo and its contents.

The basic git workflow:

1. work on stuff (make new files, edit files, delete files, etc.)

2. add changes (group specific changes together, in preparation of committing.)

3. commit (commit everything that was previously added.)

working directory---`git add`--->staging area---`git commit`---> repository

3. `git add file1 <file2 ...>` : add specific files to the staging area. Seperate files with spaces to add multiple at once.

4. `git commit -m "message"` : actually commit changes from the staging area with message. (git commit will pop up vim editor)

5. `git log` : showing the log of commit history. (use `git log --oneline` to indicate brief information)

6. `git commit --amend` : you can "redo" the previous commit using the --amend option when you made new change to the repo but do not want to create a new commit.

7. `git commit -a -m "message` : adding all unstaged files and commit them all at once. 

Atomic commits: when possible, a commit should encompass a single feature, change or fix. In other words, try to keep each commit focused on a single thing. This makes it much easier to undo or rollback changes later on. It also makes your code or project easier to review.

`.gitignore` file : we can tell Git which files and directories to ignore in a given repo.

Working with branches
---------------
Branches are an essential part of Git. They enable us to create separate contexts where we can try new things, or even work on multiple ideas in parallel.

In 2020, Github renamed the default branch from **master** to **main**. The default Git branch name is still **master** however.

**HEAD** is simply a pointer that refers to the current location in your repo. It points to a particular branch reference. HEAD always points to the latest commit you made on the master branch.

`git brach`: indicate all current branches

`git branch <branch-name>` : create a new branch based upon the current HEAD. Note that the HEAD does not switch to that newly created branch.

`git switch <branch-name>` : Once you have created a new branch, use this command to swith to it.

`git switch -c <branch-name>` : create branch and switch to this branch directly.

`git checkout <branch-name>` : perform the same as `git switch` command.

Tip: always check status before switch to other branch.

What happens if we made some changes but not commit when we switch?

- if there is not conflicts with other branches, those changes with be carried alone with you to the new branch.
- If there is a conflict, an error occured. It requires you to commit or stash first.

Delete a brach:

	- `git branch --delete <branch-name>` or `git branch -d <branch-name>`
	- `git branch --delete --force <branch-name>` or `git branch -D <branch-name>`

`git branch -m <new_name>` : rename a branch name.

Merge
---------
We can use git merge command to incorporate changes from one branch into another.

To merge, follow the basic steps:

- switch to the branch you want to merge the changes into (the receiving branch)
- use the `git merge` command to merge changes from a specific branch into the current branch (where the HEAD is at)

Fast forward branch: when we want to merge the additional branch into the split point (where there is no conflict). This is the simplest one.

`git switch receiving_branch`
`git merge another_branch`

When there is a conflict, we must solve the conflict, and then add and commit the change. We must be careful when we edit the conflict. Might need to discuss with the person who conflict with.

Compare changes with git diff
--------------------------
`git diff` : without additional options, `git diff` lists all the changes in our working directory that are NOT staged for the next commit. (**Note that it does not include untracked files!**)

`git diff HEAD` : It shows changes both staged and unstaged.

`git diff --staged` : shows the changes only in staged area.

`git diff [filename]` : specify a file to see the changes.

`git diff branch1..branch2` will list the changes between the tips of branch1 and branch2.

`git diff commit1_hash..commit2_hash` : compare two commits.