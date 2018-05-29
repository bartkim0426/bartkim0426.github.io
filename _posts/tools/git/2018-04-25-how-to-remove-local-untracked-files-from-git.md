---
layout: post
title:  "How to remove untracked local files from git work tree"
categories: git
---

Can use `git clean` command. 

It will cleaning working tree by recursively removeing files that are not under version control. 

Here's few useful options. Full optinos and description is in [here](https://git-scm.com/docs/git-clean). (Git documentation)

```git
# see which files will be deleted. (Just show, not delete)
git clean -n

# clean force (--force)
git clean -f

# clean by interactively (--interactive)
git clean -i
```
