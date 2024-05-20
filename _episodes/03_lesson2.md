---
layout: page
title: Git Tutorial 2
order: 3
session: 1
length: 15
toc: true
---

## Tutorial: Part 2 - Typical Workflow

We should now all be set up to work between our local system and our *remote repository* on GitHub.

We will cover getting local changes onto the remote, getting remote changes onto our local repository, creating *branches* and *tags*, and finally see a simple *merge*.

This is intended as a demonstration of git.
These commands will be explained and explored more in-depth in the remainder of the course, so don't worry about the details of the steps we're taking.
Feel free to write down any questions that arise and we shall make sure they are answered when taking the deeper dive.

### Commit changes to the README

On our local system, use a text editor to add your name to the `README.md` file.

Open VS Code, and use the **File > Open Folder...** dialogue to open our working directory.
On most systems you should be able to do this directly from the terminal with the command

``` sh
code .
```

Open `README.md` and make changes to the file, adding your name to the bottom, and save the file.
We can check the differences with the command

``` sh
git status
```

To *stage* the changes to be committed, use the command

``` sh
git add README.md
```

Check the status again:

``` sh
git status
```

Then *commit* the change to the local repository

``` sh
git commit --message "Add author to README"
```

We can now see the new commit, along with the original commit:

``` sh
git log
```

We can tag this commit with the title `v1` with

``` sh
git tag v1
```

### Push the commit and the tag

Looking at the status again,

``` sh
git status
```

we can see that we have made changes that are not yet on GitHub.

To get the most recent commit on the `main` branch to the remote `origin`, use

``` sh
git push origin main
```

Opening the GitHub page with our browser, we can see the new commit.

### Add a file to a new branch called "feature"

*Branches* allow us to make changes to the repository whilst leaving the `main` version of the code untouched.

We can create a new *branch* in our repository that we shall name `feature`:

``` sh
git branch feature
```

We can see the new branch with the command `git branch`:

``` sh
$ git branch
  feature
* main
```

We are still working on the `main` branch, indicated by the asterisk.
To change to the new branch, use

``` sh
$ git switch feature
Switched to branch 'feature'
```

We now want to use the editor to create a new file.
Create a new file `story.md` with a title, save it.
Then use `git add` and `git commit` to add the file to the repository.

``` sh
# Edit a new file story.md in VS code and save
git add story.md
git commit --message "New file story.md"
```

We can see the extra commit with

``` sh
git log
```

### Make a change to README through GitHub

We can also make changes to files directly in GitHub.
This is typically reserved for small changes to documentation files.
To see this in action, navigate to `README.md` in the browser, and click the edit button which looks like a pencil (✏️).

Add the date to the line with your name, then go through the "Save Changes" dialogue, adding an appropriate commit message.

Once this is done, we need to *pull* the change from GitHub onto our local repository.

``` sh
git pull origin main
```

### Merge the branch "feature" into "main"

In the final step of this demo, we're going to integrate the work we did on the local branch with the change we just got from GitHub.

Assuming we are happy with the addition we made in our `feature` branch, we can now `merge` the two branches.
This will result in our code having both changes.

In the terminal we switch back to the `main` branch:

``` sh
git switch main
```

To apply the changes we made in the `feature` branch, make a new *merge* commit:

``` sh
git merge feature --message "Merge feature into main"
```

### Reviewing the repository and tidying up

Looking in the editor we should now see both the file `story.md` along with the updated file `README.md`.
All the changes we've made are visible through the log:

``` sh
git log
```

To see a more concise log, use

``` sh
git log --oneline --all --graph
```

We can tidy up our repository and safely delete the `feature` branch.

``` sh
git branch --delete feature
```

### Summary

This concludes our tutorial.
We have worked with both local and remote (GitHub) copies of our code, moving local changes to GitHub and vice-versa.
We've also see some basic tagging, branching and a merge.

In the remainder of this course we will take a more in-depth look at the commands used above.
This will give us a greater understanding of what we have done, as well as seeing the different options we can make use of when working collaboratively on larger projects.
