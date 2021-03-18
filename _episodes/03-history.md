---
title: "Looking at history and differences"
teaching: 30
exercises: 0
questions:
- "How do I get started with Git?"
- "Where does Git store information?"
- "What is a branch?"
- "How can I merge changes from another branch?"
- reverting changes
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
-"`git branch` creates a new branch"
- "Use feature branches for new ideas and fixes, before merging into `master`"
- merging does not delete any branches
---
---


### Looking at differences

We should [reference some previous work][reference-previous-work] in the introduction section.
Make the required changes, save both files but do not commit the changes yet.
We can review the changes that we made using:

~~~
$ gedit paper.md		# Cite previous studies in introduction
$ gedit refs.txt		# Add the reference to the database
$ git diff 			# View changes
~~~
{: .language-bash}

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
$ git add paper.md refs.txt
$ git commit			# "Cite previous work in introduction"
```
{: .language-bash}

### Looking at our history

To see the history of changes that we made to our repository (the most recent
changes will be displayed at the top):

~~~
$ git log
~~~
{: .language-bash}

```
commit 8bf67f3862828ec51b3fdad00c5805de934563aa
Author: Your Name <your.name@manchester.ac.uk>
Date:   Mon Jun 26 10:22:39 2017 +0100

    Cite PCASP paper


commit 4dd7f5c948fdc11814041927e2c419283f5fe84c
Author: Your Name <your.name@manchester.ac.uk>
Date:   Mon Jun 26 10:21:48 2017 +0100

    Write introduction

commit c38d2243df9ad41eec57678841d462af93a2d4a5
Author: Your Name <your.name@manchester.ac.uk>
Date:   Mon Jun 26 10:14:30 2017 +0100

    Add author and title
