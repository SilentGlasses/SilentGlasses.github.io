---
title: Git Best Practice
author:
  name: SilentGlasses
  link: https://github.com/SilentGlasses
date: 2021-12-22 10:30:00
categories: [DevOps, Git]
tags: [git, utilities, devops, versioning]
---

These are just a collection of things I find interesting for myself and may not be for everyone.

<img class="center" alt="Git Commit XKCD Comic" src="https://imgs.xkcd.com/comics/git_commit_2x.png" style="width: 439px; max-height: 250px">

## Keep your commits small and clean

When working with changes to fix bugs, it's easy to get sidetracked and work on multiple bugs as you see them. It's fine to work on multiple things at the same time in a repository but you want to keep each commit small and focused. You can later squash your commits to just do one big commit to master (see below).

`Atomic commit = one commit for one change.`

Making smaller more focused commits allows for easier roll-backs of a small commit rather than one huge commit that undos all the work you did when it was only one minor thing that needed to change. This practice also makes it easier for the next person to read and follow the changes that were made to the repository.

## Squash commits before merging

While working on your feature branch you should commit changes often but the commit to master should only be one or as few commits as you can make. Squashing commits does not overwrite your existing commits but rather packages them into the one commit you will push to master and allows you to provide a more elaborate message explaining all the changes being made.

```
git rebase -i origin/main
```

This will pop-up a list of commits to choose from in your editor like so:

```
pick 0000000 one commit message
pick 0000000 another commit message
pick 0000000 yet another commit message
# Rebase 0000000..0000000 onto 0000000 (3 commands)
#
# Commands:
# p, pick <commit> = use commit
# r, reword <commit> = use commit, but edit the commit message
# e, edit <commit> = use commit, but stop for amending
# s, squash <commit> = use commit, but meld into previous commit
# f, fixup <commit> = like "squash", but discard this commit's log message
# x, exec <command> = run command (the rest of the line) using shell
# b, break = stop here (continue rebase later with 'git rebase --continue')
# d, drop <commit> = remove commit
# l, label <label> = label current HEAD with a name
# t, reset <label> = reset HEAD to a label
# m, merge [-C <commit> | -c <commit>] <label> [# <oneline>]
# .       create a merge commit using the original merge commit's
# .       message (or the oneline, if no original merge commit was
# .       specified). Use -c <commit> to reword the commit message.
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
```

Change `pick` for the commits you want to squash to `s`

```
pick 0000000 one commit message
s 0000000 another commit message
s 0000000 yet another commit message
```

Save and quit. You will now get a second popup to create the squash commit

```
# This is a combination of 3 commits.
# This is the 1st commit message:
one commit message
# This is the commit message #2:
another commit message
# This is the commit message #3:
yet another commit message
[...]
```

Remove the extra commits that you are squashing leaving just the one you want to keep

```
# This is a combination of 3 commits.
# This is the 1st commit message:
one commit message
[...]
```

Save and quit again then run the following to complete the squash task

```
git push --force
```



## Write meaningful commit messages

This is something many take for granted and rarely do right. When I started learning git I was shown the easy way to write a commit message `git commit -am "fixed two build-breaking issues"`. I have since learned that you should make your commit messages meaningful so that it's easier to read the changes in the change history instead of having to open each change to read the description **if** one was added. I now run `git add` independently, then `git commit` which opens my editor where I can add a title and a more detailed description.

Good meaningful messages are written in present tense. I write my commits by keeping this in my head `This commit will` and continue my title from there, like `Unpin the dd-trace gem version`.

* Write your commit message in the imperative: "**Fix**", "**Add**", "**Change**" instead of "**Fixed**", "**Added**", "**Changed**" for example.
* Don't end the summary line with a period - it's a title and titles don't end with a period.

**Per Git**:

> As a general rule, your messages should start with a single line that’s no more than about 50 characters and that describes the changeset concisely, followed by a blank line, followed by a more detailed explanation.
Here is a template you can follow

```
Capitalized, short (50 chars or less) summary
More detailed explanatory text, if necessary.  Wrap it to about 72
characters or so.  In some contexts, the first line is treated as the
subject of an email and the rest of the text as the body.  The blank
line separating the summary from the body is critical (unless you omit
the body entirely); tools like rebase will confuse you if you run the
two together.
Write your commit message in the imperative: "Fix bug" and not "Fixed bug"
or "Fixes bug."  This convention matches up with commit messages generated
by commands like git merge and git revert.
Further paragraphs come after blank lines.
- Bullet points are okay, too
- Typically a hyphen or asterisk is used for the bullet, followed by a
  single space, with blank lines in between, but conventions vary here
- Use a hanging indent
Resolves: #123
```

Others write commit messages like this:

```
<type>: <description>
[optional body]
[optional footer]
```

or for issue tracking:

```
<issue ID> - <type> <description>
<body>
```

with type being one of the following:

* feat: feature
* fix: a bug fix
* docs: a change to documentation
* style: a change to style, formatting, typing
* perf: a change that improves performance
* test: a change that adds testing
* chore: a change in the build process


## Branches

One of the best advantages of using Git is its branching feature, use it. I've been guilty of cutting corners in this regard. Best practice is to never commit directly to master.

* Create a new branch for each new feature `git branch -b <branch_name>`
    * Use meaningful names for your branches
* Checkout your new branch: `git checkout <branch_name>`
* Keep your branches up to date
* Delete stale branches `git branch -d branchName`

## Know your options

There are many options you can use when working in Git, it may be hard to remember them all and that's ok. `git help -a` is your friend.


## Additional Reading

* [Commit Often, Perfect Later, Publish Once](https://sethrobertson.github.io/GitBestPractices/#commit)
* [Semantic Commit Messages](https://seesparkbox.com/foundry/semantic_commit_messages)
* [A successful Git branching model](https://nvie.com/posts/a-successful-git-branching-model/)
* [Version Control Best Practices](https://www.git-tower.com/learn/git/ebook/en/command-line/appendix/best-practices/)
* [Atomic commits](https://en.wikipedia.org/wiki/Atomic_commit)
* [Single-responsibility principle](https://en.wikipedia.org/wiki/Single-responsibility_principle)
