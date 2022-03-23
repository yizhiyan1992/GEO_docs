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

The Ins and Outs of stashing
----------------------
Scenario where the stash will be usesd: Did some new work and change to other branches without making any commits.

What happends:

	- The change would come with me to the destination branch if there is no conflicts.
	- Gir won;t let me switch if it detects potential conflict.

**The key takeaway is that we need to use `git status` to check before we switch to other branches.**

`git stash` : it helps you save changes that you are not yet ready to commit (both unstaged and staged).

`git stash pop` : remove the most recently stashed changes in your stash and re-apply them to your working directory.

Undoing changes and Time traveling
----------------------
**detached head**

We can use `git checkout <commit-hash>` to view a previous commit. Then we are in the `detached HEAD` state.

Usually, HEAD points to a latest commit of a specific branch. In the detached HEAD, we have the following options:

- stay in the detached HEAD to examine the contents of the old commit.
- leave and go back to before. (`git switch <branch_name>` or `git switch -`)
- create a new branch and switch to it. (`git switch -c <new_branch>`)

A different way to go back to previous commits:
`git checkout HEAD~1` refers to the commit before HEAD

**discard changes**

`git checkout HEAD <file_name>` : Suppose we modified a file but do not want to keep them (havent added to the staging area yet). To revert the file back to whatever it looked like when you last committed. Use this command, or `git checkout -- <file_name>`.

**restore command**

- `git restore <file_name>`:un-modify files, which is basically the same as `git checkout HEAD <file_name>`
- `git restore --staged <file_name>`: accidentally added a file to the staging area, and don't wish to include it in the next commit, you can use this command to remove it from staging area.

**undo commits with git reset**

- `git reset <commit-hash>` will reset the repo back to a specific commit. The commits are gone, but the cotents will be remained as unstaged contents. (soft version of reset)
-`git reset --hard <commit-hash>` will reset the repo back to the specific commit and also delete associate changes.

**using revert to undo changes**
`git revert` is similar to `git reset` in that they both undo changes. But they accomplish it in different ways.

- `git reset` actually moves the branch pointer backwards, eliminating commits.
- `git revert` instead creates a brand new commit which reverses/undos the changes from a commit.

Which one should I use?

When collaborating with other people, use `git revert` because it is hard to track on other people's machine if using `git reset`. On the contrary, if you want to reverse commits that you havent shared with others, use reset and no one will ever know.

The basics of GitHub
--------------------
Github is a service that hosts Git repositories in the cloud and makes it easier to collaborate with other people. It is an online place to share work that is doing using Git.

`git clone <url>`: Git will retrieve all the files associated with the repository and will copy them to your local machine. It will also initalize a new repo on your machine, and give yuou access to the full Git history of the cloned project.

**config your SSH key**

You nned to be authenticated on Github to do certain operations. You generate and configure an SSH key. Once configured, you can connect to Github without having to supply your name/password everytime.

**remote**

Before we can push anything up to github, we need to setup a destination to push up to. In Git, we refer to these destinations as remotes. Each remote is simply a URL where a hosted repo lives.

`git remote -v`:view any exisitng remotes for your repo.

`git remote add <name> <url>`: add a new remote. Usually the name is set as "origin" by default.

**pushing**

`git push <remote> <branch>`:to push the work up to the Github, we need to specify the remote name we want to push and the specific local branch we want to push up to that remote.

`git push <remote> <local_b>:<remote_b>` : when the name of local branch does not match with branch name on remote.

`git push -u <remote> <branch>`: The -u option allows us to set the upstream of the branch we are pushing. A link connecting our local branch to a branch on Github. Next time, we can just use `git push`.

**master & main**
When we create a new repo on GitHub with initialized files (e.g. readme), Github will set the branch name as "main" bu default.

`git branch -M main`: change the name of current branch to "main".

**create a new repo on the command line**

```
echo "# animals">>README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin <url>
git push -u origin main

```
`git branch -M main`:rename the branch from master to main.

Fetching and Pulling
-------------------
When we connect a remote repo with local repo, we have 2 branches, ***branch and remote tracking branch***. 

Remote tracking branch points to the point where you last communicate with this remote repo, and they folow this patter: \<remote>/\<branch>. For example, origin/master. **When making a commit locally, the remote trackong branch stay same.**

`git branch -r`: view the remote branches our local repo knows about.

`git status` allows you to see if the branch and remote tracking branch are on the same point.

When cloning a remote repo, there is only default branch from remote repo showing on local repo (usually origin/main or origin/master).

`git switch <remote-branch-name>`: create a local branch and set it up to track the remote branch with this name.

**fetching**

Fetching allows us to download changes from a remote repo, but those changes will not be automatically integrated into our working file. It only updates remote tracking branch.

`git fecth <remote> <branch>` or `git fetch <remote>`

How to see those changes after fectching?

1. `git status` to indicate consistency.
2. `git checkout <remote>/<branch>` to see changes in detached HEAD.

