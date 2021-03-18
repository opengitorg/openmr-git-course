---
title: "Git with others"
teaching: 30
exercises: 0
questions:
- "How do I update my local repository with changes from the remote?"
- "How can I collaborate using Git?"
- "What is a remote repository"
- "How can I use GitHub to work from multiple locations?"
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
GitHub **isn't** the only remote repostitories provider. It is however very popular,
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
* Enter Repository name: "paper"
* For the purpose of this exercise we'll create a public repository
* Make sure that **Initialize this repository with a README** is **unselected**
* Click **Create Repository**

You'll get a page with new information about your repository. We already have
our local repository and we will be *pushing* it to GitHub, so this is the
option we will use:

```
$ git remote add origin https://github.com/<USERNAME>/paper.git
$ git push -u origin master
```
{: .language-bash}

The first line sets up an alias `origin`, to correspond to the URL of our
new repository on GitHub.


### Push locally tracked files to a remote repository

Now copy and paste the second line,

```
$ git push -u origin master
```
{: .language-bash}
```
Counting objects: 32, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (28/28), done.
Writing objects: 100% (32/32), 3.29 KiB | 0 bytes/s, done.
Total 32 (delta 7), reused 0 (delta 0)
To https://github.com/gcapes/paper.git
 * [new branch]      master -> master
Branch master set up to track remote branch master from origin.
```
{: .output}

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
{: .language-bash}

The branch should now be created in our GitHub repository.

To list all branches (local and remote):

```
$ git branch -a
```
{: .language-bash}

> ## Deleting branches (for information only)
> **Don't do this now.** This is just for information.
> To delete branches, use the following syntax:
>
> ```
> $ git branch -d <branch_name>			# For local branches
> $ git push origin --delete <branch_name>	# For remote branches
> ```
> {: .language-bash}
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
{: .language-bash}

Then to clone the repo into a new directory called `laptop_paper`

```
$ git clone https://github.com/<USERNAME>/paper.git laptop_paper
```
{: .language-bash}
```
Cloning into 'laptop_paper'...
remote: Counting objects: 32, done.
remote: Compressing objects: 100% (21/21), done.
remote: Total 32 (delta 7), reused 32 (delta 7), pack-reused 0
Unpacking objects: 100% (32/32), done.
Checking connectivity... done.
```
{: .output}

Cloning creates an exact copy of the repository. By deafult it creates
a directory with the same name as the name of the repository.
However, we already have a `paper` dircectory,
so have specified that we want to clone into a new directory `laptop_paper`.

Now, if we `cd` into *laptop_paper* we can see that we have our repository,

```
$ cd laptop_paper
$ git log
```
{: .language-bash}
and we can see our Git configuration files too:

```
$ ls -A
```
{: .language-bash}

In order to see the other branches locally, we can check them out as before:

```
$ git branch -r					# Show remote branches
$ git checkout simulations			# Check out the simulations branch
```
{: .language-bash}

### Push changes to a remote repository

We can use our cloned repository just as if it was a local repository so let's
[add a results section][add-results] and commit the changes.

```
$ git checkout master				# We'll continue working on the master branch
$ gedit paper.md				# Add results section
$ git add paper.md				# Stage changes
$ git commit
```
{: .language-bash}

Having done that, how do we send our changes back to the remote repository? We
can do this by *pushing* our changes,

```
$ git push origin master
```
{: .language-bash}

If we now check our GitHub page we should be able to see our new changes under
the *Commit* tab.

To see all remote repositories (we can have multiple!) type:

```
$ git remote -v
```
{: .language-bash}

[add-results]: https://github.com/gcapes/git-course-paper/commit/0c4573e5ea15d6f5dc877e8db8c0696e7675d5ed

### Pulling changes from a remote repository

Having a remote repository means we can share it and collaborate with
others (or even just continue to work alone but from multiple locations).
We've seen how to clone the whole repo, so next we'll look at how to update
our local repo with just the latest changes on the remote.

We were in the `laptop_paper` directory at the end of the last episode,
having pushed one commit to the remote.
Let's now change directory to the other repository `paper`,
and `git pull` the commit from the remote.

```
$ cd ../paper
$ git pull origin master
```
{: .language-bash}

We can now view the contents of `paper.md` and check the log to confirm we have
the latest commit from the remote:
```
$ git log -2
```
{: .language-bash}

Still in the `paper` directory, let's [add a figures section][add-figures] to `paper.md`,
commit the file and push these changes to GitHub:

```
$ gedit paper.md		# Add figures section
$ git add paper.md
$ git commit -m "Add figures"
$ git push
```
{: .language-bash}

Now let's change directory to our other repository and `fetch` the commits from our
remote repository,

