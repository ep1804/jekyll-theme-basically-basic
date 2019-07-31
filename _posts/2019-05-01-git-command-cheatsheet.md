---
layout: post
title:  Git command cheetsheet
description: "Git command cheatsheet"
modified: 2019-05-01
tags: [git]
---

Git command cheetsheet

{% raw %}

### add upstream

```bash
git remote add upstream git@....git
```

### rebase dev branch with upstream

```bash
git checkout dev
git fetch upstream
git rebase upstream/dev
git push -f origin dev
```

### rebase issue branch to dev

```bash
git checkout issue#123
git rebase dev

git push -f origin issue#123
```

### rebase issue branch to dev interactively for squash

```bash
git checkout issue#123
git rebase -i hash-of-dev-last-commit
# edit command: pick,s,s,s,...,s

git push -f origin issue#123
```

ref: https://www.internalpointers.com/post/squash-commits-into-one-git

### reset staled local branch with remote branch

```bash
git checkout issue#123
git fetch --all
git reset --hard origin/issue#123
```

### clean up local branches (except master and dev)

```bash
git branch | grep -v "master" | grep -v "dev" | xargs git branch -D
```

{% endraw %}
