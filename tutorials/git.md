---
layout: default
title: Git Tutorial
parent: Tutorials
nav_order: 1
---


# Git Tutorial

In this tutorial, you'll create your own repository on GitHub, 
make the repository available on your computer, edit and create some files,
and commit and push the edited files to GitHub.

## Creating a project repository

We'll create a tutorial from a template that already has a basic structure
and a basic version of the experiment that you will run as part of this course.

1. Go to [https://github.com/sebschu/my-project](https://github.com/sebschu/my-project).

2. Click the green "Use this template button" to create a new repository from this template.

3. Give the template a name (e.g., `my-project`) and click "Create repository from template."

This should redirect you to your newly created repository.

## Cloning the repository

In order to edit, add or delete files, you'll have to create a copy of the repository on your computer (in Git terminology this is referred to as "cloning" a repository).

1. Create a directory on your computer where you want to store all the files.

2. On the homepage of your repository (something like https://github.com/<YOUR-USERNAME>/<YOUR-REPOSITORY-NAME>), click on the green "Code" button.
  
3. Copy the HTTPS URL to the clipboard.

4. Open the Terminal (on MacOS/Linux) or the Anaconda Prompt (on Windows).

5. Change into the directory from step 1 using the `cd` command. For example, if you have a directory called `lsa-minicourse` in your home directory, you can change into it by running 

     ```bash
     cd lsa-minicourse
     ```
   If you are using MacOS you can also type `cd ` into the Terminal and then drag the directory from Finder into the Terminal window to insert the full path
   
 6. Clone the repository:
 
     ```bash
     git clone https://github.com/<YOUR-USERNAME>/<YOUR-REPOSITORY-NAME>.git
     ```
    
    (paste the URL from the clipboard)
    
    This should download all the files from the repository into a subdirectory called `<YOUR-REPOSITORY-NAME>`.
    
 7. Change into the directory of the cloned repository:
 
     ```bash
     cd <YOUR-REPOSITORY-NAME>
     ```
    
 8. List the contents of the repository
 
 
     On MacOS/Linux:
     ```bash
     ls
     ```
     
     On Windows:
     ```bash
     dir
     ```
 
 
     This should display the names of the folders in the repository and the file README.md
     
 ## Adding and editing files
 
 Now that you have a local copy of the repository, you can add and edit files with any program as you would
 do with a regular folder on your computer.
 
 1. Add a `README.md` file to the `experiments` directory using a text editor. Add a description of the contents of the `experiment` directory to this file.
 
   (This is totally optional, but if you want to format the README file, you can use [markdown](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet).)
 
 2. Edit the `README.md` file in the `data` directory using a text editor. Add a description of the contents of the `data` directory to this file.
 


