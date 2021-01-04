---
layout: page
title: Setup
description: >-
    Setting up your computer in preparation for the minicourse.
---

# Setup
{:.no_toc}

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Python

We will not directly program in Python, but some of the tools we use depend on it, so make sure you have installed Python **3**. We recommend using the [Anaconda](https://www.anaconda.com/products/individual) Python distribution.

To test whether you have Python 3 installed, open the Termninal (on MacOS or on Linux) or the Anaconda Prompt (Windows) and type 

```
python -V
```
and run the command with the return key.

This should print the python version.

## Git and GitHub

We will use GitHub to manage experiment files and host the experiment. To interact with GitHub, you need to make sure that you have `git` installed. 

To test whether you have git installed, open the Termninal (on MacOS or on Linux) or the Anaconda Prompt (Windows) and run 

```
git --version
```

If you get an error message, make sure to install Git:

1. [Install git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git). You can read the rest of the linked tutorial to learn more about git.
2. [First time Git setup](https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup): The main thing is to configure `user.name` and `user.email`

If you don't already have a GitHub account,  [sign up for one](https://github.com/).

## Prolific and Proliferate

We will test experiments on [Prolific Academic](https://prolific.co), a platform for recruiting and paying participants. For collecting data, we will use [Proliferate](https://docs.proliferate.alps.science).

1. [Sign up](https://app.prolific.co/register/researcher) for a Prolific account (if you don't already have one).
2. [Sign up](https://proliferate.alps.science/admin/signup) for a Proliferate account.
3. [Install the proliferate command line tool](https://docs.proliferate.alps.science/en/latest/cli/setup.html).

## RStudio

For analyzing the data we will use R and the [tidyverse](https://tidyverse.org) library.

1. [Install R](https://cran.rstudio.com/)
2. [Install RStudio](https://www.rstudio.com/products/rstudio/download/)
3. Install the `tidyverse` package: Lauch RStudio and run in the console: `install.packages("tidyverse")`  
**(Note that quotes are needed when you install packages)**
4. You need to load the package before you can use it:
`library(tidyverse)`  
**(Note that there are NO quotes when you load packages)**  
If you see a list of attached packages (including `ggplot2`, `dplyr` etc) then everything is good. (Don't worry about the conflicts printed afterwards.)

## Text editor

We will edit the code for some experiments and it is generally much easier to edit code with a good editor that supports syntax highlighting of your code. In case you don't have an editor installed, [Atom](https://atom.io/) is a good free editor.

## OSF

We will cover the theoretical background of pre-registration and pre-register the experiment that you'll test on the Open Science Foundation (OSF) platform.

1. [Sign up](https://osf.io/register) for an OSF account (if you don't already have one).

## Checklist

Here is a recap of all the things that should be set up before the course:

* Python 3 is installed.
* Git is installed
* You have a GitHub account
* You have a Prolific account
* You have a Proliferate account
* The Proliferate command-line utility is installed
* R is installed
* RStudio is installed
* A text editor is installed
* You have an OSF account.
