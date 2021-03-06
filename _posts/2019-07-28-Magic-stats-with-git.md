---
layout: post
title: ✨ Magic stats with git
color: rgb(216,174,71)
tags: [git]
---

## What's git log

Git log is a powerful tool in git that allows you to browse through the repository history.
It saves a bunch of information (tag, commit author, message, date, ...)

First you may want to pull origin from every repository using:
```bash
find . -type d -depth 1 -exec git --git-dir={}/.git --work-tree=$PWD/{} pull origin master \;
```

## Some nice alias

List of contributes per commits, 
You can have just the log for the first one `gitlog`, or you want a more detail view with the second one `gitauthor`:

```bash
# gitlog
git shortlog --all -s -n

# gitauthor Get the number of line per contributors
git ls-files | while read f; do git blame -w -M -C -C --line-porcelain "$f" | grep -I '^author '; done | sort -f | uniq -ic | sort -n
```

List of commits between two commits:

```bash
# Get commits at a certain day
git log --pretty='format:%H %an %ae %ai' | grep 2019-07-13  

# Print the commits between the two commit SHA
git log 2fe56fa...efc173d --oneline
```

List of commit message with `#<number>` between two releases:

```bash
# Higher Number in the tag
export LAST_RELEASE=`git tag --sort=-version:refname | sed -n 1p` 
# Second higher number in the tag
export BEFORE_LAST=`git tag --sort=-version:refname | sed -n 2p`  

# Get the logs and use some bash magic to it to get only the unique #<number> worked on. 
git log ${LAST_RELEASE}...${BEFORE_LAST} --oneline | egrep -o "#[0-9]{1,}" | sort | uniq -c | awk '{$1="";print}' | sed 's/^.//'
```

Get list of comment from somebody during a certain time:

```bash
git shortlog -s --author=John --format="%cd: %s" --after="2019-02-12" --before="2019-04-14"
```

Show the last commits made on the project:

```bash
git log --graph --oneline --abbrev-commit --decorate --all
```

Get the commit time of a commit sha:
```bash
git show -s --format=%ct $COMMIT_SHA
```

## Third party tools

Because some people have already thought of everything.
Here are a couple of nice third party script you can use to get some data out of your code base:

  - [gitfame](https://pypi.org/project/git-fame/): Pretty-print git repository collaborators sorted by contributions.
  
```bash
pip install git-fame
git config --global alias.fame "!python -m gitfame"
git fame .
```
