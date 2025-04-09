---
title: Using Git on macOS
authors: [silentglasses]
date: 2021-12-22 09:58:00
categories: [DevOps, Git]
tags: [git, utilities, devops, versioning]
---

Using Git on macOS provides a powerful and streamlined way to manage your source code and collaborate with others. With its robust command-line interface and integration with popular code editors like Visual Studio Code, Git enables version control and efficient project management for developers. In this guide, we'll walk you through the essentials of getting started with Git on macOS, from installation to basic commands and best practices.
<!-- more -->

!!! note
    Before proceeding, ensure that you have your SSH properly setup and configured. _Using SSH Keys tutorial coming soon_.

### Xcode

Install Xcode by running this command in your terminal

```
xcode-select --install
```

### Using Homebrew

You should already have Homebrew installed.  _Using Homebrew tutorial coming soon_

Run the following command to install Git

```
brew install git
```

### The manual way

-  Download the most recent release from [here](https://sourceforge.net/projects/git-osx-installer/files/latest/download)
-  Install GitHub Desktop and launch it
-  Click GitHub Desktop in the top left corner of your screen then select `Install Command Line Tool`, this will allow you to use GitHub from command line instead of a GUI.

### Verification

Verify you can connect to GitHub using your SSH Key.

```
ssh -vT git@github.com
```

If you are connecting for the first time, you will get this message, type yes then hit enter to proceed.

```
# The authenticity of host 'github.com (207.97.227.239)' can't be established.
# RSA key fingerprint is xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx.
# Are you sure you want to continue connecting (yes/no)?
```

You may see other things but you should get a line that says something like this:

```
Hi username! You've successfully authenticated, but GitHub does not provide shell access.
```

Don't worry about the shell access thing, you don't want that anyway.

### Configuring your Git Profile

-  The below commands will configure your setting for **ALL** your git repos.
-  To make them work on each repo individually run them without the `--global` flag. You will need to repeat these steps for each repo you work on.
    * Global is good if you only have one account you work in git with.
    * If you have more than one account, then non-global lets you configure each repo with its respective account.

```
git config --global user.name "Full Name"
git config --global user.email "username@domain.com"
git config --global core.editor vim
git config --global color.ui auto
```

-  Remember to switch vim above to your editor of choice.

## Using Git

-  clone repo so that `origin` is defined

```
git clone git@github.com:org/repo_name.git
```

-  load into your repo
```
cd repo_name
```

-  add your fork
```
git remote add username git@github.com:username/forked_repo.git
```

-  print remote to verify it's setup correctly
```
git remote -v
```
    * You should see something like this _(the order may be different for you)_
```
origin	git@guthub.com:org/repo_name.git (fetch)
origin	git@guthub.com:org/repo_name.git (push)
fork	git@guthub.com:username/forked_repo.git (fetch)
fork	git@guthub.com:username/forked_repo.git (push)
```

-  New Branch/PR:
```
git checkout -b my_new_branch
```

-  rebase with origin/main if outdated
```
git rebase origin/main
```

-  make changes and push branch
```
git push <fork> my_new_branch
```

## Older good ways to do the same as above

### Update Git Repo Fork with Master

This is done from a terminal.

Browse to the cloned repository you want to update and proceed with the following:

-  Verify
```
git remote -v
```

-  Specify a remote upstream repo to sync with your fork:
```
git remote add upstream <repo_url>
```

-  Verify:
```
git remote -v
```

### Update Git Repo Fork with Origin Master

-  Fetch branches and commits from the upstream repo. You’ll be storing the commits to master in a local branch upstream/master:
```
git fetch upstream
```

-  Checkout your fork’s local master, then merge changes from upstream/master into it.
```
git checkout master
git merge upstream/master
```

-  Push your updates up to your fork
```
git push origin master
```

###  Rebase a Pull Request with Master

- *Warning**: Not sure about the following entirely, it was sent to me from a co-worker but I've not used it yet.

-  Fetch branches and commits from the upstream repo. You’ll be storing the commits to master in a local branch upstream/master:
```
git fetch upstream
```

-  Checkout your fork’s local master, then
```
git checkout master
```

-  merge changes from upstream/master into it.
```
git remote update --prune
```

-  Push your updates up to your fork
```
git fetch upstream
```

-  Merge upstream master
```
git merge upstream/master
```

-  Checkout the branch you were working on
```
git checkout branchName
```

### Updating a feature branch

- *Important**: Do this after you have updated your Git Fork with the Origin Master. (see instructions [above](#update-git-repo-fork-with-origin-master))

-  Check out the branch you want to merge into
```
git checkout <feature-branch>
```

-  Merge your (now updated) master branch into your feature branch
```
git merge master
```

Depending on your git configuration this may open vim. Enter a commit message, save, and quit vim:

-  Press `a` to enter insert mode and append text following the current cursor position.
-  Press the `esc` key to enter command mode.
-  Type `:wq` to write the file to disk and quit.

-  This only updates your local feature branch. To update it on GitHub, push your changes.
```
git push origin <feature-branch>
```

## Cheatsheet

This is not an exhaustive list of what Git can do, but rather the more common things you may come across in your daily use.

### Create Repositories

_Start a new repository or obtain one from an existing URL_

-  Create a new local repository with the specified name
```
git init [project-name]
```
-  Download a project and its entire version history
```
git clone [url]
```

### Making changes

_Review edits and craft a commit transaction_

-  List all new or modified files to be committed
```
git status
```
-  Show file differences not yet staged
```
git diff
```
-  Snapshot a file in preparation for versioning
```
git add [file]
```
-  Show file differences between staging and the last file version
```
git diff --staged
```
-  Unstage a file, but preserve its contents
```
git reset [file]
```
-  Record file snapshots permanently in version history
```
git commit -m "[descriptive message]"
```

### Group Changes

_Name a series of commits and combine completed efforts_

-  List all local branches in the current repository
```
git branch
```
-  Create a new branch
```
git branch [branch-name]
```
-  Switch to a specified branch and update a working directory
```
git checkout [branch-name]
```
-  Combine a specified branch’s history into the current branch
```
git merge [branch]
```
-  Delete a specified branch
```
git branch -d [branch-name]
```

### Refactor Filenames

_Relocate and remove versioned files_

-  Delete a file from the working directory and stages the deletion
```
git rm [file]
```
-  Remove a file from version control but preserves the file locally
```
git rm --cached [file]
```
-  Change a file name and prepares it for commit
```
git mv [file-original] [file-renamed]
```

### Suppress Tracking

_Exclude temporary files and paths_

-  A text file named `.gitignore` suppresses accidental versioning of files and paths matching the specified patterns, examples:
```
- .log
build/
temp-*
```
-  List all ignored files in a project
```
git ls-files --other --ignored --exclude-standard
```

### Save Fragments

_Shelve and restore incomplete changes_

-  Temporarily store all modified tracked files
```
git stash
```
-  Restore the most recently stashed files
```
git stash pop
```
-  List all stashed changesets
```
git stash list
```
-  Discard the most recently stashed changeset
```
git stash drop
```

### Review History

_Browse and inspect the evolution of project files_

-  List version history for the current branch
```
git log
```
-  List version history for a file, including renames
```
git log --follow [file]
```
-  Show content differences between two branches
```
git diff [first-branch]...[second-branch]
```
-  Output metadata and content changes of the specified commit
```
git show [commit]
```

### Redo Commits

_Erase mistakes and craft replacement history_

-  Undo all commits after [commit], preserving changes locally
```
git reset [commit]
```
-  Discard all history and changes back to the specified commit
```
git reset --hard [commit]
```

### Synchronize Changes

_Register a repository bookmark and exchange version history_

-  Download all history from the repository bookmark
```
git fetch [bookmark]
```
-  Combine bookmark’s branch into current local branch
```
git merge [bookmark]/[branch]
```
-  Upload all local branch commits to GitHub
```
git push [alias] [branch]
```
-  Download bookmark history and incorporates changes
```
git pull
```
