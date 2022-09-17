---
date: "2016-04-09T16:50:16+02:00"
title: Git and GitHub
weight: 3
---

## Why do we need it and what is it? 🤔

Working on a DS project will create a large number of files that you want to keep control over and give some structure to. Often your work could benefit from collaboration with others so you need to make it available for reading or edits, as a project unfolds. You could manage those issues by implementing the free and open source version control system Git into your project workflow. It is a tool that tracks changes to your files and enables you to share those changes with others. Those Git configured sets of files are called the repositories or repos and are organised in a highly structured way. 

To provide a home for your Git-based data analytical projects on the internet you can use the hosting site GitHub for statistical and data scientific work flows. It will help you manage the varied accumulation of files as your project develops, starting with a data file, through the creation of figures, reports, and of course your ultimate pride and joy, the source code file.

## Setting Git up

If you have never used Git or GitHub before, start by installing Git and creating a GitHub account. Then, link the two together.

1) You need to install Git from [git-scm](https://git-scm.com/downloads)
Note: It will automatically detect your operating system and give you a link to download.

2) Tell Git your name and email address. These are used to label each commit so that when you start collaborating with others, it is clear who made each change. 

### On a Mac

The first step is to check if Git is already installed on your computer. (Note: Git should be installed automatically on Macs.).

Open the terminal and type

```
git --version
```
You should get something like this

```
git version 2.18.0
```

In case you have an older version or you don’t have it installed then just go to the website: [git-scm.com](https://git-scm.com).

Once you have the latest Git version on your Mac set up your identity by typing the following commands in your terminal:

```
git config --global user.email "your@email.com"
git config --global user.name "your name"
```


### On a PC 

Run CMD to obtain a command prompt window in which you should type the following command to check if Git already exists on your computer:

```
git --version
```

If Git is not recognised, that means that it needs to be installed from <https://gitforwindows.org>. Download the necessary file and run the installation by selecting all the defaults. Close the command window to allow for the change in your system to go through and reopen it. Once again type the above command.

Now, after Git has been added to your system you should set it up by providing your email address and your name:

```
git config --global user.email "your@email.com"
git config --global user.name "your name"
```


In the shell, run:


<http://rstudio-pubs-static.s3.amazonaws.com/272737_7d6178a0e50043528d9ba636fdde209e.html>

## Let RStudio talk to GitHub 🤓

RStudio has integrated facilities that make the use of Git simpler. You will have to go through this set-up once or once per computer.

Once you have enabled communication between RStudio and GitHub on your computer for new or existing R projects, you will:

- Dedicate a directory (a.k.a “folder”) to it
- Make it an RStudio Project
- Make it a Git repository

{{% notice note %}}
You might find it difficult and frustrating to use Git at first, but as with many other tools you will find it easier the more you practise. Practice makes perfect!  
{{% /notice %}}

We will go and learn how to use GitHub in RStudio by exploring the material available at [RStuio's website](https://support.rstudio.com/hc/en-us/articles/200532077-Version-Control-with-Git-and-SVN). 

First we will start by setting up SSH keys that will allow you to securely communicate with websites without a password. To do this follow the instructions from the [`Git and GitHub`](http://r-pkgs.had.co.nz/git.html) section in [Hadle's](http://hadley.nz) book [R Packages](http://r-pkgs.had.co.nz).

You can read about how to use GitHub further by exploring [`Chapter 9: Connect to GitHub`](https://happygitwithr.com/push-pull-github.html) in [Jenny's](https://jennybryan.org) book [Happy Git and GitHub for the useR](https://happygitwithr.com/index.html).

{{% notice tip %}}
You might find it useful bookmarking the link for [GitHub Cheat Sheet](https://education.github.com/git-cheat-sheet-education.pdf)!
{{% /notice %}}


-----------------------------
[Creative Commons Attribution-ShareAlike 4.0 International License.](https://creativecommons.org/licenses/by-sa/4.0/)
