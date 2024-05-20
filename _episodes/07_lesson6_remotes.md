---
layout: page
title: Remotes - GitHub
order: 7
session: 1
length: 20
toc: true
adapted: false
---

## Working with Remotes

At the start of the session we first created our repository on GitHub.
We then linked our *local repository* with the *remote repository*.
Git then tracks branches on the remote repository.

The information for the remote branches is stored in two places:

- `.git/config` contains information on the URL for the remote
- `.git/refs/remotes` contains a reference to the commit

Instead of rooting around in the `.git` directory (which should generally be avoided), we can find the details of the remote with

``` sh
git remote show origin
```

GitHub is useful, since it has extensive tooling to allow collaboration (more on this in the next session).
There are alternative which serve a similar purpose (such as Bitbucket and GitLab).

### Sending Changes to GitHub

Since we are working with our own repository, we have the relevant permissions to upload our local changes to our remote repository.

``` sh
git push origin
```

When uploading a new branch, we can specify that we want git to keep track of differences between the local and remote branches.

``` sh
git push -u origin feature-branch
```

If we use `git checkout` and supply a branch-name that only exists on a remote, git will automatically set up branch tracking for us.

### Fetch From Remote

To get any recent changes from a remote, we need to `fetch`.
To demonstrate this, edit a file using GitHub, then

``` sh
git fetch origin
```

To see this on our local copy, we'll have to use the `--all` flag in `log`:

``` sh
git log --oneline --graph --all --simplify-by-decoration
```

In order to get these changes onto our branch, we have to fast-forward with `git merge`.
Since this is such a common action (fetch and merge), the two actions are combined into a single command,

``` sh
git pull origin main
```

### Multiple Remotes

So far we have just seen the single remote `origin`.
We can use multiple remotes to track other people's versions of the code.
In this way, we can mark multiple remote repositories from which we can pull changes.

To add a new remote, we use

``` sh
git remote add <name> <url>
```

where `<name>` is our local title and `<url>` is the network path for the new remote.

By default we can't push changes to someone else's GitHub repository.
Instead, we can **Fork** the repository, giving us our own version (associated with our GitHub user account).
If we add the original repository as a second remote, we can still track and `pull` changes on the *upstream* repository.
We can `push` those changes, along with our own additions, to our forked version.

We will see some typical workflows that use multiple remotes for collaboration in the next session.

#### Note on Forking

If there are libraries on GitHub on which your code relies, it can be a good idea to create a fork.
Even if you don't intend on making any changes, you will have a copy which cannot be deleted by the original author.

### Summary

We have seen how to work with remote repositories, and how download and upload changes.

This concludes the first session of the Intermediate Version.
In the next session we will look at typical workflows using GitHub.
We'll also see how we can rewrite our history, as well as the tools we have available to get unstuck when applying these more complicated changes.

### Exercise

We will go through an example of creating our own fork of a public repository, cloning our own version of it, then adding an upstream remote to the original repository.