```
{: .output}

The output shows (on separate lines):
- the commit identifier (also called revision number) which
uniquely identifies the changes made in this commit
- author
- date
- your commit message

Git automatically assigns an identifier (e.g. 4dd7f5) to each commit
made to the repository
--- we refer to this as *COMMITID* in the code blocks below.
In order to see the changes made between any earlier commit and our
current version, we can use  `git diff` followed by the commit identifier of the
earlier commit:

~~~
$ git diff COMMITID		# View differences between current version and COMMITID
~~~
{: .language-bash}

And, to see changes between two commits:

~~~
$ git diff OLDER_COMMITID NEWER_COMMITID
~~~
{: .language-bash}

Using our commit identifiers we can set our working directory to contain the
state of the repository as it was at any commit. So, let's go back to the very
first commit we made,

~~~
$ git log
$ git checkout INITIAL_COMMITID
~~~
{: .language-bash}

We will get something like this:

~~~
Note: checking out '21cfbdec'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by performing another checkout.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -b with the checkout command again. Example:

  git checkout -b new_branch_name

HEAD is now at 21cfbde... Add title and authors
~~~
{: .output}

This strange concept of the 'detached HEAD' is covered in the next section ...
just bear with me for now!

If we look at `paper.md` we'll see it's our very first version. And if we
look at our directory,

~~~
$ ls
~~~
{: .language-bash}
~~~
paper.md
~~~
{: .output}

then we see that our `refs.txt` file is gone. But, rest easy, while it's
gone from our working directory, it's still in our repository. We can jump back
to the latest commit by doing:

~~~
$ git checkout master
~~~
{: .language-bash}

And `refs.txt` will be there once more,

~~~
$ ls
~~~
{: .language-bash}
~~~
paper.md refs.txt
~~~
{: .output}
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

## Visualising your own repository as a graph
If we use `git log` with a couple of options, we can display the history as a graph,
and decorate those commits corresponding to Git references (e.g. `HEAD`, `master`):

~~~
$ git log --graph --decorate --oneline
~~~
{: .language-bash}

```
* 6a48241 (HEAD, master) Cite previous work in introduction
* ed26351 Cite PCASP paper
* 7446b1d Write introduction
* 4f572d5 Add title and author
```
{: .output}

Notice how `HEAD` and `master` point to the same commit.
Now checkout a previous commit again, and look at the graph again.
We can display, this time specifying that we want to look at `--all` the history,
rather than just up to the current commit.

```
$ git checkout HEAD~				# This syntax refers to the commit before HEAD
$ git log --graph --decorate --oneline --all
```
{: .language-bash}

```
* 6a48241 (master) Reference second paper in introduction
* ed26351 (HEAD) Reference Allen et al in introduction
* 7446b1d Write introduction
* 4f572d5 Add title and authors
```
{: .output}

Notice how `HEAD` no longer points to the same commit as `master`.
Let's return to the current version of the project by checking out `master` again.

```
$ git checkout master
```
{: .language-bash}

### Using tags as nicknames for commit identifiers

Commit identifiers are long and cryptic. Git allows us to create tags, which
act as easy-to-remember nicknames for commit identifiers.

For example,

```
$ git tag PAPER_STUB
```
{: .language-bash}

We can list tags by doing:

```
$ git tag
```
{: .language-bash}

Let's explain to the reader [why this research is important][give-context]:

```
$ gedit paper.md	# Give context for research
$ git add paper.md
$ git commit -m "Explain motivation for research" paper.md
```
{: .language-bash}

We can checkout our previous version using our tag instead of a commit
identifier.

```
$ git checkout PAPER_STUB
```
{: .language-bash}

And return to the latest checkout,

```
$ git checkout master
```
{: .language-bash}

> ## Top tip: tag significant events
> When do you tag? Well, whenever you might want to get back to the exact
> version you've been working on. For a paper, this might be a version that has
> been submitted to an internal review, or has been submitted to a conference.
> For code this might be when it's been submitted to review, or has been
> released.
{: .callout}

> ## Where to create a Git repository?
> Avoid creating a Git repository within another Git repository.
> Nesting repositories in this way causes the 'outer' repository to
> track the contents of the 'inner' repository - things will get confusing!
{: .callout}

> ## Exercise: "bio" Repository
>
> - Create a new Git repository on your computer called "bio"
> - Be sure not to create your new repo within the 'paper' repo (see above)
> - Write a three-line biography for yourself in a file called **me.txt**
> - Commit your changes
> - Modify one line, add a fourth line, then save the file
> - Display the differences between the updated file and the original
>
> You may wish to use the faded example below as a guide
>
> ```
> cd ..                # Navigate out of the paper directory
>                      # Avoid creating a repo within a repo - confusion will arise!
> mkdir ___            # Create a new directory called 'bio'
> cd ___               # Navigate into the new directory
> git ____             # Initialise a new repository
> _____ me.txt         # Create a file and write your biography
> git ___ me.txt       # Add your biography file to the staging area
> git ______           # Commit your staged changes
> _____ me.txt         # Edit your file
> git ____ me.txt      # Display differences between your modified file and the last committed version
> ```
> {: .language-bash}
>
> > ## Solution
> >
> > ```
> > cd ..                # Navigate out of the paper directory
> >                      # Avoid creating a repo within a repo - confusion will arise!
> > mkdir bio            # Create a new directory
> > cd bio               # Navigate into the new directory
> > git init             # Initialise a new repository
> > gedit me.txt         # Create a file and write your biography
> > git add me.txt       # Add your biography file to the staging area
> > git commit           # Commit your staged changes
> > gedit me.txt         # Edit your file
> > git diff me.txt      # Display differences between your modified file and the last committed version
> > ```
> > {: .language-bash}
> {: .solution}
{: .challenge}

[reference-previous-work]: https://github.com/gcapes/git-course-paper/commit/add59ba7d0b963531195eb3774b79bb9bcd749d1#diff-0403ef06adf405f7b310b4518bd6a3559854f54c61676f676ce9cbfee7172ab6
[give-context]: https://github.com/gcapes/git-course-paper/commit/92f674de54c77afd7ae32342f9f03d2bdd8fb76d#diff-0403ef06adf405f7b310b4518bd6a3559854f54c61676f676ce9cbfee7172ab6
### What is a branch?

You might have noticed the term *branch* in status messages:

~~~
$ git status
~~~
{: .language-bash}
~~~
On branch master
nothing to commit (working directory clean)
~~~
{: .output}

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

One of our colleagues wants to contribute to the paper but is not quite sure
if it will actually make a publication. So it will be safer to create a branch
and carry on working on this "experimental" version of the paper in a branch
rather than in the master.

~~~
$ git checkout -b simulations
~~~
{: .language-bash}
~~~
Switched to a new branch 'simulations'
~~~
{: .output}

We're going to change the title of the paper and update the author list (adding John Smith).
However, before we get started it's a good practice to check that we're working
on the right branch.

~~~
$ git branch			# Double check which branch we are working on
~~~
{: .language-bash}
~~~
  master
* simulations
~~~
{: .output}

The * indicates which branch we're currently in. Now let's [make the changes][change-title] to the paper.

~~~
$ gedit paper.md		# Change title and add co-author
$ git add paper.md
$ git commit			# "Modify title and add John as co-author"
~~~
{: .language-bash}

If we now want to work in our `master` branch. We can switch back by using:

~~~
$ git checkout master
~~~
{: .language-bash}
~~~
Switched to branch 'master'
~~~
{: .output}

Having written some of the paper, we have thought of a [better title][aircraft-title] for
the `master` version of the paper.

~~~
$ gedit paper.md		# Rewrite the title
$ git add paper.md
$ git commit			# "Include aircraft in title"
~~~
{: .language-bash}

### Merging and resolving conflicts

We are now working on two papers: the main one in our `master` branch and the one
which may possibly be collaborative work in our "simulations" branch.
Let's [add another section][simulations-section] to the paper to write about John's simulations.

~~~
$ git checkout simulations	# Switch branch
$ gedit paper.md		# Add 'simulations' section
$ git add paper.md
$ git commit -m "Add simulations" paper.md
~~~
{: .language-bash}

At this point let's visualise the state of our repo,
and we can see the diverged commit history reflecting the recent work
on our two branches:

```
git log --graph --all --oneline --decorate
```
{: .language-bash}

```
* 89d5c6e (simulations) Add simulations
* 05d393a Change title and add coauthor
| * (HEAD, master) bdebbe0 Include aircraft in title
|/  
* 87a65e6 Explain motivation for research
* 6a48241 Cite previous work in introduction
* ed26351 Cite PCASP paper
* 7446b1d Start the introduction
* 4f572d5 Add title and author
```
{: .output}

After some discussions with John we decided that we will publish together,
hence it makes sense to now merge all that was authored together with John
in branch "simulations".
We can do that by *merging* that branch with the `master` branch. Let's try
doing that:

~~~
$ git checkout master		# Switch branch
$ git merge simulations		# Merge simulations into master
~~~
{: .language-bash}
~~~
Auto-merging paper.md
CONFLICT (content): Merge conflict in paper.md
Automatic merge failed; fix conflicts and then commit the result.
~~~
{: .output}

Git cannot complete the merge because there is a conflict - if you recall,
after creating the new branch, we changed the title of the paper on both branches.
We have to resolve the conflict and then complete the merge. We can get
some more detail

~~~
$ git status
~~~
{: .language-bash}
~~~
On branch master
You have unmerged paths.
  (fix conflicts and run "git commit")

Unmerged paths:
  (use "git add <file>..." to mark resolution)

    	both modified:      paper.md
~~~
{: .output}

Let's look inside paper.md:

```
# Title
<<<<<<< HEAD
Aircraft measurements of biomass burning aerosols over West Africa
=======
Simulations of biomass burning aerosols over West Africa
>>>>>>> simulations
```

The mark-up shows us the parts of the file causing the conflict and the
versions they come from. We now need to manually edit the file to resolve the
conflict. This means removing the mark-up and doing one of:

- Keep the current version, which is the one marked-up by HEAD i.e.
"Aircraft measurements of biomass burning aerosols over West Africa"

- Keep the version from the other branch, which is the one marked-up by simulations
i.e. "Simulations of biomass burning aerosols over West Africa"

- Or manually edit the line to something new which might combine some elements
of the two e.g. "Aircraft measurements and simulations of biomass burning aerosols over West Africa"

[We edit the file][resolve-conflict]. Then commit our changes:

~~~
$ gedit paper.md		# Resolve conflict by editing paper.md
$ git add paper.md		# Let Git know we have resolved the conflict
$ git commit
~~~
{: .language-bash}

This is where version control proves itself better than DropBox or GoogleDrive,
this ability to merge text files line-by-line and highlight the conflicts
between them, so no work is ever lost.

We can see the two branches merged if we take another look at the log graph:

```
$ git log --graph --decorate --all --oneline
```
{: .language-bash}

```
*   39cc80d (HEAD, master) Merge branch 'simulations'
|\  
| * 89d5c6e (simulations) Add simulations
| * 05d393a Change title and add coauthor
* | bdebbe0 Include aircraft in title
|/  
* 87a65e6 Explain motivation for research
* 6a48241 Cite previous work in introduction
* ed26351 Cite PCASP paper
* 7446b1d Start the introduction
* 4f572d5 Add title and author
```
{: .output}

### Looking at our history - revisited
We already looked at "going back in time with Git". But now we'll look at it in
more detail to see how moving back relates to branches and we will learn how to
actually undo things. So far we were moving back in time in one branch by
checking out one of the past commits.

But we were then in the "detached HEAD" state.

> ## Add a commit to detached HEAD
>
> - Checkout one of the previous commits from our repository.
> - Make some changes and commit them. What happened?
> - Now try to run `git branch`. What can you see?
>
> > ## Solution
> > ```
> > git checkout HEAD~1  # Check out the commit one before last
> > gedit paper.md     # Make some edits
> > git add paper.md   # Stage the changes
> > git commit           # Commit the changes
> > git branch           # You should see a message like the one below,
> >                      # indicating your commit does not belong to a branch
> > ```
> > {: .language-bash}
> > ```
> > * (detached from 57289fb)
> >   master
> > ```
> > {: .output}
> > You have just made a commit on a detached HEAD --
> > as you can see from the output above, a new temporary branch has been created,
> > which doesn't have a name.
> >
> > See this [detached HEAD animation] of the above process.
> {: .solution}
{: .challenge}

[detached HEAD animation]: https://learngitbranching.js.org/?NODEMO&command=git%20checkout%20HEAD~;git%20commit

> ## Abandon the commit on a detached HEAD
> You decide that you want to abandon that commit.
> How would you get back to the current version of your project?
>
> > ## Solution
> > ```
> > git checkout master
> > ```
> > {: .language-bash}
> > Git will warn you that you are leaving behind changes that would be lost:
> >
> > The output you see will be slightly different to that below,
> > reflecting your previous commit message and commit ID.
> >
> > ```
> > Warning: you are leaving 1 commit behind, not connected to
> > any of your branches:
> >
> > eb7c650 Add empty line for branching exercise
> >
> > If you want to keep them by creating a new branch, this may be a good time
> > to do so with:
> >
> >  git branch new_branch_name eb7c650
> >
> >  Switched to branch 'master'
> >  Your branch is up-to-date with 'master'.
> > ```
> > {: .output}
> > See this [abandon detached HEAD] animation.
> {: .solution}
{: .challenge}

[abandon detached HEAD]: https://learngitbranching.js.org/?NODEMO&command=git%20checkout%20HEAD~;git%20commit;git%20checkout%20master

> ## Save your changes in a new branch
> Preparation:
>
> - You should be on the `master` branch after that last exercise.
> If not, check out master again: `git checkout master`
> - Checkout one of the previous commits from your repository.
> - Make some changes, save the file(s), and make a commit on the detached HEAD as
> you did in the first exercise.
> - Run `git branch` to list your local branches, and see that you are on a temporary branch.
>
> This time we want to keep the commit rather than abandon it.
> - Create a new branch and check it out.
> - Now run `git log` and see that your new commit belongs to this new branch.
> - List your local branches again and see that the temporary branch has gone.
> - Switch back to (i.e. checkout) the `master` branch
>
> > ## Solution
> >
> > ```
> > git checkout HEAD~1		# Checkout the commit before last
> > gedit paper.md		# Modify one of your files
> > git commit -a			# Commit all the modified files
> > git branch			# List local branches
> > ```
> > {: .language-bash}
> >
> > ```
> > * (HEAD detached from f908519)
> >  master
> >  simulations
> > ```
> > {: .output}
> > You are currently on a temporary, unnamed branch, as indicated by the `*`.
> > ```
> > git branch dh-exercise		# Create a new branch
> > git checkout dh-exercise	# Switch to the new branch
> > ```
> > {: .language-bash}
> > ```
> > Switched to a new branch 'dh-exericise'
> > ```
> > {: .output}
> > ```
> > git branch			# View local branches
> > ```
> > {: .language-bash}
> > ```
> > * dh-exericise
> >  master
> >  simulations
> > ```
> > {: .output}
> > The commit you made on the detached HEAD now belongs to a named branch
> > (`dh-exercise` in the example above), rather than a temporary branch.
> > ```
> > git checkout master		# Switch back to the 'master' branch
> > ```
> > {: .language-bash}
> > See this [new branch animation] for the key points in this exercise.
> {: .solution}
{: .challenge}

[new branch animation]: https://learngitbranching.js.org/?NODEMO&command=git%20checkout%20HEAD~;git%20commit;git%20branch%20dh-ex;git%20checkout%20dh-ex;git%20checkout%20master
[change-title]: https://github.com/gcapes/git-course-paper/commit/6d3e6fb24213f796a36fc8bec9db4cc779685482#diff-0403ef06adf405f7b310b4518bd6a3559854f54c61676f676ce9cbfee7172ab6
[aircraft-title]: https://github.com/gcapes/git-course-paper/commit/adcd2a0ceba5d87f8c56873d7984020fa3d82809
[simulations-section]: https://github.com/gcapes/git-course-paper/commit/804bd97911c7b8191e5a372df364e87d27ea8c05
[resolve-conflict]: https://github.com/gcapes/git-course-paper/commit/366bc48f3529ea740f5345b2307e105b942ad48b

### Discarding local changes

Maybe we made our change just to see how something looks, or to
quickly try something out. But we may be unhappy with our changes. If we
haven't yet done a `git add` we can just throw the changes away and return
our file to the most recent version we committed to the repository by using:

```
$ gedit paper.md		# Make some small edits to the file
$ git checkout paper.md		# Discard edits we just made
```
{: .language-bash}

and we can see that our file has reverted to being the most up-to-date one in
the repository:

```
$ git status			# See that we have a clean working directory
$ gedit paper.md		# Inspect file to verify changes have been discarded
```
{: .language-bash}

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

Let's try it on our example. First, let's [modify two files][describe-methodology]: our paper file and
the references file. We will add a methodology section to the paper where we
detail the model used for the simulations, and add a reference for this to the references
file.

```
$ gedit paper.md		# Add methodology section, including a reference to model
$ gedit refs.txt		# Add new reference for the model used
$ git status			# Get a status update on file modifications
```
{: .output}
{: .language-bash}

```
$ On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   refs.txt
	modified:   paper.md