**pulling**

`git pull <remote> <branch>`: fetch the latest information from the remote branch and merge those changes into our current branch.

git pull = git fetch + merge

Note that git pull can result in conflicts.

Before we push our code, it is recommended to pull to see if there is any conflicts first. If there is a conflict, fix and merge it, and then finally push.

`git pull`: remote will default to origin, and branch will default to whatever tracking nonnection is configured for your current branch.

workspace--->staging--->local repo--->remote repo

1. git fecth: remote repo to local repo
2. git pull: remote repo to workspace

git fetch is safe to do anytime. Git pull is not recommened if you have uncommitted changes, because it can result in conflicts.

Github Collaborations
----------------------
1. centralized workflow

Everyone works on the master branch (the most basic workflow possible). It is straightforward, but it has quite a few shortcomings. If there is bug on the branch, it will prevent other collaborators further pushing the code.

2. feature branches

All new development should be done on seperate branches. Treat master/main branch as the official project history.

Rebase
---------------------
There are two main ways to use the git rebase command:

- as an alternative to merging
- as a cleanup tool

```
git switch feature
git rebase master
```
We can rebase the feature branch onto the master branch. This moves the entire feature branch so that it begins at the tip of the master branch. All of the work is still there, but we have re-written history. (submit new commits)

The golden rule: **when not to use rebase**

Never rebase commits that have been shared with others. If you have already pushed commits up to github, do not rebase them unless you are positive no one on the team is using thos commits.

Solve rebase conflicts:

```
git switch feature
git rebase master
>>>solve conflicts
git rebase --continue 
(or use git rebase --abort to go back)
```

**rewriting history**
Sometimes we want to rewrite, delete, rename, or even reorder commits before sharing them.

`git rebase -i HEAD~4`:interactive rebase

squash and fixup?

Git Tags - Making important moments in history
--------------------
Tags are pointers that refer to particular points in Git history. Tags are most often used to mark **version releases** in project.

There are two types of tags:

- lightweight tags
- annotated tags

### Semantic Versioning
The semantic versioning spec outlines a standarized versioning system for software releases.

Formats:

>  version . minor version . patch version

patch release: normally do not contain new features or significant changes. They typically signify bug fixes and other changes that do not impact how the code is used.

Minor release: signify new featuers or functionality have been added. The project is still backward compatible. (should not force users to rewrite their own code)

Major release: significant changes that is no longer backwars compatible. Features may be removed or changed substantially.

There are some pre-leased version (like -beta). And when we update the minor or major release, we need to reset the number followed by it to 0.

`git tag`: list all tags

`git tag -l "*v16*"`: using wildcard to search specific tags

`git checkout <tag_name>`: check out the status of that tag commit (in a detached head).

`git diff <tag1> <tag2>` : compare the differences of two tags. For instance: `git diff v17.0.0 v17.0.1`.

`git tag <tag_name>` : create lightweight tag (after commit). It points to the current HEAD.

`git tag -a <tag_name>`: create an anotated tag. Git will them open your default text editor and promt you for additional info.

`git show <tag_name>` : show more info about this tag.

`git tag <tag_name> <commit-hash>`: tagging previous commit

`git tag -d <tag_name>` : delete a tag

`git push <remote-name> <tag-name>`: push a tag

Git behind the scenes
-------------------------
### local config file

The config file is for configuration. This config file is local one (per repo-basis), and there is a different one which handles global configurations.

`git config --local user.name`

`git config --global user.name`

We can also specify different colors for changes.

### Refs folder

Inside of refs, you will find a heads directory. **refs/heads** contains one file per branch in a repo. Each file is named after a branch and contains the hash of the commit at the tip of the branch.

Refs also contains a refs/tags folder which contains one file for each tag in the repo.

### HEAD

HEAD is just a text file that keeps track of where HEAD points.

### Objects folder

The objects directory contains all the repo files. This is where Git stores the backups of files, the commits in a repo and more. The files are all compressed and encrypted, so thet won't look like much.

And there are 4 types of git objects: commit, tree, blob, and annotated tag.

Git is a key-value data store. We can insert any kind of content into a Git repo, and Git will hand us back a unique key we can later use to retrieve that content. (SHA-1 hasing, which returns 40 digits hexadecimal numbers.)

`git hash-object <file>`: get the hash key for storing file (not really storing it)

> `echo "hello" | git hash-object --stdin -w`

Retrieve the content by hash key

> `git cat-file -p <hash_code>` #-p stands for pretty print

### Blob

Git blobs (binary large object) are the object type Git uses to store the contents of files in a given repo. Blobs dont even include the filenames or any other data. They just store the contents of a file.

### Trees

Trees are git objects used to store the contents of a directory. Each tree contains pointers that can refer to blobs and to other trees.

### Commits

Commit objects combine a tree object aling with information about the context that led to the current tre. It stores: tree, pointer to the parent, committer, author, and commit message.
