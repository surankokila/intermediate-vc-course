---
layout: page
title: Basic Git
order: 5
session: 1
length: 45
toc: true
adapted: false
---

## Basic Git

We're going to review some git commands used to create a basic series of snapshots of our code.
This will cover setting up a repository, seeing the status and history, creating new snapshots, and restoring previous snapshots of the directory.
We'll also look into what's happening "behind the scenes" to enable this workflow.

We'll cover commands for setup (`init` and `clone`), info (`status`, `diff` and `log`), making snapshots (`add` and `commit`) and loading snapshots (`restore` and `checkout`).

### Getting Help on Commands

Recall that we can see information on git commands with the `-h` flag.
For example, to see the help for `commit`, use

``` sh
git commit -h
```

### Basic Git Recap

Here is a quick summary of the commands we will cover in this lesson:

- `git init` will start tracking changes in the current directory.
- `git clone` will create a copy of a repository, such as from GitHub.
- `git status` will show us differences in the workspace relative to the last snapshot.
- `git diff` will show the line-by-line differences relative to the last snapshot.
- `git log` will show us all the previous snapshots that led to the current state of the repository.
- `git add` will mark changes to be included in the next snapshot.
- `git commit` will make a new snapshot from the files marked with `add`.
- `git restore` will undo the actions of `add`, removing the changes from the next snapshot.
- `git checkout` will load a previous state of the code.

Before looking at the details of these commands, we'll briefly discuss how git is storing the information required for tracking the version history.

### Behind the Scenes: Git's Three Locations

Conceptually, it's helpful to thing of git commands managing the state of three locations:

- Work directory
- Staging area
- Local Repository

The **Work Directory** (or **workspace**) is simply the files you can see in the root directory (with the exception of `.git/`).
If we weren't using any form of version control, we would have to be very careful with changes made to our files.

The **Staging Area** (or **index**) represents the changes made to our files that will be used for the next snapshot.

The **Local Repository** (or **repository**) contains all the snapshots of our files.

The key concept of version control with git is moving changes between these three locations.

#### Starting out: `init` and `clone`

If we have a directory that isn't currently managed by git (and isn't a subdirectory of one that is),

``` sh
git init
```

will create a (hidden) directory called `.git/` and allow us to start adding files and creating snapshots.

If a codebase is available over a network (such as on GitHub), we can get a local copy of the repository and its history with

``` sh
git clone <repository>
```

where `<repository>` is a URL or path to the repository.
By default this will get the full repository, including the full history of the default branch.
All previous snapshots will be available locally (stored in the `.git/` directory)

#### Getting information: `status`, `diff` and `log`

To get general information of changes that have been made to the files since the last snapshot, use

``` sh
git status
```

With regards to the "Three Locations", it will tell us how the **Work Directory** differs from the **Staging Area** and the most recent commit in the **Local Repository**.
The `status` command also shows information on the branch, any linked remote repositories, and updates from some more involved git commands.

We can get more detail about what changed with the `diff` command:

``` sh
git diff
```

Without any additional arguments, `diff` shows us the line-by-line changes between the **Work Directory** and the **Staging Area**.
We can supply additional arguments to file differences between commits.

To see the history of the repository we can use

``` sh
git log
```

This will start *paging* the contents of the log.
We can step through extra pages with the `Space` key, or stop the pager entirely with `q`.
The basic form of the log is not always the most useful, so we can modify this with arguments to change

- the range and number of commits (e.g., `git log -n 2`)
- which commits are shown (e.g., `git log --simplify-by-decoration`)
- the format of the log output (e.g., `git log --oneline`).

We can see the full list of arguments from the [https://git-scm.com/docs/git-log](online help).

Try different combinations of the following arguments.

- `--oneline` (show a oneline summary of the commit)
- `--all` (show all commits, not just those in the current commit's history)
- `--graph` (render a graph to visualise branching and merging)

Some other useful arguments are `--grep` (search for specific changes), `--patch` (show line-by-line changes) and `--stat` (summarise how many lines in each file changed).

#### Creating snapshots: `add` and `commit`

In order to prepare a snapshot, we need to tell git which changes the *stage*.
This is done with the add command

``` sh
git add <pathspec>
```

where `<pathspec>` is a path to a file, such as

``` sh
git add README.md
```

This will move changes from the specified file from the **Work Directory** to the **Staging Area**.

We can also use a *glob* pattern to match all matching files, such as adding all files ending in `.txt` with

``` sh
git add *.txt
```

We can move all changes to the **Staging Area** with the `--all` argument, although this often results in staging changes we didn't want tracked.
To remove a file from the index, we can use `restore`, for example

``` sh
git restore --staged README.txt
```

When we've added all the changes we want to the **Staging Area** (and checked the output of `git status`) we can create a new commit with

``` sh
git commit --message "Commit message here"
# equivalently git commit -m "Commit message here"
```

This will create a new commit in the **Local Repository** from the **Staging Area**.

If we omit the `-m`/`--message` argument, git will open an editor to prompt us to enter a message.
This will typically be either nano or vim - examples of both will be demonstrated in the session.

#### Restoring previous snapshots: `checkout`

To see a previous version of the code, we need an ID of the version.
This will either be from `git log`, or a named reference (typically the name of a tag or branch, or a modification of `HEAD`).

One example of a reference is `HEAD^`, being the commit before the most recent one (`HEAD` is the current commit, `HEAD^` is the *parent* of that commit).

The command

``` sh
git checkout HEAD^
```

will extract the commit from the **Local Repository** and change the **Working Directory** to match it.

To get back to the most recent snapshot, we can use

``` sh
git checkout main
```

### The `.git` Directory

The `.git` directory contains all the local files that git uses.

- local options (`.git/config`)
- information on the most recent snapshot (`.git/HEAD`)
- binary objects containing past snapshots and the staged files (`.git/objects`)
- references to any tags, branches and remotes (`.git/refs/`)

Almost all changes made to `.git/` should be handled by git commands (we will see a few rare exceptions later).

### Ignoring files

We can tell git that we never want to track changes in a file by adding it to a special file named `.gitignore`.
This file is typically tracked by git itself.
With this file in place, we wont get prompts about changes to this file from `git status`.
It will also be ignored by commands such as `git add --all` and `git commit --all`, although it's best not to rely on this in case you move to a previous version with an old `.gitignore` file.

There are times when we have files to be ignored on the local system but we don't want them stored in the repository in the `.gitignore`.
In these cases we can alternatively use `.git/info/exclude` - one of the rare exceptions to "leave `.git/` alone".

### Git's Fourth Location

In addition to the workspace, index and local repository, we frequently work with **remote repositories**, often described as git's fourth location.
Changes are moved between the local and remote repository with `push` and `fetch`/`pull`.
These commands will be used extensively in the second session.

![The four locations (workspace, index, repository and remote repository) and some operations moving changes between them](../images/three-four-spaces.png)

### Summary

We have taken an in-depth look at the basic git commands that allow us to create a chain of commits, and review the history of our files.
In the next lesson we'll be looking at branching, which allows us to maintain and track multiple versions of our code.

### Exercise

Explore the help files of some of the commands we've seen in this lesson.
Would any of these be worth creating a new alias for?

Discuss what you've found with your course mates.
