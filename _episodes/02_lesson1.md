---
layout: page
title: Git Tutorial 1
order: 2
session: 1
length: 15
toc: true
---

## Tutorial: Part 1 - Git Setup

We'll start by running through a short tutorial covering configuration, committing changes, GitHub, pushing and pulling, tagging, branching, and merging.
This is split in two parts.

Part 1 will act as a checkpoint to ensure everyone has their credentials set up to work between their local *terminal* and GitHub.
Unless you already have your system set up for access, you will need your GitHub username and password.

Part 2 will take you through a demonstration of a typical workflow.
This should be mostly familiar, perhaps will a few tasks you've not met before.

Now to get started using Git.
Being able to complete these steps is vital for remainder of the course, so don't hesitate to ask for help from the helpers in the room or online.

### Open a new directory in the terminal

To start, we want to create a new directory, and have a terminal open in this directory.
This step will change depending on the operating system (OS) we're working will, and will be demonstrated in the classroom.

### Check the version of git

From the terminal, we want to confirm that git is installed.
Type the first line of the following command (*excluding the leading dollar sign* `$`) then press `Enter`.

``` sh
$ git --version
git version 2.45.0.windows.1
```

If git is installed, you should see something like the second line of the above code.
Your output may change depending on your OS and how recently git was installed.

### Configure your name and e-mail

We want to make sure our name and e-mail address is being attached to commits.
We can check the values of the config file with the command

``` sh
git config --global --list
```

If your name and e-mail are not as expected, set them with the following command:

``` sh
git config --global user.name "<Your Name>"
git config --global user.email "<your-id>@exeter.ac.uk"
```

replacing the text in angle brackets (`<...>`) with your details.

### Create a new repository on GitHub

We are going to create a blank repository on GitHub and then create and upload a file from our local machine.

Navigate your browser to <https://www.github.com> and sign in (if not already).
In the left navigation panel, use the green **New** button.

Enter a repository name (e.g., `test-repo`) and, leaving the other fields with their default values, use the **Create repository** button at the bottom.

From the page that follows, make sure HTTPS (not SSH) is selected below Quick setup.
Copy the instructions in the section titled **...or create a new repository on the command line** into your terminal window.

They will look something like the following, but with the URL for your new repository on the line starting `git remote`:

``` sh
echo "# test-repo" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/username/test-repo.git
git push -u origin main
```

The last `git push` command may ask you to put in your GitHub username and password.
You should use your username and a [personal access token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens) (further instructions can be found on the [Introduction To VCS: Setting up GitHub credentials](https://coding-for-reproducible-research.github.io/CfRR_Courses/individual_modules/introduction_to_version_control/configuring_git.html#setting-up-github-credentials)).

If the above commands were successful, we should now be able to refresh the website for our repository in the browser and see the new file.

### Summary

We should now have a local git repository, and an equivalent repository on GitHub.
It is important that everyone gets to this point, so be sure to ask for help if you're stuck.

Next, we're going to see some git commands that make up a typical work flow.
