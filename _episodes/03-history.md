---
title: "Looking at history and differences"
teaching: 25
exercises: 0
questions:
- "How do I get started with Git?"
- "Where does Git store information?"
- "What is a branch?"
- "How can I merge changes from another branch?"
- "Reverting changes"
objectives:
- "Be able to view history of changes to a repository"
- "Be able to view differences between commits"
- "Understand how and when to use tags to label commits"
- "Know what branches are and why you would use them"
- "Understand how to merge branches"
- "Understand how to resolve conflicts during a merge"
keypoints:
- "`git log` shows the commit history"
- "`git diff` displays differences between commits"
- "`git checkout` recovers old versions of files"
- "`HEAD` points to the commit you have checked out"
- "`master` points to the tip of the `master` branch"
- "`git branch` creates a new branch"
- "Use feature branches for new ideas and fixes, before merging into `master`"
- "merging does not delete any branches"
---


### Looking at differences

We should [reference some previous work][reference-previous-work] in the introduction section.
Make the required changes, save both files but do not commit the changes yet.
We can review the changes that we made using:

```
$ atom article.md		# Cite previous studies in introduction
$ atom refs.txt		# Add the reference to the database
$ git diff 			# View changes
```

This shows the difference between the latest copy in the repository and the
unstaged changes we have made.

* `-` means a line was deleted.
* `+` means a line was added.
* Note that a line that has been edited is shown as a removal of the old line and an
addition of the updated line.