```
$ cd ../laptop_paper		# Switch to the other directory
$ git fetch
```
{: .language-bash}

`git fetch` doesn't change any of the local branches,
it just gets information about what commits are on the remote branches.

We can visualise the remote branches in the same way as we did for local branches,
so let's draw a network graph before going any further:

```
git log --graph --all --decorate --oneline
```
{: .language-bash}

```
* 7c239c3 (origin/master, origin/HEAD) Add figures
* 0cc2a2d (HEAD -> master) Discuss results
* 3011ee0 Describe methodology
*   6420699 Merge branch 'simulations'
|\
| * 7138785 (origin/simulations) Add simulations
| * e695fa8 Change title and add coauthor
* | e950911 Include aircraft in title
|/
* 0b28b0a Explain motivation for research
* 7cacba8 Cite previous work in introduction
* 56781f4 Cite PCASP paper
* 5033467 Start the introduction
* e08262e Add title and author
```
{: .output}

As expected, we see that the `origin/master` branch is ahead of our local `master` branch
by one commit  --- note that the history hasn't diverged,
rather our local branch is missing the most recent commit on `origin/master`.

We can now see what the differences are by doing,

```
$ git diff origin/master
```
{: .language-bash}

which compares our `master` branch with the `origin/master` branch
which is the name of the `master` branch in `origin` which is the alias for our
cloned repository, the one on GitHub.

We can then `merge` these changes into our current repository,
but given the history hasn't diverged, we don't get a merge commit ---
instead we get a *fast-forward* merge.

```
$ git merge origin/master
```
{: .language-bash}

```
Updating 0cc2a2d..7c239c3
Fast-forward
 paper.md | 4 ++++
 1 file changed, 4 insertions(+)
```
{: .output}

If we look at the network graph again, all that has changed
is that `master` now points to the same commit as `origin/master`.

```
git log --graph --all --decorate --oneline -4
```
{: .language-bash}

```
* 7c239c3 (HEAD -> master, origin/master, origin/HEAD) Add figures
* 0cc2a2d Discuss results
* 3011ee0 Describe methodology
*   6420699 Merge branch 'simulations'
```
{: .output}

We can inspect the file to confirm that we have our changes.

```
$ cat paper.md
```
{: .language-bash}

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
$ gedit paper.md		# Write Conclusions
$ git add paper.md
$ git commit -m "Write Conclusions" paper.md
$ git push origin master
$ cd ../paper			# Switch back to the paper directory
$ git pull origin master	# Get changes from remote repository
```
{: .language-bash}

This is the same scenario as before, so we get another fast-forward merge.

We can check that we have our changes:

```
$ cat paper.md
$ git log
```
{: .language-bash}

### Conflicts and how to resolve them

Let's continue to pretend that our two local repositories are hosted
on two different machines. You should still be in the original *paper* folder.
[Add an affiliation for each author][author-affiliations].
Then push these changes to our remote repository:

```
$ gedit paper.md		# Add author affiliations
$ git add paper.md
$ git commit -m "Add author affiliations"
$ git push origin master
```
{: .language-bash}

Now let us suppose, at a later date, we use our other repository (on the laptop)
and we want to [change the order of the authors][change-first-author].

The remote branch `origin/master` is now ahead of our local `master` branch on the laptop,
because we haven't yet updated our local branch using `git pull`.

```
$ cd ../laptop_paper		# Switch directory to other copy of our repository
$ gedit paper.md		# Change order of the authors
$ git add paper.md
$ git commit -m "Change the first author" paper.md
$ git push origin master
```
{: .language-bash}
```
To https://github.com/<USERNAME>/paper.git
 ! [rejected]        master -> master (fetch first)
error: failed to push some refs to 'https://github.com/<USERNAME>/paper.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```
{: .output}

Our push fails, as we've not yet pulled down our changes from our remote
repository. Before pushing we should always pull, so let's do that...

```
$ git pull origin master
```
{: .language-bash}

and we get:

```
Auto-merging paper.md
CONFLICT (content): Merge conflict in paper.md
Automatic merge failed; fix conflicts and then commit the result.
```
{: .output}

As we saw earlier, with the fetch and merge, `git pull` pulls down changes from the
repository and tries to merge them. It does this on a file-by-file basis,
merging files line by line. We get a **conflict** if a file has changes that
affect the same lines and those changes can't be seamlessly merged. We had this
situation before in the *branching* episode when we merged a *feature* branch into *master*.
If we look at the status,

```
$ git status
```
{: .language-bash}

we can see that our file is listed as *Unmerged* and if we look at
*paper.md*, we see something like:

```
<<<<<<< HEAD
Author
G Capes, J Smith
=======
author
J Smith, G Capes
>>>>>>> 1b55fe7f23a6411f99bf573bfb287937ecb647fc
```

