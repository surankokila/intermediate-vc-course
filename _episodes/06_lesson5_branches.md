---
layout: page
title: References, Tags and Branches
order: 7
session: 1
length: 20
toc: true
adapted: false
---

## Branches and Merging

We've seen the mechanisms for commits in some detail.
You should feel confident creating a chain of annotated commits (snapshots) and seeing that history with `git log`.
We've also seen how we can use a SHA (*secure hash algorithm*) to get a copy of the files from any commit in the history.

The we're going to look at an important topic in git, **branching**, and the closely related concept of **merging**.

### Branches

So far we have mostly been working with a single **branch** called `main`.
In this case, `main` is a **reference** which points to the most recent commit.
Any time a new commit is added to the end of this chain, `main` is updated to this new commit.

Git is keeping track of `main` in a file `.git/refs/head/main`:

``` sh
cat .git/refs/heads/main
```

#### Creating Branches

We can create new branches, giving us the flexibility to work progressively on new features.
This approach enables multiple people to work on the same repository, as we shall see later.

To create a branch called `feature-branch` at the current commit, use

``` sh
git branch feature-branch
```

Running `git branch` by itself should now show both `main` and `feature-branch`.
If we change to our new branch

``` sh
git switch feature-branch
```

then any new commits will advance the head of `feature-branch`, leaving `main` untouched.

To change back to the default branch, we can use

``` sh
git switch main
```

### Tags

As a brief aside, we are going to look at **tags**.
As an example, we can add a tag titled *v0.1* to the most recent commit with

``` sh
git tag v0.1
```

This creates a lightweight reference in `.git/refs/tags/v0.1`.
References in tags do not change as we add new commits (in contrast to branches).
This makes them ideal for marking commits with version numbers which can e.g., be tied to code outputs.

We can later restore the code to this version using the tag name instead of the SHA:

``` sh
git checkout v0.1
```

See <https://semver.org/> for best practice of naming versions.

References, such as branches and tags, are visible in the log.
One potentially useful view is *only* the commits with references:

``` sh
git log --oneline --simplify-by-decoration
```

### Creating Commits on Branches

If a branch is checked out (`HEAD` points at the branch), the branch reference is updated when we add commits.

This is demonstrated in the following example.

If we consider two commits:

- `a1a` with tag `v1` and branch `main`; and
- `2f7` with tag `v2` and currently checked-out branch `feature`:

``` none
            HEAD
 main     feature
  v1         v2
  |          |
 a1a   ->   2f7
```

The `HEAD` is following `feature` and currently at `2f7`.
If we create a new commit now (say `83c`):

- `83c` will be a child of `2f7`,
- the `HEAD` and `feature` references will both be updated to `83c`,
- but the tag `v2` (and `v1` and the branch `main`) be remain unchanged.

``` none
 main                  HEAD
  v1         v2      feature
  |          |          |
 a1a   ->   2f7   ->   83c
```

If we were to move to the `main` branch (with `git switch main`) and add a new commit, our history would diverge.
This happens frequently when collaborating on a repository.

![A chain of commits with a diverging history.](../images/history2.png)

### Merging

Returning to branches, a typical way of working on code is to use a branch (appropriately named) to develop a single feature.
Once we're happy that the feature meets our requirements, we move the work back on to the main branch.
We will now look at how to combine changes from different branches, a process known as **merging**.

#### Simple Merge - Fast-Forward

The simplest case is where `main` has not been updated and we want to just apply the changes from our feature branch.
Here the reference to `main` can simple be moved to the tip of `feature-branch` (referred to as a **fast-forward**).
One approach (which overlaps with the following methods) is to change to the main branch

``` sh
git switch main
```

then fast-forward with

``` sh
git merge feature-branch
```

#### Merging with Diverged Histories

In the case that changes have been made to `main`, the same syntax will combine changes from both branches.

``` sh
git merge feature-branch
```

The merge will conclude automatically if the changes were on different lines.
If the changes overlapped between the two branches, we must manually resolve the conflicts.

### Exercise

Practice making branches, adding commits to each branch then merging the branches.
Try making changes in different files, in the same file but different lines, and in the same file and the same lines.

Remember to check `git status` often!
