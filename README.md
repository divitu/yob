Yob
===

Yob is a branch history tracker for/in Git.  It only tracks activity on branches
that track remotes.

This software is in its early days.  Many errors are not checked; other errors
are masked.  Use at your own risk.

Manual Installation (sorry)
===========================

To enable Yob, add the files in `src/hooks` to a repository's `.git/hooks`
directory.  Add the location of `src/bin` to your `$PATH`, or add the files
therein to a directory in your `$PATH`.

Wat Do
======

First, wat don't do.  Don't give local branches names different from their
tracking branches.  Don't mess with any of the `yob/` branches.

When you would use `git fetch`, use `yob-fetch` instead.  Remeber that `git
pull` is `git fetch && git merge <TRACKING>`, so don't `git pull`.

That's it.

But Why?
========

Yob is probably useful in two scenarios.  If you need a lot of auditing, but you
trust your developers, Yob will keep track of everything they've done, including
amendments, and divergences they've resolved forcefully.

If you are confused by Git, Yob can help your local Git ninja understand what
mistakes you've made and the best way to correct them.  It can also help recover
data that would otherwise be lost.  In this way, Yob is like trianing wheels.

Okay, How?
==========

Every time you make a commit (including a merge commit), Yob makes another
commit to the Yob branch.  This Yob commit will have the previous Yob commit as
its first parent, and your commit as the other.  If there is no previous Yob
commit, it will create an orphan commit, then make the above commit.

Before you push to a remote, Yob pushes `yob/<REMOTE>` to `yob` on the
remote.

When you fetch with `yob-fetch`, Yob does `git fetch <REMOTE>`, then merges the
remote `yob` with the local `yob/<REMOTE>`.

Consideration
=============

Those who are knowledgeable in Git are probably shouting at me right now that
this scheme will blow up the object store to ridicuous proportions.  You're
probably right, but it's a trade-off for better visibility.  If your application
can't handle the extra overhead, Yob is probably not the solution for you.

To Do
=====

* review code
* handle errors
* test the merge process in `yob-fetch`
* test multiple remotes
* canonicalize remote domain names
* visualisation tool
* `yob-pull`
* allow tracking branches with names different than their remote counterparts
* better Git integration (like `hub`)
* only push once per invocation