Looking at differences between commits is one of the most common activities.
The `git diff` command itself has a number of [useful
options](http://git-scm.com/docs/git-diff.html).

There is also a range of GUI-based tools for looking at differences and
editing files. For example:

* [Diffmerge](https://sourcegear.com/diffmerge/) (Free, cross-platform)
* [WinMerge](http://winmerge.org/) - open source tool available for Windows;
* GitHub [Compare
view](https://help.github.com/articles/comparing-commits-across-time)

Git can be configured to use graphical diff tools, and this is functionality
is accessed using `git difftool` in place of `git diff`.
Configuring a visual diff tool is covered on the
[hints and tips](11-hints-and-tips.html) page.
The choice of GUI for viewing differences depends on the context in which you
are working and your own preferences related to choosing tools and
technologies.

Now commit the change we made by adding the second reference:
```
$ git add article.md refs.txt
$ git commit			# "Cite previous work in introduction"
```

### Looking at our history

To see the history of changes that we made to our repository (the most recent
changes will be displayed at the top):

```
$ git log

commit 3eac70f1e09f003c26783d2467e8c7d9a4f6826e (HEAD -> master)
Author: Selim Atay <youremail@yourdomain.com>
Date:   Sun Mar 21 09:51:55 2021 +0300

    cite previous work in introduction

commit 635f24bf13cab059e9f04db28a9d3008af4b837c 		# COMMITID
Author: Selim Atay <youremail@yourdomain.com>
Date:   Fri Mar 19 19:52:28 2021 +0300

    write introduction section

commit 537997c4b1837237a1f9c54a3d9dda88995eb256 		# COMMITID
Author: Selim Atay <youremail@yourdomain.com>
Date:   Fri Mar 19 18:51:06 2021 +0300

    add title and authors

```


The output shows (on separate lines):
- the commit identifier (also called revision number) which
uniquely identifies the changes made in this commit
- author
- date
- your commit message

Git automatically assigns an identifier (e.g. 635f24...) to each commit
made to the repository
--- we refer to this as *COMMITID* in the code blocks below.
In order to see the changes made between any earlier commit and our
current version, we can use  `git diff` followed by the commit identifier of the
earlier commit:

```
$ git diff COMMITID		# View differences between current version and COMMITID
```

And, to see changes between two commits:

```
$ git diff OLDER_COMMITID NEWER_COMMITID
```

Using our commit identifiers we can set our working directory to contain the
state of the repository as it was at any commit. So, let's go back to the very
first commit we made,

```
$ git log
$ git checkout INITIAL_COMMITID
```

We will get something like this:

```
Note: checking out '3eac70f'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by performing another checkout.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -b with the checkout command again. Example:

  git checkout -b new_branch_name

HEAD is now at 3eac70f... Add title and authors
```

 'detached HEAD' is a strange concept in git

If we look at `article.md` we'll see it's our very first version. And if we
look at our directory,

```
$ ls
```
```
article.md
```


then we see that our `refs.txt` file is gone. But, rest easy, while it's
gone from our working directory, it's still in our repository. We can jump back
to the latest commit by doing:

```
$ git checkout master
```

And `refs.txt` will be there once more,

```
$ ls
```
```
article.md refs.txt
```

So we can get any version of our files from any point in time. In other words,
we can set up our working directory back to any stage it was when we made
a commit.

### The `HEAD` and `master` pointers

*HEAD* is a reference, or pointer, which points to the branch at the commit where
you currently are.
We said previously that `master` is the default branch. But `master` is
actually a pointer - that points to the tip of the `master` branch (the sequence
of commits that is created by default by Git). You may think of `master` as two
things:

1. a pointer
2. the default branch.

Before we checked out one of the past commits, the *HEAD* pointer was pointing to
`master` i.e. the most recent commit of the `master` branch.
After checking out one of the past commits, *HEAD* was pointing to that commit i.e.
not pointing to master any more.
That is what Git means by a 'detached HEAD' state and advises us that if we want to make a commit
now, we should create a new branch to retain these commits.

![Checking out a previous commit - detached head](../fig/detached-head.svg)

If we created a new commit without first creating a new branch, i.e. working from
the 'detached HEAD' these commits would not overwrite any of our existing work,
but they would not belong to any branch. In order to save this work, we would need
to checkout a new branch. To discard any changes we make from the detached HEAD
state, we can just checkout master again.

## Visualizing your own repository as a graph
If we use `git log` with a couple of options, we can display the history as a graph,
and decorate those commits corresponding to Git references (e.g. `HEAD`, `master`):

```
$ git log --graph --decorate --oneline

* 3eac70f (HEAD -> master) cite previous work in introduction
* 635f24b write introduction section
* 537997c add title and authors

```

Notice how `HEAD` and `master` point to the same commit.
Now checkout a previous commit again, and look at the graph again.
We can display, this time specifying that we want to look at `--all` the history,
rather than just up to the current commit.

```
$ git checkout HEAD~				# This syntax refers to the commit before HEAD
$ git log --graph --decorate --oneline --all

* 3eac70f (master) cite previous work in introduction
* 635f24b (HEAD) write introduction section
* 537997c add title and authors
```

Notice how `HEAD` no longer points to the same commit as `master`.
Let's return to the current version of the project by checking out `master` again.

```
$ git checkout master
```

### Using tags as nicknames for commit identifiers

Commit identifiers are long and cryptic. Git allows us to create tags, which
act as easy-to-remember nicknames for commit identifiers. Which makes to track the changes in a structured way

For example,

```
$ git tag article_STUB
```

We can list tags by doing:

```
$ git tag
```

Let's explain to the reader [why this research is important][give-context]:

```
$ atom article.md	# Give context for research
$ git add article.md
$ git commit -m "Explain motivation for research" article.md
```

We can checkout our previous version using our tag instead of a commit
identifier.

```
$ git checkout article_STUB
```

And return to the latest checkout,

```
$ git checkout master
```

> ## Top tip: tag significant events
> When do you tag? Well, whenever you might want to get back to the exact
> version you've been working on. For a article, this might be a version that has
> been submitted to an internal review, or has been submitted to a conference.
> For code this might be when it's been submitted to review, or has been
> released.
{: .callout}


### What is a branch?

You might have noticed the term *branch* in status messages:

```
$ git status

On branch master
nothing to commit (working directory clean)
```


and when we wanted to get back to our most recent version of the repository, we
used `git checkout master`.

Not only can our repository store the changes made to files and directories, it
can store multiple sets of these, which we can use and edit and update in
parallel. Each of these sets, or parallel instances, is termed a `branch` and
`master` is Git's default branch.

A new branch can be created from any commit. Branches can also be *merged*
together.

### Why are branches useful?
Suppose we've developed some software and now we want to
try out some new ideas but we're not sure yet whether we'll keep them. We
can then create a branch 'feature1' and keep our `master` branch clean. When
we're done developing the feature and we are sure that we want to include it
in our program, we can merge the feature branch with the `master` branch.
This keeps all the work-in-progress separate from the `master` branch, which
contains tested, working code.

When we merge our feature branch with master git creates a new commit which
contains merged files from master and feature1. After the merge we can continue
developing. **The merged branch is not deleted.** We can continue developing (and
making commits) in feature1 as well.

### Branching workflows

One popular model is the [Gitflow model](http://nvie.com/posts/a-successful-git-branching-model/):

- A `master` branch, representing a released version of the code
- A release branch, representing the beginnings of the next release - a branch
where the code is still undergoing testing
- Various feature and/or developer-specific branches representing
work-in-progress, new features, bug fixes etc

For example:

![Feature branches ([image source)](https://www.atlassian.com/git/tutorials/comparing-workflows#feature-branch-workflow)](../fig/feature-branch.svg)

There are different possible workflows when using Git for code development.
If you want to learn more about different workflows with Git, have a look at
[this discussion](https://www.atlassian.com/git/tutorials/comparing-workflows)
on the Atlassian website.


### Branching in practice

One of our colleagues wants to contribute to the article but is not quite sure
if it will actually make a publication. So it will be safer to create a branch
and carry on working on this "experimental" version of the article in a branch
rather than in the master.

```
$ git checkout -b methodology

Switched to a new branch 'methodology'
```


We're going to change the title of the article and update the author list (adding John Smith).
However, before we get started it's a good practice to check that we're working
on the right branch.

```
$ git branch			# Double check which branch we are working on

  master
* methodology
```


The * indicates which branch we're currently in. Now let's [make the changes][change-title] to the article.

```
$ atom article.md		# Change title and add co-author
$ git add article.md
$ git commit			# "Modify title and add John as co-author"
```

If we now want to work in our `master` branch. We can switch back by using:

```
$ git checkout master

Switched to branch 'master'
```


Having written some of the article, we have thought of a [better title][aircraft-title] for
the `master` version of the article.

```
$ atom article.md		# Rewrite the title
$ git add article.md
$ git commit			# "Include aircraft in title"
```

### Merging and resolving conflicts

We are now working on two articles: the main one in our `master` branch and the one
which may possibly be collaborative work in our "methodology" branch.
Let's [add another section][methodology-section] to the article to write about John's methodology.

```
$ git checkout methodology	# Switch branch
$ atom article.md		# Add 'methodology' section
$ git add article.md
$ git commit -m "Add methodology" article.md
```

At this point let's visualise the state of our repo,
and we can see the diverged commit history reflecting the recent work
on our two branches:

```
git log --graph --all --oneline --decorate

* cc8efe9 (HEAD -> methodology) Add methodology
* 9a7dc94 Add methodology
| * 69fefc9 (master) Include git in title
|/  
* 4714690 Explain motivation for research
* 3eac70f (tag: article_STUB) cite previous work in introduction
* 635f24b write introduction section
* 537997c add title and authors

```


After some discussions with John we decided that we will publish together,
hence it makes sense to now merge all that was authored together with John
in branch "methodology".
We can do that by *merging* that branch with the `master` branch. Let's try
doing that:

```
$ git checkout master		# Switch branch
$ git merge methodology		# Merge methodology into master

Auto-merging article.md
CONFLICT (content): Merge conflict in article.md
Automatic merge failed; fix conflicts and then commit the result.
```


Git cannot complete the merge because there is a conflict - if you recall,
after creating the new branch, we changed the title of the article on both branches.
We have to resolve the conflict and then complete the merge. We can get
some more detail

```
$ git status

On branch master
You have unmerged paths.
  (fix conflicts and run "git commit")

Unmerged paths:
  (use "git add <file>..." to mark resolution)

    	both modified:      article.md
```

Let's look inside article.md:

```
# Title
<<<<<<< HEAD
The most amazing git and github article with the best title
=======
The most amazing git and github article ever
>>>>>>> methodology
```

The mark-up shows us the parts of the file causing the conflict and the
versions they come from. We now need to manually edit the file to resolve the
conflict. This means removing the mark-up and doing one of:

- Keep the current version, which is the one marked-up by HEAD i.e.
"The most amazing git and github article with the best title"

- Keep the version from the other branch, which is the one marked-up by methodology
i.e. "The most amazing git and github article ever"

- Or manually edit the line to something new which might combine some elements
of the two e.g. "The most amazing git and github article with the best title ever"

[We edit the file][resolve-conflict]. Then commit our changes:

```
$ atom article.md		# Resolve conflict by editing article.md
$ git add article.md		# Let Git know we have resolved the conflict
$ git commit
```

This is where version control proves itself better than DropBox or GoogleDrive,
this ability to merge text files line-by-line and highlight the conflicts
between them, so no work is ever lost.

We can see the two branches merged if we take another look at the log graph:

```
$ git log --graph --decorate --all --oneline

*   1c90e39 (HEAD -> master) Merge branch 'methodology'
|\  
| * cc8efe9 (methodology) Add methodology
| * 9a7dc94 Add methodology
* | 69fefc9 Include git in title
|/  
* 4714690 Explain motivation for research
* 3eac70f (tag: article_STUB) cite previous work in intriduction
* 635f24b write introduction section
* 537997c add title and authors
```



### Discarding local changes

Maybe we made our change just to see how something looks, or to
quickly try something out. But we may be unhappy with our changes. If we
haven't yet done a `git add` we can just throw the changes away and return
our file to the most recent version we committed to the repository by using:

```
$ atom article.md		# Make some small edits to the file
$ git checkout article.md		# Discard edits we just made
```

and we can see that our file has reverted to being the most up-to-date one in
the repository:

```
$ git status			# See that we have a clean working directory
$ atom article.md		# Inspect file to verify changes have been discarded
```

---

### Amending the most recent commit

If you just made a commit and realised that either you did it a bit too early
and the files are not yet ready to be commited. Or, which is not as uncommon as
you think, your commit message is not as it is supposed to be. You can fix that
using the command `git commit --amend`

This opens up the default editor for Git which includes the previous commit
message - you can edit it and close the editor. This will simply fix the commit
message.

But what if we forgot to include some files in the commit?

Let's try it on our example. First, let's [modify two files][describe-methodology]: our article file and
the references file. We will add a methodology section to the article where we
detail the model used for the methodology, and add a reference for this to the references
file.

```
$ atom article.md		# Add methodology section, including a reference to model
$ atom refs.txt		# Add new reference for the model used
$ git status			# Get a status update on file modifications

$ On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   refs.txt
	modified:   article.md

no changes added to commit (use "git add" and/or "git commit -a")
```


Let's then add and commit *article.md* but **not the references file**.

```
$ git add article.md		 # Add article to staging area
$ git commit -m "Describe methodology"
```

Let's have a look at our working directory now:

```
$ git status

$ On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout --	<file>..." to discard changes in working directory)

	modified:   refs.txt

no changes added to commit (use "git add" and/or "git commit -a")
```


Also, run `git log -2` to see what is the latest commit message and ID.

Now, we want to fix our commit and add the references file.

```
$ git add refs.txt	# Add reference file
$ git commit --amend		# Amend most recent commit
```

This will again bring up the editor and we can amend the commit message if required.

Now when we run `git status` and then `git log` we can see that our Working
Directory is clean and that both files were added.

```
$ git status
$ git log -3
```

---

### `git revert` (undo changes associated with a commit)

`git revert` removes the changes applied in a specified commit. However, rather
than deleting the commit from history, git works out how to undo those changes
introduced by the commit, and appends a new commit with the resulting content.

Let's try it on our example. Modify the article, [describing Github][Github], and then make a commit.

```
$ atom article.md		# Describe other instrument
$ git add article.md
$ git commit -m "Describe Github"
```

We now realise that what we've just done in our journal article is incorrect
because we are not using the data from that instrument.
Some of the data got corrupted, and due to problems with the logging computer
we are not going to use that data.
So it makes sense to abandon the commit completely.

```
$ git revert HEAD		# Undo changes introduced by most recent commit
```

When we revert, a new commit is created. The *HEAD* pointer and the branch
pointer are in fact moved forward rather than backwards. 	

We can revert any previous commit. That is, we can "abandon" any of the
previous changes. However, depending on the changes we have made since,
we may bump into a *conflict* (which we will cover in more detail later on).
For example:

```
error: could not revert 848361e... Describe Github
hint: after resolving the conflicts, mark the corrected paths
hint: with 'git add <paths>' or 'git rm <paths>'
hint: and commit the result with 'git commit'
```


Behind the scenes Git gets confused trying to merge the commit *HEAD* is pointing
to with the past commit we're reverting.

So we have seen that `git revert` is a non-destructive way to undo a commit.
What if we don't want to keep a record of undoing commits? That would give a neater
history. `git reset` can also be used to undo commits, but it does so by deleting
history.

---

### `git reset --hard` (restore a previous state by deleting history)

`git reset` has several uses, and is most often used to unstage files from the staging
area i.e. `git reset` or `git reset <file>`.

We are going to use a variant `git reset --hard <commit>` to reset things to how
they were at `<commit>`. This is a permanent undo which deletes all changes more recent
than `<commit>` from your history. There is clearly potential here to lose work, so use
this command with care.

Let's try that on our article, using the same example as before. Now we have two commits
which we want to abandon: the commit outlining the unreliable instrumentation, and
the subsequent revert commit. We can achieve this by resetting to the last
commit we want to keep.

We can do that by running:

```
$ git reset --hard HEAD~2	# Move tip of branch to two commits before HEAD

HEAD is now at fbdc44b Add methodology section and update references file
```


This moves the tip of the branch back to the specified commit. If we look in-depth,
this command moves back two pointers: `HEAD` and the pointer to the tip of the
branch we currently are working on (master). (`HEAD~` = the commit right before HEAD;
`HEAD~2` = two commits before HEAD)

The final effect is what we need: we abandoned the commits and we are now back
to where we were before making the commit about the data we are not using.

---
Click for an [animation] of the revert and reset operations we just used.

[animation]: https://learngitbranching.js.org/?NODEMO&command=git%20commit;git%20revert%20C2;git%20reset%20HEAD~2

[This article](https://git-scm.com/book/en/v2/Git-Tools-Reset-Demystified) discusses more in
depth `git reset` showing the differences between the three options:

* `--soft`
* `--mixed`
* `--hard`


> ## Top tip: do not use `git reset` with remote branches
> There is one important thing to remember about the `reset` command - it
> should only be used with branches that have not been shared yet (that is they
> haven't been pushed into a remote repository that others are using). Resetting
> is changing the history **without** leaving trace. This is always a bad practice
> when using remote repositories and can lead to a horrible mess.
>
> Reverting records the fact of "abandoning the commit" in the history.
> When we revert in a branch that is shared with others and then push that branch
> into the remote repository, it is as if we "came clean" about what we were
> doing. Everyone who pulls the branch in which we reverted changes will see it.
> With `git reset` we "keep it secret" that we have undone some changes.
>
> As such, if we want to abandon changes in branches that are shared
> with others, we should to use the `revert` command.
{: .callout}

![Reset vs revert](../fig/git-revert-vs-reset.svg)


See this [Atlassian online tutorial](
https://www.atlassian.com/git/tutorials/resetting-checking-out-and-reverting/commit-level-operations)
for further reading about the differences between `git revert` and `git reset`.

### How to undo almost anything with Git
See [this blog post](https://github.com/blog/2019-how-to-undo-almost-anything-with-git) for more example scenarios and how to recover from them.
