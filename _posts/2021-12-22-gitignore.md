---
title: gitignore
author:
  name: SilentGlasses
  link: https://github.com/SilentGlasses
date: 2021-12-22 10:25:00
categories: [DevOps, Git]
tags: [git, utilities, devops, versioning]
---

## Introduction

The `.gitignore` file is a simple text file that tells Git which files and folders in a repo it should ignore. These files can be stored at the repo level or at the global level. To get a good setup for your needs, you can use [gitignore.io](https://www.gitignore.io/) to create your file with the entries you need.


**WARNING**: Files already being tracked by Git are not affected by adding this file.

To use a global `.gitignore` file, create the file in your home directory:

```
touch ~/.gitignore_global
```

Once you have the file created, run the following command to add it to your git configuration:

```
git config --global core.excludesfile ~/.gitignore_global
```

## Structure

Ignores in your `.gitignore` are made one entry per line.

* Lines that start with `#` are comments.
* Escape key `\` are used when patterns start with `#`.
* The slash `/` is used as the directory separator.
* An asterisk `*` matches anything except a slash.
* The character `?` matches any one character except `/`.
* The range notation, e.g. `[a-zA-Z]`, can be used to match one of the characters in a range.
* A leading `**/` followed by a slash means match in all directories.
* A trailing `/**` matches everything inside.
* A slash followed by two consecutive asterisks then a slash eg. `a/**/b` matches zero or more directories.

## Example .gitignore file

This is a basic `.gitignore` file. There are numerous things you can add depending on the OS you use down to the language you develop on that will determine the entries in your `.gitignore` file. You may even choose to have a setup where you have multiple `name.gitignore` type structure which I will not cover.

```
# Created by https://www.toptal.com/developers/gitignore/api/ruby,jekyll,macos
# Edit at https://www.toptal.com/developers/gitignore?templates=ruby,jekyll,macos

# MkDocs documentation
site/
# Packages
*.7z
*.dmg
*.gz
*.iso
*.jar
*.rar
*.tar
*.zip

### macOS ###
# General
.DS_Store
.AppleDouble
.LSOverride

# Icon must end with two \r
Icon

# Thumbnails
._*

# Files that might appear in the root of a volume
.DocumentRevisions-V100
.fseventsd
.Spotlight-V100
.TemporaryItems
.Trashes
.VolumeIcon.icns
.com.apple.timemachine.donotpresent

# Directories potentially created on remote AFP share
.AppleDB
.AppleDesktop
Network Trash Folder
Temporary Items
.apdisk

### Jekyll ###
_site/
.sass-cache/
.jekyll-cache/
.jekyll-metadata

### Ruby ###
*.gem
*.rbc
/.config
/coverage/
/InstalledFiles
/pkg/
/spec/reports/
/spec/examples.txt
/test/tmp/
/test/version_tmp/
/tmp/

# Used by dotenv library to load environment variables.
# .env

# Ignore Byebug command history file.
.byebug_history

## Specific to RubyMotion:
.dat*
.repl_history
build/
*.bridgesupport
build-iPhoneOS/
build-iPhoneSimulator/

## Specific to RubyMotion (use of CocoaPods):
#
# We recommend against adding the Pods directory to your .gitignore. However
# you should judge for yourself, the pros and cons are mentioned at:
# https://guides.cocoapods.org/using/using-cocoapods.html#should-i-check-the-pods-directory-into-source-control
# vendor/Pods/

## Documentation cache and generated files:
/.yardoc/
/_yardoc/
/doc/
/rdoc/

## Environment normalization:
/.bundle/
/vendor/bundle
/lib/bundler/man/

# for a library or gem, you might want to ignore these files since the code is
# intended to run in multiple environments; otherwise, check them in:
# Gemfile.lock
# .ruby-version
# .ruby-gemset

# unless supporting rvm < 1.11.0 or doing something fancy, ignore this:
.rvmrc

# Used by RuboCop. Remote config files pulled in from inherit_from directive.
# .rubocop-https?--*

# End of https://www.toptal.com/developers/gitignore/api/ruby,jekyll,macos
```
