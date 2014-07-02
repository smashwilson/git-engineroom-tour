---
title: Git, the Engine Room Tour
---

# Git: The Engine Room Tour

![an engine](images/RR_Merlin_labeled.jpg)

*Ash Wilson, Rackspace, DRG*

<!--
intended for people who use git, but are somewhat mystified by it
not an introduction: that would be a half-day workshop or so
not a laundry list of git commands
-->

---
=data-y="1000"

## Git is *weird*.

No, really.

* Lots of **strange nomenclature**.
* Odd **division of labor** among commands.
* Strange **error messages**.

Especially if you learned it by rote!

<!--
devs have two learning modes: "quick as possible" and "in-depth"
git is pretty elegant if you learn it in-depth;
 it's ugly and weird if you learn it as quick as possible!
you're not alone!
-->

---
=data-y="2000"

# Outline

1. The Object Database
2. The Index and The Working Copy
3. Remotes
4. The Git Way

<!--
here's the plan: start with git's internals, its data structures and concepts;
then introduce how git sees your code;
then cover shipping code around the network;
and finally touch on some more philosophic points.
-->

---
=data-x="2000" data-y="2500"

# .git

`git` stores *everything* in a single `.git/` directory.

* local **configuration**
* your **full history**
* **branches**, **tags**, the works

### some terminology:

Each git repository is called a *clone*.

*bare* clones don't have a working copy.

<!--
git is distributed, so every repository is independent and self-contained.
just about everything is plain-text, maybe compressed.
if you're brave, you can drive git using nothing but a text editor!
-->

---
=data-x="2000" data-y="3500"

# .git/objects

at `git`'s heart is a filesystem-based **content-addressable database**.
if you peek inside, here's what you'll see:

```bash
$ find .git/objects/ -type f
.git/objects/05/b6bc38da385b07652d2d09123719193e77b1b8
.git/objects/0f/3a0b8da1742a091c822376291abceb01cda3d6
.git/objects/13/a6fbc30e1e85a2641083427bc4b2075a45ebf3
.git/objects/15/796067f1a8d405eb728206d06fef3c100ce496
.git/objects/1b/72f857b356aa5622374593fa78bff9a585a42e
.git/objects/25/9e166d36e8a9e8a0235af3a1aa2d3a2592ed1a
```

<!--
"things" in regular files. each name 40 hex chars, a sha1 checksum of its contents
deflate-compressed.
-->

---
=data-x="2000" data-y="4500"

# .git/objects: blobs

### files you store in git become *blobs*

![blob diagram](images/blob.png)

<!--
format: "blob\t{size in bytes}\n{content}"
no metadata: no filename, mode, whatever
-->

---
=data-x="2000" data-y="5500"

# .git/objects: trees

### directories are stored as *trees*

![tree diagram](images/tree.png)

<!--
list of sha1 checksums of other tree objects or blobs
stored as simple, \0-separated records: kind, type, sha1, name
-->

---
=data-x="2000" data-y="6500"

# .git/objects: commits

### *commits* are snapshots of the root tree at a particular point in time

![commit diagram](images/commit.png)

<!--
commits point to:
0..n parent commits;
a root tree;
your name, email;
and your commit message.
then these form a big graph. all git does is work with this graph!
-->

---
=data-x="2000" data-y="7500"

# .git/refs: branches and tags

### *branches* and *tags* store a commit's address

![branch and tag diagram](images/branches.png)

### `.git/HEAD` is a *pointer to a branch*

## ... and that's basically everything

<!--
literally a text file holding a sha1 hash in hex
branch: "moves along" when you make new commits
tag: just a marker
HEAD tells git the "current branch" so it knows which one to move!
it is literally a text file that holds "refs/heads/master"
-->

---
=data-x="3000" data-y="2500"

# the Index and the Working Copy

while you're working, there are three places code can be:

![three trees](images/three-trees.png)

`git status` will tell you what's in each.

<!--
git has this thing called the "index" between HEAD and your code on disk.
basically, this lets you make partial commits.
-->

---
=data-x="3000" data-y="3500"

# Working Copy -> Index

![git add](images/git-add.png)

`git add -- path/to/file`: stage one file at a time

`git add -p`: stage diffs

<!--
"git add" moves changes from your working copy to the index.
you can stage parts of files, the whole directory
-->

---
=data-x="3000" data-y="4500"

# Index -> .git/objects

![git commit](images/git-commit.png)

`git commit` persists the index in `.git/objects`, makes a new commit object,
and points the `HEAD` branch to it.

<!--
"git commit" makes a new commit out of whatever you've staged with "git add".
notice that it drags `HEAD` along with it.
-->

---
=data-x="3000" data-y="5500"

# .git/objects --> Working Copy

![git checkout](images/git-checkout.png)

`git checkout -- path/to/file`: check out a single file

`git checkout -p`: check out individual diffs

<!--
"git checkout" turns your working copy (and index) into another state, like
  a branch.
if the state you name is a branch, it also moves HEAD!
"detached HEAD" means "you checked out something that can't move"
"git reset --hard" is similar: but it yanks HEAD along with you.
-->

---
=data-x="4000" data-y="3000"

# *Sidenote:* How to Name Things

git gives you a bunch of ways to name commits.

* giant 40-character sha1 checksums: `ca0d1ce34c23e9c2c875ae7c33a742e12ad1ff82`
* any unique prefix: `ca0d1c`
* branches or tags: `master`, `my-feature`
* parent-of operator: `my-feature^`, `master^^`, `some-branch~5`
* by time, absolute or relative: `master@{1 day ago}`
* regexp search on commit messages: `:/broken`

`git help rev-parse` has the full list!

<!--
almost all of the time you use ref names... but these other ones are handy too.
-->
