---
title: "Tracking changes with a local repository"
teaching: 10
exercises: 0
questions:
- "How to configure Git locally?"
objectives:
- "Know how to set Git credentials."
- "Be able to commit changes to your repository."
keypoints:
- "lets configure git"
"`git init` initializes a new repository"
---

## Version Control
Version control is centered round the notion of a *repository* which holds your
directories and files. We'll start by looking at a local repository. The local
repository is set up in a directory in your local file system (local machine).
For this we will use the command line interface.

> ## Why are we using the command line?

> There are lots of graphical user interfaces (GUIs) for using Git, both stand-alone and integrated into text editors (e.g. VSCode/atom/sublime/...).
> So why aren't we using those today?
> Using the terminal is a great way to understand the process underlying `git`.
> Unluckily, there is (almost) no substitute for that. Moreover, learning the basics in a terminal will help you use `git` everywhere and in any condition - from HPCs to your geeky friend's laptop. You will still be able to learn how to use any GUI you would prefer later.
> Besides, how are you going to show off your `git` skills, if not by using a terminal? ;)

## Setting up Git

Check if you already have git installed and if that is not the case, install it - you can find instructions [here]({{ page.root }}/setup).

## Git configuration

Git records information not only about the changes to files,
but also about _who_ made those changes.
In collaborating, this information is often critical
(e.g., you probably want to know who rewrote your 'Conclusions' section!).
So, we need to tell Git about who we are:

```
$ git config --global user.name "Your Name" 			# Put quotation marks around your name
$ git config --global user.email yourname@yourplace.org
```

Note that you need to enclose your name in quotation marks!

### Optional (but recommended): Set a default editor

When working with Git we will often need to provide some short but useful information (commit messages).
It's possible to enter such information in the command itself, but in case you forget, `git` will open your favorite editor to remind you to do that.
However, you need to tell `git` which editor you prefer first.
You can choose any editor available on your system.

```
$ git config --global core.editor nano
```

To set up alternative editors, follow the same notation e.g.
`git config --global core.editor vim`, `git config --global core.editor notepad`, `git config --global core.editor atom`,
`git config --global core.editor xemacs`, ...

Mac users can use *TextEdit*: `git config --global core.editor 'open -W -n'`.

### Git's global configuration

We can now preview (and edit, if necessary) Git's global configuration (such as
our name and the default editor which we just set up). If we look in our home
directory, we'll see a `.gitconfig` file,

```
$ cat ~/.gitconfig
    [user] name = Your Name email = yourname@yourplace.org
    [core] editor = nano
```

If you are executing this tutorial locally, these settings would similarly persist over time;
i.e. the `--global` commands above are only required once per computer.
