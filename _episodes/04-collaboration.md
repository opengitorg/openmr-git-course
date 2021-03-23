---
title: "Git with others"
teaching: 30
exercises: 0
questions:
- "How do I update my local repository with changes from the remote?"
- "How can I collaborate using Git?"
- "What is a remote repository"
- "How can I use GitHub to work from multiple locations?"
- "What is rebasing?"
objectives:
- "Understand how to set up remote repository"
- "Understand how to push local changes to a remote repository"
- "Understand how to clone a remote repository"
- "Understand how to pull changes from remote repository"
- "Understand how to resolve merge conflicts"
keypoints:
- "Git is the version control system: GitHub is a remote repositories provider."
- "`git clone` to make a local copy of a remote repository"
- "`git push` to send local changes to remote repository"
- "`git pull` to integrate remote changes into local copy of repository"
---

[GitHub](http://GitHub.com) is a company which provides remote repositories for
Git and a range of functionalities supporting their use. GitHub allows users to
set up  their private and public source code Git repositories. It provides
tools for browsing, collaborating on and documenting code. GitHub, like other
services such as [Launchpad](https://launchpad.net),
[Bitbucket](https://bitbucket.org), [GoogleCode](http://code.google.com), and
[SourceForge](http://sourceforge.net) supports a wealth of resources to support
projects including:

* Time histories changes to repositories
* Commit-triggered e-mails
* Browsing code from within a web browser, with syntax highlighting
* Software release management
* Issue (ticket) and bug tracking
* Download
* Varying permissions for various groups of users
* Other service hooks e.g. to Twitter.

**Note**  GitHub's free repositories have public licences **by default**. If
you don't want to share (in the most liberal sense) your stuff with the world
and you want to use GitHub, you will need to pay for the
private GitHub repositories (GitHub offers up to 5 free private repositories,
if you are an academic - but do check this information as T&C may change).

### GitHub for research
GitHub **isn't** the only remote repostitory provider. It is however very popular,
in particular within the Open Source communities. The reason why we teach GitHub
in this tutorial is mainly due to popular demand.

Also, GitHub has started working on [functionality which is particularily useful
for researchers](https://github.com/blog/1840-improving-github-for-sciences)
such as making code citable.

---

### Get an account

Let's get back to our tutorial. We will first need a GitHub account.

[Sign up](https://GitHub.com) or if you already have an account [sign
in](https://GitHub.com).

### Create a new repository

Now, we can create a repository on GitHub,

* Log in to [GitHub](https://GitHub.com/)
* Click on the **Create** icon on the top right
* Enter Repository name: "article"
* For the purpose of this exercise we'll create a public repository
* Make sure that **Initialize this repository with a README** is **unselected**
* Click **Create Repository**

You'll get a page with new information about your repository. We already have
our local repository and we will be *pushing* it to GitHub, so this is the
option we will use:

```
$ git remote add origin https://github.com/<USERNAME>/article.git
$ git push -u origin master
```

The first line sets up an alias `origin`, to correspond to the URL of our
new repository on GitHub.


### Push locally tracked files to a remote repository

Now copy and paste the second line,

```
$ git push -u origin master

Enumerating objects: 25, done.
Counting objects: 100% (25/25), done.
Delta compression using up to 8 threads
Compressing objects: 100% (23/23), done.
Writing objects: 100% (25/25), 2.56 KiB | 875.00 KiB/s, done.
Total 25 (delta 8), reused 0 (delta 0)
remote: Resolving deltas: 100% (8/8), done.
To https://github.com/i-am-mel-dev/git-course-article.git
 * [new branch]      master -> master
Branch 'master' set up to track remote branch 'master' from 'origin'.

```

This **pushes** our `master` branch to the remote repository, named via the alias
`origin` and creates a new `master` branch in the remote repository.

Now, on GitHub, we should see our code and if we click the `Commits` tab we should see
our complete history of commits.

Our local repository is now available on GitHub. So, anywhere we can access
GitHub, we can access our repository.


### Push other local branches to a remote repository

Let's push each of our local branches into our remote repository:

```
$ git push origin branch_name
```

The branch should now be created in our GitHub repository.

To list all branches (local and remote):

```
$ git branch -a
```

> ## Deleting branches (for information only)
> **Don't do this now.** This is just for information.
> To delete branches, use the following syntax:
>
> ```
> $ git branch -d <branch_name>			# For local branches
> $ git push origin --delete <branch_name>	# For remote branches
> ```
>
{: .callout}

### Cloning a remote repository

Now that we have a copy of the repo on GitHub,
we can download or `git clone` a fresh copy to work on from another computer.

So let's pretend that the repo we've been working on so far is on a PC in the office,
and you want to do some work on your laptop at home in the evening.

Before we clone the repo, we'll navigate up one directory so that we're not already in a git repo.
```
cd ..
```

Then to clone the repo into a new directory called `laptop_article`

```
$ git clone https://github.com/<USERNAME>/article.git laptop_article

Cloning into 'laptop_article'...
remote: Enumerating objects: 25, done.
remote: Counting objects: 100% (25/25), done.
remote: Compressing objects: 100% (15/15), done.
remote: Total 25 (delta 8), reused 25 (delta 8), pack-reused 0
Unpacking objects: 100% (25/25), 2.54 KiB | 520.00 KiB/s, done.
```

Cloning creates an exact copy of the repository. By deafult it creates
a directory with the same name as the name of the repository.
However, we already have a `article` dircectory,
so have specified that we want to clone into a new directory `laptop_article`.

Now, if we `cd` into *laptop_article* we can see that we have our repository,

```
$ cd laptop_article
$ git log
```
and we can see our Git configuration files too:

```
$ ls -A
```

In order to see the other branches locally, we can check them out as before:

```
$ git branch -r					# Show remote branches
$ git checkout simulations			# Check out the simulations branch
```

### Push changes to a remote repository

We can use our cloned repository just as if it was a local repository so let's
[add a results section][add-results] and commit the changes.

```
$ git checkout master				# We'll continue working on the master branch
$ atom article.md				# Add results section
$ git add article.md				# Stage changes
$ git commit
```

Having done that, how do we send our changes back to the remote repository? We
can do this by *pushing* our changes,

```
$ git push origin master
```

If we now check our GitHub page we should be able to see our new changes under
the *Commit* tab.

To see all remote repositories (we can have multiple!) type:

```
$ git remote -v
```



### Pulling changes from a remote repository

Having a remote repository means we can share it and collaborate with
others (or even just continue to work alone but from multiple locations).
We've seen how to clone the whole repo, so next we'll look at how to update
our local repo with just the latest changes on the remote.

We were in the `laptop_article` directory at the end of the last episode,
having pushed one commit to the remote.
Let's now change directory to the other repository `article`,
and `git pull` the commit from the remote.

```
$ cd ../article
$ git pull origin master
```

We can now view the contents of `article.md` and check the log to confirm we have
the latest commit from the remote:
```
$ git log -2
```

Still in the `article` directory, let's [add a figures section][add-figures] to `article.md`,
commit the file and push these changes to GitHub:

```
$ atom article.md		# Add figures section
$ git add article.md
$ git commit -m "Add figures"
$ git push
```

Now let's change directory to our other repository and `fetch` the commits from our
remote repository,

```
$ cd ../laptop_article		# Switch to the other directory
$ git fetch
```

`git fetch` doesn't change any of the local branches,
it just gets information about what commits are on the remote branches.

We can visualize the remote branches in the same way as we did for local branches,
so let's draw a network graph before going any further:

```
git log --graph --all --decorate --oneline

* 68a3dee (origin/master, origin/HEAD) add figures
* b37a12f (HEAD -> master) results added
*   1c90e39 Merge branch 'methodology'
|\  
| * cc8efe9 Add methodology
| * 9a7dc94 Add methodology
* | 69fefc9 Include git in title
|/  
* 4714690 Explain motivation for research
* 3eac70f cite previous work in intriduction
* 635f24b write introduction section
* 537997c add title and authors

```

As expected, we see that the `origin/master` branch is ahead of our local `master` branch
by one commit  --- note that the history hasn't diverged,
rather our local branch is missing the most recent commit on `origin/master`.

We can now see what the differences are by doing,

```
$ git diff origin/master
```

which compares our `master` branch with the `origin/master` branch
which is the name of the `master` branch in `origin` which is the alias for our
cloned repository, the one on GitHub.

We can then `merge` these changes into our current repository,
but given the history hasn't diverged, we don't get a merge commit ---
instead we get a *fast-forward* merge.

```
$ git merge origin/master

Updating b37a12f..68a3dee
Fast-forward
 article.md | 4 ++++
 1 file changed, 4 insertions(+)
```

If we look at the network graph again, all that has changed
is that `master` now points to the same commit as `origin/master`.

```
git log --graph --all --decorate --oneline -4

* 68a3dee (HEAD -> master, origin/master, origin/HEAD) add figures
* b37a12f results added
*   1c90e39 Merge branch 'methodology'
|\  
| * cc8efe9 Add methodology
```

We can inspect the file to confirm that we have our changes.

```
$ cat article.md
```

So we have now used two slightly different methods to get the latest changes
from the remote repo.
You may already have guessed that `git pull` is a shorthand for `git fetch` followed by `git merge`.

> ## `Fetch` vs `pull`
> If `git pull` is a shortcut for `git fetch` followed by `git merge` then, why would
> you ever want to do these steps separately?
>
> Well, depending on what the commits on the remote branch contain,
> you might want to abandon your local commits before merging
> (e.g. your local commits duplicate the changes on the remote),
> rebase your local branch to avoid a merge commit, or something else.
>
> Fetching first lets you inspect the changes
> before deciding what you want to do with them.
{: .callout}

Let's [write the conclusions][write-conclusions]:

```
$ atom article.md		# Write Conclusions
$ git add article.md
$ git commit -m "Write Conclusions" article.md
$ git push origin master
$ cd ../article			# Switch back to the article directory
$ git pull origin master	# Get changes from remote repository
```

This is the same scenario as before, so we get another fast-forward merge.

We can check that we have our changes:

```
$ cat article.md
$ git log
```

### Conflicts and how to resolve them

Let's continue to pretend that our two local repositories are hosted
on two different machines. You should still be in the original *article* folder.
[Add an affiliation for each author][author-affiliations].
Then push these changes to our remote repository:

```
$ atom article.md		# Add author affiliations
$ git add article.md
$ git commit -m "Add author affiliations"
$ git push origin master
```

Now let us suppose, at a later date, we use our other repository (on the laptop)
and we want to [change the order of the authors][change-first-author].

The remote branch `origin/master` is now ahead of our local `master` branch on the laptop,
because we haven't yet updated our local branch using `git pull`.

```
$ cd ../laptop_article		# Switch directory to other copy of our repository
$ atom article.md		# Change order of the authors
$ git add article.md
$ git commit -m "Change the first author" article.md
$ git push origin master

To https://github.com/<USERNAME>/article.git
 ! [rejected]        master -> master (fetch first)
error: failed to push some refs to 'https://github.com/<USERNAME>/article.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

Our push fails, as we've not yet pulled down our changes from our remote
repository. Before pushing we should always pull, so let's do that...

```
$ git pull origin master
```

and we get:

```
Auto-merging article.md
CONFLICT (content): Merge conflict in article.md
Automatic merge failed; fix conflicts and then commit the result.
```

As we saw earlier, with the fetch and merge, `git pull` pulls down changes from the
repository and tries to merge them. It does this on a file-by-file basis,
merging files line by line. We get a **conflict** if a file has changes that
affect the same lines and those changes can't be seamlessly merged. We had this
situation before in the *branching* episode when we merged a *feature* branch into *master*.
If we look at the status,

```
$ git status
```

we can see that our file is listed as *Unmerged* and if we look at
*article.md*, we see something like:

```
<<<<<<< HEAD
Author
Smith, M John
=======
author
John, Smith
>>>>>>> 7a1c84f54933a1719031f6d908d33c0fee7293e9
```

The mark-up shows us the parts of the file causing the conflict and the
versions they come from. We now need to manually edit the file to *resolve* the
conflict. Just like we did when we had to deal with the conflict when we were
merging the branches.

[We edit the file][merge-conflict]. Then commit our changes. Now, if we *push* ...

```
$ atom article.md		# Edit file to resolve merge conflict
$ git add article.md		# Stage the file
$ git commit			# Commit to mark the conflict as resolved
$ git push origin master
```

... all goes well. If we now go to GitHub and click on the "Overview" tab we can
see where our repository diverged and came together again.

This is where version control proves itself better than DropBox or GoogleDrive,
this ability to merge text files line-by-line and highlight the conflicts
between them, so no work is ever lost.

We'll finish by pulling these changes into other copy of the repo,
so both copies are up to date:

```
$ cd ../article			# Switch to 'article' directory
$ git pull origin master	# Merge remote branch into local
```

We now know how to solve conflicts between branches!
## What is rebasing

We were in the *article* directory at the end of the last episode,
which is where this episode continues.

Let's review the recent history of our project,
noting particularly the commit message which results when `origin/master` and `master` diverge,
and `origin/master` is merged back into `master`.

```
$ git log --graph --all --oneline --decorate -6

*   365748e (HEAD -> master, origin/master, origin/HEAD) Merge branch 'master' of github.com:i-am-mel-dev/github course article
* af1042b (HEAD -> master, origin/master) add author affiliations
* a83a765 write conclusions
* 68a3dee add figures
* b37a12f results added
*   1c90e39 Merge branch 'methodology'

```

Normally a merge commit indicates that a feature branch has been completed,
a bug has been fixed, or marks a release version of our project.
Our most recent merge commit doesn't mark any real milestone in the history of the project ---
all it tells us is that we didn't pull before we tried to push.
Merge commits like this don't add any real value[^opinion],
and can quickly clutter the history of a project.

If only there were a way to avoid them,
e.g. by starting with the tip of the remote branch
and reapplying our local commits from this new starting point.
You could also describe this as moving the local commits onto a new *base* commit
i.e. **rebasing**.


### What is it?
Rebasing is the process of moving a whole branch to a new base commit.
Git takes your changes, and "replays" them onto the new base commit.
This creates a brand new commit for each commit in the original branch.
As such, **your history is rewritten when you rebase**.

It's like saying "add my changes to what has already been done".

![Visual illustration of rebasing - image taken from [https://www.atlassian.com/git/tutorials/rewriting-history/git-rebase](https://www.atlassian.com/git/tutorials/rewriting-history/git-rebase)](../fig/git-rebase.svg)

### How's that different to merging?
Imagine you create a new feature branch to work in, and meanwhile there have been
commits added to the `master` branch, as shown below.

![](../fig/forked-history.svg)

You've finished working on the feature, and
you want to incorporate your changes from the `feature` branch into the `master` branch.
You could merge directly or rebase then merge. We have already encountered merging, and it looks like this:

![](../fig/merge-without-rebase.svg)

The main reason you might want to rebase is to maintain a linear project history.
In the example above, if you merge directly (recall that there are new commits on
both the `master` branch and `feature` branch), you have a 3-way merge
(common ancestor, HEAD and MERGE_HEAD) and a merge commit results.
Note that you get a merge commit whether or not there are any merge conflicts.

If you rebase, your commits from the `feature` branch are replayed onto `master`,
creating brand new commits in the process.
If there are any merge conflicts, you are prompted to resolve these.

![](../fig/rebase-master.svg)

After rebasing, you can then perform a fast-forward merge into `master` i.e. without
an extra merge commit at the end, so you have a nice clean linear history.

![](../fig/rebase-then-merge.svg)

### Why would I consider rebasing?
`Rebase` and `merge` solve the same problem: integrating commits from one branch into another.
Which method you use is largely personal preference.

Some reasons to consider rebasing:
- To give a linear project history, which is easier to follow
	- This makes using `git log`, and `git bisect` easier
- To integrate upstream changes into your local repository, without creating any merge commits
- To keep a feature branch up to date with master, without polluting your feature branch with extraneous merge commits
- Makes pull requests easier to manage (because you've already resolved any merge conflicts while rebasing)
- To tidy up a feature branch before merging into master.
