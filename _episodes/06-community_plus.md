---
title: "Git& Open Science Management"
teaching: 10
exercises: 0
questions:
- "Tips and Tricks"
- "How can I collaborate using Git?"
- "Licensing: what, why, how?"
- "Open Science and VCS"
objectives:
- "Understand how to pull changes from remote repository"
- "Understand how to resolve merge conflicts"
keypoints:
- "`git pull` to integrate remote changes into local copy of repository"
---
### Getting help

#### `man` page

Like many Unix/Linux commands, `git` has a `man` page,

```
$ man git
```

You can scroll the manual page up and down using the up and down arrows.

You can search for keywords by typing `/` followed by the search term e.g. if
interested in help, type `/help` and then hit enter.

To exit the manual page, type `q`.


#### Command-line help

Type,

```
$ git --help
```
{: .language-bash}

and Git gives a list of commands it is able to help with, as well as their
descriptions.

You can get more help on a specific command, by providing the command name e.g.

```
$ git init --help
$ git commit --help
```
{: .language-bash}

#### Google
Search for your problem online. Someone has probably already asked (and answered) your question on stackoverflow.com.

---

### Add a repository description

You can edit the file `.git/description` and give your repository a name e.g.
"My first repository".

---

### Ignore scratch, temporary and binary files

You can create a `.gitignore` file which lists the patterns of files you want
Git to ignore. It's common practice to not add to a repository any file you can
automatically create in some way e.g. C object files (`.o`), Java class
(`.class`) files or temporary files e.g. XEmacs scratch files (`~`). Adding
these to `.gitignore` means Git won't complain about them being untracked.

Create or edit `gitignore`,

```
$ gedit .gitignore
```
{: .language-bash}

Then add patterns for the files you want to ignore, where `*` is a wildcard,

    *~ *.o *.so *.dll *.exe *.class *.jar

Then, add `.gitignore` to your repository,

```
$ git add .gitignore $ git commit -m "Added rules to ignore vim scratch
files and binary files"
```
{: .language-bash}

---

