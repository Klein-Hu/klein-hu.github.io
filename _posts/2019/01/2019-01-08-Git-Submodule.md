---
layout: post
title: Git Submodules
categories: [Tips]
tags: [Git, Configuration Management]
fullview: false
comments: true
---

In the current project, I need to build gRPC Server and run it in Linux environment. There are several ways to build a gRPC server. I'm not trying to talking more about it here. But during my prototype, I noticed one interesting topic: git submodule, because no matter how you build the gRPC server, where to put the Google gRPC code still a decision you need to make.

For git submodule, there are a few interesting points I want to learn:

1. How to create a submodule;
2. How to select a particular commit/tag/branch;
3. How to delete a submodule;

## Submodule Creation

That's not hard. From the `git submodule` command could easily get the solution. 

```bash
git submodule [--quiet] add [-b <branch>] [-f|--force] [--name <name>] [--reference <repository>] [--] <repository> [<path>]
```

The created submodule will be the latest commit in the defined branch. If the branch is not specified, it will be the default branch.

## How to Select a Particular Commit/Tag/Branch

When the submodule is created, we could only specify the branch. But after we init the submodule, we could go to the folder and `git checkout` a particular commit or tag. After that, the main repo will have a staged change. After that change got committed and pushed, other people in the project could pull the main repo and run `git submodule update`, then the selected commit/tag will be synced down. 

One thing need to pay attenion: The last `git submodule update` is a must-have step to get the latest setup for a particular submodule.

## Submodule Deletion

This is not something clearly mentioned in the documents. What I found from Internet:

1. `git submodule deinit -f -- a/submodule`
2. `rm -rf .git/modules/a/submodule`
3. `git rm -f a/submodule`

I have not tested above steps yet. But keep them here in case in the future need them.

## Reference

1. [Git Tools - Submodules](https://git-scm.com/book/en/v2/Git-Tools-Submodules)
2. [git-submodule - Initialize, update or inspect submodules](https://git-scm.com/docs/git-submodule)
3. [[PATCH v3] submodule: add 'deinit' command](http://git.661346.n2.nabble.com/PATCH-v3-submodule-add-deinit-command-td7576946.html)