---
title: "Tracking changes with a local repository"
teaching: 10
exercises: 0
questions:
- "How to get started with Git?"
objectives:
- "Understand how to start tracking files."
- "Be able to commit changes to your repository."
keypoints:
- "`git status` shows the status of a repository"
- "Files can be stored in a project’s `working directory` (which users see),
  the `staging area` (where the next commit is being built up) and the
  `local repository` (where commits are permanently recorded)"
- "`git add` puts files in the staging area"
- "`git commit` saves the staged content as a new commit in the local repository"
- "Always write a log message when committing changes"
---

## Create a new repository with Git

We will be working with a simple example in this tutorial. It will be a paper
that we will first start writing as a single author and then work on it further
with one of our colleagues.

 First, let's create a directory within your home directory:

```
$ cd								# Switch to your home directory.
$ pwd								# Print working directory (output should be /home/<username>)
$ mkdir my-git
$ cd my-git
```
{: .language-bash}

Now, we need to set up this directory up to be a Git repository (or "initiate
the repository"):

~~~
$ git init
~~~
{: .language-bash}
~~~
Initialized empty Git repository in /home/user/my-git/.git/
~~~
{: .output}

The directory "my-git" is now our working directory.

 If we look in this directory, we'll find a `.git` directory:

~~~
$ ls .git
~~~
{: .language-bash}
~~~
branches  config  description  HEAD  hooks  info  objects refs
~~~
{: .output}
The `.git` directory contains Git's configuration files. Be careful not to
accidentally delete this directory!

## Tracking files with a git repository

Now, we'll create a file. Let's say we're going to write a journal paper/article, so
we will start by adding the author names and a title, then save the file.

~~~
$ atom article.md
# Add author names and paper title
~~~
{: .language-bash}

> ## Accessing files from the command line

> In this lesson we create and modify text files using a command line interface
> (e.g. terminal, Git Bash etc), mainly for convenience.
> You can use any text editor and you can see the files from the file browser on any operating system (e.g. Windows explorer), and by other programs.
{: .callout}

`git status` allows us to find out about the current status
of files in the repository. So we can run,

~~~
$ git status
~~~
{: .language-bash}
~~~
On branch master

Initial commit

Untracked files:
(use "git add <file>..." to include in what will be committed)

article.md

nothing added to commit but untracked files present (use "git add" to track)
~~~
{: .output}

Information about what Git knows about the directory is displayed. We are on
the `master` branch, which is the default branch in a Git repository
(one way to think of branches is like parallel versions of the project.
Don't worry -- we'll talk more about branches later).

For now, the important bit of information is that our file is listed as
**Untracked** which means it is in our working directory but Git is not
tracking it - that is, any changes made to this file will not be recorded by
Git.

## Add files to a Git repository

To tell Git about the file, we will use the `git add` command:

~~~
$ git add article.md
$ git status
~~~
{: .language-bash}
~~~
On branch master

Initial commit

Changes to be committed:
(use "git rm --cached <file>..." to unstage)

      	new file:   article.md
~~~
{: .output}

Now our file is listed underneath where it says **Changes to be committed**.

`git add` is used for two purposes. Firstly, to tell Git that a given file
should be tracked. Secondly, to put the file into the Git **staging area**
which is also known as the *index* or the *cache*.

The staging area can be viewed as a "loading dock", a place to hold files we have
added, or changed, until we are ready to tell Git to record those changes in the
repository.

![The staging area](../fig/git-staging-area.svg)

## Commit changes

In order to tell Git to record our change, our new file, into the repository,
we need to  **commit** it:

~~~
$ git commit
# Type a commit message: "Add title and authors"
# Save the commit message and close your text editor (vim, atom, notepad etc.)
~~~
{: .language-bash}

Our default editor will now pop up. Why? Well, Git can automatically figure out
that directories and files are committed, and by whom (thanks to the information
we provided before) and even, what changes were made, but it cannot figure out
why. So we need to provide this in a commit message.

If we save our commit message **and exit the editor**, Git will now commit our file.

~~~
[master (root-commit) 537997c]
1 file changed, 2 insertions(+) Add title and authors
create mode 100644 article.md
~~~
{: .output}

This output shows the number of files changed and the number of lines inserted
or deleted across all those files. Here, we have changed (by adding) 1 file and
inserted 2 lines.

Now, if we look at its status,

~~~
$ git status
~~~
{: .language-bash}
~~~
On branch master
nothing to commit, working directory clean
~~~
{: .output}

our file is now in the repository.
The output from the `git status` command means that we have a clean directory
i.e. no tracked but modified files.

Now we will work a bit further on our *article.md* file by writing the introduction
section.

```
$ atom article.md
# Write introduction section
```
{: .language-bash}
If we now run,

~~~
$ git status
~~~
{: .language-bash}

we see changes not staged for commit section and our file is marked as
modified:

~~~
On branch master
Changes not staged for commit:
(use "git add <file>..." to update what will be committed)
(use "git checkout -- <file>..." to discard changes in working directory)

     modified:   article.md

no changes added to commit (use "git add" and/or "git commit -a")
~~~
{: .output}

This means that a file Git knows about has been modified by us but
has not yet been committed. So we can add it to the staging area and then
commit the changes:

~~~
$ git add article.md
$ git commit							# "Write introduction"
~~~
{: .language-bash}
Note that in this case we used `git add` to put article.md to the staging
area. Git already knows this file should be tracked but doesn't know if we want
to commit the changes we made to the file  in the repository and hence we have
to add the file to the staging area.

It can sometimes be quicker to provide our commit messages at the command-line
by doing `git commit -m "Write introduction section"`.

Let's add a directory *common* and a file *references.txt* for references we may
want to reuse:

~~~
$ mkdir common
$ atom common/references.txt					# Add a reference
~~~
{: .language-bash}

We will also add a citation in our introduction section (in article.md).

~~~
$ atom article.md 						# Use reference in introduction
~~~
{: .language-bash}

Now we need to record our work in the repository so we need to make a commit.
First we tell Git to track the references.
We can actually tell Git to track everything in the given sub-directory:

~~~
$ git add common						# Track everything currently in the 'common' directory
$ git status							# Verify that common/references.txt is now tracked
~~~
{: .language-bash}

All files that are in *common* are now tracked.  We would also have to add
article.md in the staging area. But there is a shortcut. We can use
`commit -a`. This option means "commit all files that are tracked and
that have been modified".

~~~
$ git commit -am "Reference Huang and add references file" 	# Add and commit all tracked files
~~~
{: .language-bash}
and Git will add, then commit, both the directory and the file.

In order to add all tracked files to the staging area, use `git commit -a`
(which may be very useful if you edit e.g. 10 files and now you want to commit all of them).

![The Git commit workflow](../fig/git-committing.svg)
[an additional resource](https://zenodo.org/record/3369466)
