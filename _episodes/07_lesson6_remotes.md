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

TODO:
