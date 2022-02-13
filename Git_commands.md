## Basic command lines

1. `git init` : create a new git repo. Before we can do anything git-related, we must initialize a repo first.

2. `git status` : gives information on the current status of a git repo and its contents.

The basic git workflow:

1. work on stuff (make new files, edit files, delete files, etc.)

2. add changes (group specific changes together, in preparation of committing.)

3. commit (commit everything that was previously added.)

working directory---`git add`--->staging area---`git commit`---> repository

3. `git add file1 <file2 ...>` : add specific files to the staging area. Seperate files with spaces to add multiple at once.

4. `git commit -m "message"` : actually commit changes from the staging area with message.

5. `git log` : showing the log of commit history.