### `git add --patch`
This is a way to stage only parts of a file. If you have done lots of work
without committing, it may be useful to commit your changes as a series of
small commits. This command allows you to choose which changes go into which
commit so you can group the changes logically.
- [Guide to `git add --patch`](
http://nuclearsquid.com/writings/git-add/)
- Manually [editing hunks] is the  most difficult aspect.

---

### `git commit --author`
You can commit changes made by someone else, by using the `--author`
flag. Consider how this may enable you to collaborate with your colleagues.
The syntax is:

`git add --author="FirstName Surname <Firstname.Surname@example.com>"`

---

### Colours in Git

On many computers, the terminal output is automatically coloured which makes
reading the output easier.
If your output is not coloured (e.g. in the Sackville/G11 cluster) there is a command
which will add the colour (**note the spelling of *color***):

```
$ git config --global --add color.ui true                       # Note US spelling of color
```
{: .language-bash}

---

### Add colour to `diff`

```
$ git config --global color.diff auto
```
{: .language-bash}

---

### Configure a visual diff tool

`git diff` is ok, but not very user friendly.
It represents changes as removal of a line, followed by the addition of a new line.
There are many diff GUIs available, which can be much easier to work with.
To view differences with a GUI instead of using the command-line diff tool, first configure
git to use your chosen diff tool:

```
$ git config --global diff.tool diffmerge    # Set diffmerge as your visual diff tool
$ git config --global difftool.prompt false  # Suppress confirmation before launching GUI
```
{: .language-bash}
Then to use the GUI, use the following command instead of `git diff`:

```
$ git difftool
```
{: .language-bash}

---

### `git stash`
Sometimes you are working on one branch and want to switch to another branch for
a while.
In order to do so you would normally need to have a clean working directory i.e.
no modified files or staged changes.
You could commit all the changes you have made, then switch branch, but that would
involve committing incomplete work just to return to this state later on.
`git stash` saves the dirty state of your working directory and saves it on a stack
of unfinished changes that you can reapply at any time using `git stash apply`.
See [here](https://git-scm.com/book/en/v1/Git-Tools-Stashing) for more details and
for examples.

---

### Git GUIs

There are a number of available GUIs for working with Git. The official Git
page contains a [comprehensive list](http://git-scm.com/downloads/guis).

However, Git [for Windows](https://git-for-windows.github.io/) already comes
with all the tools you need (Git Bash, Git GUI, Shell integration).

Some IDEs already have integration with version control e.g. MATLAB, R studio.

---

### Git configuration

The global configuration file for git `.gitconfig` is automatically created by
Git in the `home` directory. If you set up some basic configuration (in the
first steps of this tutorial), it should look like this.

```
$ cat ~/.gitconfig
```
{: .language-bash}

```
[user] name = Your Name email = yourname@yourplace.org
[core] editor = gedit
```
{: .output}

You can add more configuration options. For example, instead of typing `git
commit -m` we can have a shorter version of this command:

```
$ git config --global alias.cms 'commit -m'
```
{: .language-bash}

And now our configuration file will have a new section added:

```
… [alias] cms = commit -m
```

Next time we can simply type:

```
$ git cms "Commit message"
```
{: .language-bash}

---

### Completely removing unwanted files from the repository    

As we discussed earlier, there are a number of ways to undo what we did in Git.
However, most of the time, we actually want to make some amendments rather than
discard everything completely. Also often undoing things means, in fact,
creating a new commit (not abandoning them). Since Git is a version control
system, everything that we recorded in the past commits will be available in
the repository.

For example, if you accidentally committed a file with sensitive data (passwords)
in your local repository and then pushed it to the remote repository, the file
will be there even if in the next commit-and-push you'll remove it (`git rm`).

This [article](https://help.github.com/articles/remove-sensitive-data) provides
a step-by-step tutorial on how to remove completely files from your repository
(purge the repository) using `git filter-branch`.

Removing files from the repository may be useful not only when the files
contain sensitive data. Another case may be if you committed a large file in
your local repository. Essentially, by default, there are no limitations on the
size of files you can commit. However, there may be (and quite likely there
will be) limits on the size of the files you can push to remote repositories
(GitHub allows for max 100MB). You may encounter an annoying situation when you
committed a large file locally and then kept on working making local commits but
not pushing. Finally, you decide to push to GitHub (or elsewhere remote) and
you can't because the file is too big. Using `git rm` won't help because you
are pushing since the last pushed commit and that means in between there is
a commit with the large problematic file. To recover from this you will have to
purge your large file from the repository (or switch to a different remote repository
provider that allows for large files).

Again, as always with Git **before** you execute the above, make sure you know
what you're doing!

[editing hunks]: http://joaquin.windmuller.ca/2011/11/16/selectively-select-changes-to-commit-with-git-or-imma-edit-your-hunk


When a repository with source code, a manuscript or other creative
works becomes public, it should include a file `LICENSE` or
`LICENSE.txt` in the base directory of the repository that clearly
states under which license the content is being made available. This
is because creative works are automatically eligible for intellectual
property (and thus copyright) protection. Reusing creative works
without a license is dangerous, because the copyright holders could
sue you for copyright infringement.

A license solves this problem by granting rights to others (the
licensees) that they would otherwise not have. What rights are being
granted under which conditions differs, often only slightly, from one
license to another. In practice, a few licenses are by far the most
popular, and [choosealicense.com](https://choosealicense.com/) will
help you find a common license that suits your needs.  Important
considerations include:

* Whether you want to address patent rights.
* Whether you require people distributing derivative works to also
  distribute their source code.
* Whether the content you are licensing is source code.
* Whether you want to license the code at all.

Choosing a license that is in common use makes life easier for
contributors and users, because they are more likely to already be
familiar with the license and don't have to wade through a bunch of
jargon to decide if they're ok with it.  The [Open Source
Initiative](https://opensource.org/licenses) and [Free Software
Foundation](https://www.gnu.org/licenses/license-list.html) both
maintain lists of licenses which are good choices.

[This article][software-licensing] provides an excellent overview of
licensing and licensing options from the perspective of scientists who
also write code.

At the end of the day what matters is that there is a clear statement
as to what the license is. Also, the license is best chosen from the
get-go, even if for a repository that is not public. Pushing off the
decision only makes it more complicated later, because each time a new
collaborator starts contributing, they, too, hold copyright and will
thus need to be asked for approval once a license is chosen.

## Can I Use Open License?
 Find out whether you are allowed to apply an open license to your software.
Can you do this unilaterally,
or do you need permission from someone in your institution?
 If so, who?

{: .challenge}

## What licenses have I already accepted?
 Many of the software tools we use on a daily basis (including in this workshop) are
released as open-source software. Pick a project on GitHub from the list below, or
 one of your own choosing. Find its license (usually in a file called `LICENSE` or
`COPYING`) and talk about how it restricts your use of the software. Is it one of
 the licenses discussed in this session? How is it different?
 - [Git](https://github.com/git/git), the source-code management tool
 - [CPython](https://github.com/python/cpython), the standard implementation of the Python language
 - [Jupyter](https://github.com/jupyter), the project behind the web-based Python notebooks we'll be using
 - [EtherPad](https://github.com/ether/etherpad-lite), a real-time collaborative editor

{: .challenge}

[software-licensing]: https://doi.org/10.1371/journal.pcbi.1002598

Free sharing of information might be the ideal in science,
but the reality is often more complicated.
Normal practice today looks something like this:

*   A scientist collects some data and stores it on a machine
    that is occasionally backed up by her department.
*   She then writes or modifies a few small programs
    (which also reside on her machine)
    to analyze that data.
*   Once she has some results,
    she writes them up and submits her paper.
    She might include her data -- a growing number of journals require this -- but
    she probably doesn't include her code.
*   Time passes.
*   The journal sends her reviews written anonymously by a handful of other people in her field.
    She revises her paper to satisfy them,
    during which time she might also modify the scripts she wrote earlier,
    and resubmits.
*   More time passes.
*   The paper is eventually published.
    It might include a link to an online copy of her data,
    but the paper itself will be behind a paywall:
    only people who have personal or institutional access
    will be able to read it.

For a growing number of scientists,
though,
the process looks like this:

*   The data that the scientist collects is stored in an open access repository
    like [figshare](https://figshare.com/) or
    [Zenodo](https://zenodo.org), possibly as soon as it's collected,
    and given its own
    [Digital Object Identifier](https://en.wikipedia.org/wiki/Digital_object_identifier) (DOI).
    Or the data was already published and is stored in
    [Dryad](https://datadryad.org/).
*   The scientist creates a new repository on GitHub to hold her work.
*   As she does her analysis,
    she pushes changes to her scripts
    (and possibly some output files)
    to that repository.
    She also uses the repository for her paper;
    that repository is then the hub for collaboration with her colleagues.
*   When she's happy with the state of her paper,
    she posts a version to [arXiv](https://arxiv.org/)
    or some other preprint server
    to invite feedback from peers.
*   Based on that feedback,
    she may post several revisions
    before finally submitting her paper to a journal.
*   The published paper includes links to her preprint
    and to her code and data repositories,
    which  makes it much easier for other scientists
    to use her work as starting point for their own research.

This open model accelerates discovery:
the more open work is,
[the more widely it is cited and re-used](https://doi.org/10.1371/journal.pone.0000308).
However,
people who want to work this way need to make some decisions
about what exactly "open" means and how to do it. You can find more on the different aspects of Open Science in [this book](https://link.springer.com/book/10.1007/978-3-319-00026-8).

This is one of the (many) reasons we teach version control.
When used diligently,
it answers the "how" question
by acting as a shareable electronic lab notebook for computational work:

*   The conceptual stages of your work are documented, including who did
    what and when. Every step is stamped with an identifier (the commit ID)
    that is for most intents and purposes unique.
*   You can tie documentation of rationale, ideas, and other
    intellectual work directly to the changes that spring from them.
*   You can refer to what you used in your research to obtain your
    computational results in a way that is unique and recoverable.
*   With a version control system such as Git,
    the entire history of the repository is easy to archive for perpetuity.

## Making Code Citable
 Anything that is hosted in a version control repository (data, code, papers,
etc.) can be turned into a citable object.
Details and tutorial is [here](https://guides.github.com/activities/citable-code/)

{: .callout}

## How Reproducible Is My Work?
Ask one of your labmates to reproduce a result you recently obtained
using only what they can find in your papers or on the web.
Try to do the same for one of their results,
 then try to do it for a result from a lab you work with.

{: .challenge}

## How to Find an Appropriate Data Repository?
Surf the internet for a couple of minutes and check out the data repositories
 mentioned above: [Figshare](https://figshare.com/), [Zenodo](https://zenodo.org),
[Dryad](https://datadryad.org/). Depending on your field of research, you might
find community-recognized repositories that are well-known in your field.
You might also find useful [these data repositories recommended by Nature](
https://www.nature.com/sdata/data-policies/repositories).
Discuss with your neighbor which data repository you might want to
approach for your current project and explain why.

{: .challenge}

## How to Track Large Data or Image Files using Git?
Large data or image files such as `.md5` or `.psd` file types can be tracked within
 a github repository using the [Git Large File Storage](https://git-lfs.github.com)
open source extension tool.  This tool automatically uploads large file contents to
a remote server and replaces the file with a text pointer within the github repository.
Try downloading and installing the Git Large File Storage extension tool, then add
tracking of a large file to your github repository.  Ask a colleague to clone your
repository and describe what they see when they access that large file.   

{: .challenge}
We've seen how we can use version control to:

* Keep track of changes like a lab notebook for code and documents.  
* Roll back changes to any point in the history of changes to our files - "undo" and
"redo" for files.  
* Back up our entire history of changes in various locations.  
* Work on our files from multiple locations.  
* Identify and resolve conflicts when the same file is edited within two
repositories without
losing any work.  
* Collaboratively work on code or documents or any other files.

Now, consider again our initial scenario:

If someone asks you, "Can you process a new data file in exactly the same
way as described in your journal paper? Or can I have the code to do it myself?"
You can use your version control logs and tags to easily retrieve the exact
version of the code that you used.

Version control serves as a log book for your software and documents, ideas
you've explored, fixes you've made, refactorings you've done, false paths
you've explored - what was changed, who by, when and why - with a powerful undo
and redo feature!

It also allows you to work with others on a project, whether that be writing
code or papers, down to the level of individual files, without the risk of
overwriting and losing each others work, and being able to record and
understand who changed what, when, and why.

### Find out more...

* [Download](https://git-scm.com/downloads) and install Git on your own computer (it's free!)
* [Atlassian Git tutorials](https://www.atlassian.com/git/tutorials/) --- an
excellent resource with clear explanations and illustrations
* [Learn Git branching](https://learngitbranching.js.org/) --- interactive, visual tutorials
* K. Ram  (2013) "git can facilitate greater reproducibility and increased
transparency in science", Source Code for Biology and Medicine 2013, 8:7
doi:[10.1186/1751-0473-8-7](http://dx.doi.org/10.1186/1751-0473-8-7) --- survey
of the range of ways in which version control can help research.  
* [Visual Git Reference](http://marklodato.github.com/visual-git-guide/index-en.html)
--- pictorial representations of what Git commands do
* [Pro Git](http://git-scm.com/book) --- the "official" online Git book.  
* [Version control by example](http://www.ericsink.com/vcbe/) --- an acclaimed online book
on version control by Eric Sink.  
* [Git beyond the basics](https://speakerdeck.com/zakkak/git-beyond-the-basics) --- a nice reference
slideshow covering some more advanced topics
* G. Wilson, D. A. Aruliah, C. T. Brown, N. P. Chue
Hong, M. Davis, R. T. Guy, S. H. D. Haddock, K. Huff, I. M. Mitchell, M.
Plumbley, B. Waugh, E. P. White, P. Wilson (2012) "[Best Practices for
Scientific Computing](http://arxiv.org/abs/1210.0530)", arXiv:1210.0530
[cs.MS].
