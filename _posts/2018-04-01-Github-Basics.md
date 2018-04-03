---
title:  "Contributing to the SDF with Github"
date:   2018-04-01 16:16:01 -0600
categories: learning bounties
post_importance: 3
bounty: 10
---
This is an unfinished article. Earn {{ page.bounty }} STRAT by completing this article. Edit it [here](https://github.com/StratisDevelopmentFoundation/StratisDevelopmentFoundation.github.io/edit/master/{{ page.path }}).
{: .notice--info}

# Contributing to the SDF with Github

This article will show you how to use Git and Github to fork an SDF github repo, and create a pull request to submit work to the SDF

## sign up for github
1. Enter the website https://github.com
2. Register your account (if you have account you can ommit this step)
3. Sign up to your account.

## Install git
1. [Download git](https://git-scm.com/downloads) (version depends from your OS system)
2. Install git.exe (click next button until starting installation)
3. Before fork or clone repository you must set up your git. [Here](https://help.github.com/articles/set-up-git/) 
   is good explanation (Setting up Git chapter and three steps below) 

## fork an SDF repo
1. Navigate to [SFD repository](https://github.com/StratisDevelopmentFoundation)  
2. Click repository StratisDevelopmentFoundation.github.io
3. Find Fork button and click it
   You have just done fork repostiory into your account

## clone the repo locally
1. Navigate to your fork, should be like this *https://github.com/YOUR-USERNAME/StratisDevelopmentFoundation.github.io*
2. Under the repository name, click *Clone or download*.
3. You can clone repostiory by HTTPS or SSH protocol (if you prefer second option, you must first [generate SSH keys](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/)
4. Open Git Bash
5. Move to your catalog (use cd .., cd, ls command) 
6. Use *git clone git@github.com:YOUR-USERNAME/StratisDevelopmentFoundation.github.io.git*

## modify the repo locally
Before you start you must create your own local branch:
1. Open Git Bash 
2. Move to catalog ../StratisDevelopmentFoundation.github.io on your computer (use *cd .., cd, ls* command) 
3. Use *git checkout -b "name_of_your_new_branch"* (you can check your current branch with git branch)
4. You can write article on your repository locally right now. When you finished you can commit your changes. 
   Below steps describe it.

## commit changes to your github fork
1. Open Git Bash
2. Use *git status*. You can see result of your changes.
3. Use *git add -A* 
4. Use *git commit -m "commit_name"* 
5. Use *git push origin "name_of_your_new_branch"*

## create a pull request to the original SDF repo
1. Enter into your account Github
2. Select repository *StratisDevelopmentFoundation.github.io*
3. Click *New Pull Request* button
4. Select your own branch to comparison with *base: master* SDF repository
5. Click *Create Pull Request* button

# Remaining tasks to complete this article

* Follow existing steps, increase clarity and add more specific steps
* Add screenshots

Try to find existing articles about pull requests? These should already exist.
