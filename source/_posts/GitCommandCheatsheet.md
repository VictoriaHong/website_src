---
title: Git Command Cheatsheet
date: 2016-05-02 18:38:41
tags: [Git, Notes]
categories:
- Tech Notes
---

## My A to Z

Help yourself with these cookies.

<!--more-->

### Concepts

**HEAD** is the symbolic name for the currently checked out commit -- it's essentially what commit you're working on top of.

`git check branch_name` will sync HEAD with latest branch. Check out a commit will attach HEAD to that commit.

### Commit

- `git checkout origin master && git commit`

	![](http://i.imgur.com/HeNywjX.png)

### Reverse Changes

- `git reset HEAD~1`

	Reset local current commit to 1 step before HEAD.

- `git revert HEAD~1`

	Revert remote pushed commit to 1 step before HEAD.

- `git fetch && git rebase origin master && git push`

	`git pull --rebase` is short for `git fetch && git rebase`

![](http://i.imgur.com/Cmu9UdI.png)

### Branch

- `git checkout -b new_branch`

	Create a new branch and check out.

- `git branch -d my_branch`

	Delete a new branch.

- `git checkout my_branch`

	Before checking out to another branch, commit or revert changes. You branch becomes visible only if you push it to origin branch.

- `git reset --hard HEAD`

	Discard all uncommitted changes at local repo.

- `git fetch origin`

	`git reset --hard origin master`

	Discard all committed changes at local repo.

- `git merge my_branch`

	Check out master branch and then merge your own branch to master.

- `git add <filename>`

	After solving all conflicts, mark as merged.

- `git checkout branch_name^`

	Caret(^) is relative ref to parent. ^^ is to grandparent. Alternative to hash.

- `git checkout HEAD~4`

	Equivalent to HEAD^^^^.

- `git branch -f master HEAD~3`

	Moves (by force) the master branch to three parents behind HEAD.


### Fetch Data

- `git fetch`

	Downloads the commits that the remote has but are missing from our local repository, and updates where our remote branches point (for instance, origin master).

	Git fetch, however, does not change anything about your local state. It will not update your master branch or change anything about how your file system looks right now.

- `git pull`

	Equivalent to `git fetch && git merge`

	![](http://i.imgur.com/7nCIfHw.png)


### Tricks

- `git mv old_name new_name && git commit -m && git push origin master`

	Git can't track file renaming but only content modification. So deal with it separately. Do it at the dir where the files locate.

- `git diff <source_branch> <target-branch>`

	Check difference between two versions.

- `git log`

	Get id for your commit.

- `git tag 1.0.0 <id>`

	Id can be less than 10 characters but should be unique.

- `git rebase master`

	![](http://i.imgur.com/KDFUWgL.png)

- `git cherry-pick c1 c2 c3`

	Git cherry-pick is great when you know which commits you want (and you know their corresponding hashes).

	![](http://i.imgur.com/SRF66QK.png)

- `git rebase -i HEAD~4`

	Interactive rebase. From current to 4 steps before, choose the commits you need and order them. New branch starts from 4 step before.

	![](http://i.imgur.com/ura6v9g.png)

- `git rebase -i && git commit --amend`

	We used rebase -i to reorder the commits. Once the commit we wanted to change was on top, we could easily --amend it and re-order back to our preferred order.

- `git fakeTeamwork`

	Fake teamwork on remote repo.

![](http://i.imgur.com/ynxS8Ap.png)

### Syncing a fork

	- [Configuring a remote for a fork](https://help.github.com/articles/configuring-a-remote-for-a-fork/)

	- [Syncing a fork](https://help.github.com/articles/syncing-a-fork/)

#### Reference:

+ [Learn Git Branching](http://pcottle.github.io/learnGitBranching/)
