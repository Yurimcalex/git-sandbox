## Content

- [Basics - git configuration](#title-1)
- [Repo creation, first commit](#title-2)
- [Git and file permissions](#title-3)
- [Git show, author and commiter](#title-4)
- [Adding files and directories, git status](#title-5)
- [A good commit](#title-6)
- [Why do we need an index?](#title-7)
- [A Commit without git add](#title-8)
- [Deleting and renaming files](#title-9)
- [Branches - creation and switching](#title-10)
- [Checkout command when there are uncommitted changes](#title-11)
- [Branches - Transfer of uncommitted changes](#title-12)
- [Branches - Transferring branches manually](#title-13)
- [Branches – State of the detached HEAD](#title-14)
- [Restoring previous versions of files](#title-15)
- [View history and old files](#title-16)

### <a id="title-1">Basics - git configuration</a>

Configuration level flags.

- --system   /etc/gitconfig

- --global   ~/.gitconfig  

- --local   <project>/.git/config

Order of searching for settings: local > global > system.

These settings will be applied to all projects of the current user:

```sh
git config --global user.name "username"
git config --global user.email user@email.com
```

List settings:

```sh
git config --list
git config --list --global
cat ~/.gitconfig
```

Remove parameter name of user section:

```sh
git config --unset user.name
```

Remove section user:

```sh
git config --remove-section user
```

Alias for config command:

```sh
git config --global alias.c 'config --global'
```

Alias for multiple commands:

```sh
git config alias.<command_name> '!git ...; git ...'
```

Show config option:

```sh
git config -h
git help config
```

### <a id="title-2">Repo creation, first commit</a>

Create repository:

```sh
git init
```
There are three zonez.
Working Directory > Index > Repository.
- Working Directory - current project files.
- Index - list of files tracked by git.
- Repository - full project's history.

Add file to Index:

```sh
git add <file>
```

Add indexed file to Repository:

```sh
git commit
```

### <a id="title-3">Git and file permissions</a>

... create mode 100644 <file>  
First three nnn - object type saved to git, 100 - file.  
Second three nnn - file permissions, 644 - not executable, 755 - executable.

chmod +x <file> - changing file permissions modifies the file in git like adding some content to it.

### <a id="title-4">Git show, author and commiter</a>

To view a commit:

```sh
git show <commit id>
```

To show the current commit with additional information:

```sh
git show --pretty=fuller
```

Commit's author is the one who came up with current changes.  
Commiter is the one who created commit in the repsitory.

Set commit's author:

```sh
git commit --author='John Doe <email@some.com>' --date='...'
```
### <a id="title-5">Adding files and directories, git status</a>

Add to git all current catalogue - all made changes :

```sh
git add .
```

.gitignore - list of files that git doesn't touch.  

Add to git index a file that .gitignore contains:

```sh
git add -f <file>
```

### <a id="title-6">A good commit</a>

It's important that one commit do one thing.   
Such a commit is called atomic.   
A commit should be consistent.


### <a id="title-7">Why do we need an index?</a>

If we have different changes related to different topics at the same time, we can add the specific topic's changes to the index and then commit them as consistent.

At the single file level, we can choose which changes to add to the index.
```sh
git add -p <file>
```


### <a id="title-8">A Commit without git add</a>

To save all changes:
```sh
git commmit -am"..."
```

Commit a file without using git add and affecting the other changes:
```sh
git commit -m"..." <file>
```
These commands don't affect untracked files.

We can create an alias for commiting all changes even untracked files. Add only current directory:
```sh
git config --global.commitall '!git add .; git commit'
git commitall -m ...
```

Such alias adds all changes and untracked files in all directories:
```sh
git config --global.commitall '!git add -A; git commit'
```



### <a id="title-9">Deleting and renaming files</a>

```sh
git rm <paths>
git rm -r <dir>
```
The command 'git rm' is the same as 'rm' + 'git add'.

Remove from the index but leave in the working directory:
```sh
git rm -r --cached <dir>
```
After executing this command 'dir' will be untracked.

Flag --cached means that an operation is applied to the index instead of the working directory.

To remove a file with changes that were not saved in the repo:
```sh
git rm -f <file>
```

To rename a file:
```sh
git mv <old> <new>
```

### <a id="title-10">Branches - creation and switching</a>

A branch is a special link to a commit.   
.git/refs/heads/branchName - commitId inside.

Show current branch:
```sh
git branch
git branch -v
```

.git/HEAD - using this file git knows on which branch is it now. 

Create a new branch:
```sh
git branch <branchName>
```

Switch to a branch:
```sh
git checkout <branchName>
```

Create and switch at the same time:
```sh
git checkout -b <branchName>
```

### <a id="title-11">Checkout command when there are uncommitted changes</a>

If these changes are not needed (the changes will be removed):
```sh
git checkout -f <otherBranch>
```

Remove all uncommited changes:
```sh
git checkout -f HEAD
```

If the changes are needed:
```sh
git stash
git checkout <otherBranch>
```

Switch back and apply pre saved changes:
```sh
git checkout <prevBranch>
git stash pop
```

Checkout overwrites files that differ between branches and therefore warns about such a switch. But it does not warn when the files are not different, so you need to be careful not to accidentally commit changes from one branch to another.

### <a id="title-12">Branches - Transfer of uncommitted changes</a>

If we have some uncommitted changes that are not ready to go into the current branch, then we can create another branch for them:
```sh
git checkout -b <anotherBranch>
```

### <a id="title-13">Branches – Transferring branches manually</a>

If we made commits on the master branch and then realized that we would like to put them in a separate branch:
```sh
git branch <separateBranch>
git checkout <separateBranch>
git branch -f master <commitIdBeforeTheCommits>
```

or:

```sh
git branch <separateBranch>
git checkout -B master <commitIdBeforeTheCommits>
```

If you change your mind about doing it:
```sh
git branch -f master <separateBranch>
```

### <a id="title-14">Branches – State of the detached HEAD</a>

It occurs when we move not to a branch but to some commit. And when we forgot about it and already made a commit, we would move this commit to the current branch:
```sh
git cherry-pick <commitId>
```

### <a id="title-15">Restoring previous versions of files</a>

Restore the specified file to the state of a specific commit:
```sh
git checkout <commitId> <filePath>
```

Reset a file from the index:
```sh
git reset <file>
```

Remove file changes in the working directory.   
Return the file from the repo to the index and WD:
```sh
git checkout HEAD <paths>
```   
Return the file from the index to the WD:
```sh
git checkout <paths>
```

### <a id="title-16">View history and old files</a>

All commits:
```sh
git log --oneline
```

Commits on a specific branch:
```sh
git log <branch> --oneline
```

Specific commit:
```sh
git show <commitId>
```

Two commits under The HEAD. Instead of HEAD there can be any commit or branch:
```sh
git show HEAD~~ --quiet
git show HEAD~2 --quiet
git show @~2 --quiet
```

Show not changes but the entire file:
```sh
git show <commitLink>:<filr>
example: git show @~:index.html
```

If the file version in Index is different from the current one:
```sh
git show :<file>
```

Newest commit with the word in the description:
```sh
git show :/<word>
```