no changes added to commit (use "git add" and/or "git commit -a")
```
{: .output}

Let's then add and commit *paper.md* but **not the references file**.

```
$ git add paper.md		 # Add paper to staging area
$ git commit -m "Describe methodology"
```
{: .language-bash}

Let's have a look at our working directory now:

```
$ git status
```
{: .language-bash}
```
$ On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout --	<file>..." to discard changes in working directory)

	modified:   refs.txt

no changes added to commit (use "git add" and/or "git commit -a")
```
{: .output}

Also, run `git log -2` to see what is the latest commit message and ID.

Now, we want to fix our commit and add the references file.

```
$ git add refs.txt	# Add reference file
$ git commit --amend		# Amend most recent commit
```
{: .language-bash}

This will again bring up the editor and we can amend the commit message if required.

Now when we run `git status` and then `git log` we can see that our Working
Directory is clean and that both files were added.

```
$ git status
$ git log -3
```
{: .language-bash}

---

### `git revert` (undo changes associated with a commit)

`git revert` removes the changes applied in a specified commit. However, rather
than deleting the commit from history, git works out how to undo those changes
introduced by the commit, and appends a new commit with the resulting content.

Let's try it on our example. Modify the paper, [describing the SMPS][SMPS] which is
another instrument used to measure particle sizes, and then make a commit.

```
$ gedit paper.md		# Describe other instrument
$ git add paper.md
$ git commit -m "Describe SMPS"
```
{: .language-bash}

We now realise that what we've just done in our journal article is incorrect
because we are not using the data from that instrument.
Some of the data got corrupted, and due to problems with the logging computer
we are not going to use that data.
So it makes sense to abandon the commit completely.

```
$ git revert HEAD		# Undo changes introduced by most recent commit
```
{: .language-bash}

When we revert, a new commit is created. The *HEAD* pointer and the branch
pointer are in fact moved forward rather than backwards. 	

We can revert any previous commit. That is, we can "abandon" any of the
previous changes. However, depending on the changes we have made since,
we may bump into a *conflict* (which we will cover in more detail later on).
For example:

```
error: could not revert 848361e... Describe SMPS
hint: after resolving the conflicts, mark the corrected paths
hint: with 'git add <paths>' or 'git rm <paths>'
hint: and commit the result with 'git commit'
```
{: .output}

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

Let's try that on our paper, using the same example as before. Now we have two commits
which we want to abandon: the commit outlining the unreliable instrumentation, and
the subsequent revert commit. We can achieve this by resetting to the last
commit we want to keep.

We can do that by running:

```
$ git reset --hard HEAD~2	# Move tip of branch to two commits before HEAD
```
{: .language-bash}
```
HEAD is now at fbdc44b Add methodology section and update references file
```
{: .output}

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

[describe-methodology]: https://github.com/gcapes/git-course-paper/commit/00c685625b66952e33a7d88150232b7d6716a185
[SMPS]: https://github.com/gcapes/git-course-paper/commit/bb77f4eafaf8a5c374a00ae17c79585d30343461