The mark-up shows us the parts of the file causing the conflict and the
versions they come from. We now need to manually edit the file to *resolve* the
conflict. Just like we did when we had to deal with the conflict when we were
merging the branches.

[We edit the file][merge-conflict]. Then commit our changes. Now, if we *push* ...

```
$ gedit paper.md		# Edit file to resolve merge conflict
$ git add paper.md		# Stage the file
$ git commit			# Commit to mark the conflict as resolved
$ git push origin master
```
{: .language-bash}

... all goes well. If we now go to GitHub and click on the "Overview" tab we can
see where our repository diverged and came together again.

This is where version control proves itself better than DropBox or GoogleDrive,
this ability to merge text files line-by-line and highlight the conflicts
between them, so no work is ever lost.

We'll finish by pulling these changes into other copy of the repo,
so both copies are up to date:

```
$ cd ../paper			# Switch to 'paper' directory
$ git pull origin master	# Merge remote branch into local
```
{: .language-bash}

> ## Collaborating on a remote repository
>
> In this exercise you should work with a partner or a group of three.
> One of you should give access to your remote repository on GitHub to
> the others (by selecting `Settings -> Manage access -> Invite a collaborator`).
> The invited person should then check their email to accept the invitation.
>
> Now those of you who are added as collaborators should clone the repository of
> the first person on your machines. (make sure that you **don't clone into
> a directory that is already a repository**!)
>
> Each of you should now make some changes to the files in the repository
> e.g. fix a typo, add a file containing supplementary material.
> Commit the changes and then push them back to the remote repository.
> Remember to pull changes before you push.
{: .challenge}

> ## Creating branches and sharing them in the remote repository
>
> Working with the same remote repository, each of you should create a new branch
> locally and push it back to the remote repo.
>
> Each person should use a different name for their local branch.
> The following commands assume your new branch is called `my_branch`,
> and your partner's branch is called `their_branch` ---
> you should substitute the name of your new branch and your partner's new branch.
>
> ```
> $ git checkout -b my_branch		# Create and check out a new branch.
>				 	# Substitute your local branch name for 'my_branch'.
> ```
> {: .language-bash}
>
> Now create/edit a file (e.g. fix a typo, add supplementary material etc), and then commit your changes.
>
> ```
> $ git push origin my_branch		# Push your new branch to remote repo.
> ```
> {: .language-bash}
>
> The other person should check out local copies of the branches created by others
> (so eventually everybody should have the same number of branches as the remote
> repository).
>
> To fetch new branches from the remote repository (into your local `.git` database):
>
> ```
> $ git fetch origin
> ```
> {: .language-bash}
> ```
> Counting objects: 3, done.  remote:
> Compressing objects: 100% (3/3), done.
> remote: Total 3 (delta 0), reused 2 (delta 0) Unpacking objects: 100% (3/3), done.
> From  https://github.com/gcapes/paper
> 9e1705a..640210a master -> origin/master
> * [new branch] their_branch -> origin/their_branch
> ```
> {: .output}
>
> Your local repository should now contain all the branches from the remote repository,
> but the `fetch` command doesn't actually update your local branches.
>
> The next step is to check out a new branch locally to track the new remote branch.
>
> ```
> $ git checkout their_branch
> ```
> {: .language-bash}
> ```
> Branch their_branch set up to track remote branch their_branch from origin.
> Switched to a new branch 'their_branch'
> ```
> {: .output}
{: .challenge}

> ## Undoing changes using revert
>
> Once you have the branches which others created, try to undo one of the commits.
>
> Each one of you should try to [revert]({{ page.root }}/07-undoing) a commit in a different
> branch to your partner(s).
>
> Push the branch back to the remote repository. The others should pull that
> branch to get the changes you made.
>
> What is the end result? What happens when you pull the branch that your
> colleagues changed using `git revert`?
>
> > ## Solution
> > The revert shows up in everyone's copy.
> > You should always use `revert` to undo changes which have been shared with others.
> {: .solution}
{: .challenge}

[add-figures]: https://github.com/gcapes/git-course-paper/commit/8bfe74f715173b3f000134d56678e39e64d4bfbb
[write-conclusions]: https://github.com/gcapes/git-course-paper/commit/7f9d94849c7de7c8dd915938b13fecae037f675a
[author-affiliations]: https://github.com/gcapes/git-course-paper/commit/047e3f80e6530a3559efea82fe56229baf57f0b4
[change-first-author]: https://github.com/gcapes/git-course-paper/commit/3e431fe1e6b9111a614e156994209c78169bb7f5
[merge-conflict]: https://github.com/gcapes/git-course-paper/commit/0bba3e4aff27b7f8fa92736ab6e07429fd1fc49d
