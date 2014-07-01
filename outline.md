# intro

imgs:
 - engine block

* what this talk is and who it's for
* git is weird and looks offputting!
* the key is to learn the insides first, then the outside will make sense.

# .git/objects

imgs:
 - [ ] blob
 - [ ] tree
 - [ ] commit
 - [ ] branch
 - [ ] tag

* git is distributed.
 * each repository lives alone, with full history.
 * terminology: "clones", bare vs. non-bare
 * nothing until part iii touches the network at all!
 * everything sits in .git, most things are plain-text, maybe compressed

* .git/objects - a content-addressable object database
 * "things" in files, filenames are sha1 checksums of contents
 * omg collisions?!

* blobs
 * files you store in git. no metadata, just bytes

* trees
 * directory listing: filenames, modes of children
 * point to blobs or other trees

* commits
 * parent tree + metadata
 * pointer to 0..n parent commits

* .git/refs
 * pointers to commits
 * literally 40-character text files
 * branches
 * tags (non-annotated)
 * also: pointers-to-pointers like HEAD, MERGE_HEAD, FETCH_HEAD

# the Index and the Working Copy

imgs:
 - [ ] .git/objects ; index ; fs
 - [ ] git add
 - [ ] git commit
 - [ ] git checkout
 - [ ] git reset
 - [ ] merge: trivial
 - [ ] merge: ff

* great, but what about your code

* between .git/objects and your real fs sits the index
 * a staging area for building new commits

* fs -> index: `git add`, `git rm`
 * `-p`, single path, etc
* index -> .git/objects: `git commit`
* .git/objects -> index -> fs: `git checkout`
 * `-p`, single path, etc
 * `git reset --hard`: similar, but yanks the branch along with you

* how to name a commit

* merges
 * trivial case
 * fast-forward (only one allowed in bare)
 * recursive
 * conflicts!

* rebasing
 * why rebase?

# remotes

imgs:
 - [ ] network of remotes
 - [ ] fetch
 - [ ] push

* clones can talk to each other
* remote: short name for a URL of another clone
* git gives *eventualy consistency* among network of remotes

* git fetch: somewhere else -> here
  * "remote tracking branches", `refs/remotes/origin/master`
   * read-only (by you)
   * state of a branch in another clone, as of last fetch
  * updates *only* remote tracking branches, does not touch fs

* git push: here -> somewhere else
 * only push to a **bare** clone
 * **must** be a fast-forward merge on the other end

* git pull: git fetch + git merge
 * one difference. fetch stores intermediate result in FETCH_HEAD, not remote
   tracking branches

# the "git way"

* commit deliberately
 * stage in chunks
 * use clear commit messages
* commit often
 * small diffs are more readable
 * tools work best on smaller chunks

# other places to look

* http://git-scm.com/book/en/
