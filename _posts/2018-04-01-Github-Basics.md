---
title:  "Contributing to the SDF with Github"
date:   2018-04-01 16:16:01 -0600
categories: tutorial
post_importance: 3
author: LukasDev
toc: true
toc_label: "Contributing through Github"
toc_icon: "x"
permalink: /contribute_to_sdf/
sidebar:
  nav: "newdev"
---
# Contributing to the SDF with Github

This article will show you how to use Git and Github to fork an SDF github repo, and create a pull request to submit work to the SDF.

You would do this process if you were working on one of the [bounties found here](/bounties/)

## Sign up for github
1. Enter the website https://github.com
2. Register your account (if you have account you can omit this step)

    ![alt text](/assets/images/register_github.png "register_github")
3. Sign in to your account
    - click *Sign in* button

        ![alt text](/assets/images/sign_in_button_github.png "sign_in_button_github")

    - write your *username* and *password*

        ![alt text](/assets/images/sign_in_username_password_github.png "sign_in_username_password_github")

## Install git
1. [Download git](https://git-scm.com/downloads) (version depends on your OS)
2. Install downloaded file git
3. Before fork or clone repository you must set up your git. [Here](https://help.github.com/articles/set-up-git/)
   is a good explanation (Setting up Git chapter and the three steps below)

## Fork an SDF repo
1. Navigate to the [SFD repository](https://github.com/StratisDevelopmentFoundation)
2. Click repository StratisDevelopmentFoundation.github.io
3. Find *Fork* button and click it

    ![alt text](/assets/images/fork_button_github.png "fork_button_github")

   You have just forked the repository into your account

## Clone the repo locally
1. Navigate to your fork, should be like this *https://github.com/YOUR-USERNAME/StratisDevelopmentFoundation.github.io*
2. Under the repository name, click *Clone or download* and copy address (button in blue rectangle)

    ![alt text](/assets/images/clone_or_download_button_github.png "clone_or_download_button_github")
3. You can clone repository by HTTPS or SSH protocol (if you prefer second option, you must first [generate SSH keys](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/))
4. Open Git Bash
5. Move to your folder with local repository (use *cd .., cd, ls, pwd* command)
6. Use *git clone git@github.com:YOUR-USERNAME/StratisDevelopmentFoundation.github.io.git*
    ![alt text](/assets/images/git_clone.png "git_clone")

## Modify the repo locally

Before you start you must create your own local branch:

1. Open Git Bash
2. Move to folder ../StratisDevelopmentFoundation.github.io on your computer (use *cd .., cd, ls, pwd* command)
3. Use *git checkout -b "name_of_your_new_branch"* (you can check your current branch with *git branch*)

    ![alt text](/assets/images/git_checkout_b.png "git_checkout_b")

    ![alt text](/assets/images/git_branch.png "git_branch")

4. You can make changes on your repository locally right now. When you finished you can commit your changes.
   The below steps describe how

## Commit changes to your github fork
1. Open Git Bash
2. Use *git status*. You can see the result of your changes

    ![alt text](/assets/images/git_status.png "git_status")
3. Use *git add -A*

    ![alt text](/assets/images/git_add_a.png "git_add_ad")
4. Use *git commit -m "commit_name"*

    ![alt text](/assets/images/git_commit.png "git_commit")
5. Use *git push origin "name_of_your_new_branch"*

    ![alt text](/assets/images/git_push.png "git_push")

## Create a pull request to the original SDF repo
1. Enter into your account on https://github.com
2. Select repository *StratisDevelopmentFoundation.github.io*
3. Click *New pull request* button

    ![alt text](/assets/images/new_pull_request_button.png "new_pull_request_button")
4. Select your own branch for comparison with *base: master* SDF repository

    ![alt text](/assets/images/pr_comparison_branch.png "pr_comparison_branch")
5. Click *Create Pull Request* button

    ![alt text](/assets/images/create_pull_request_button.png "create_pull_request_button")


From now you contribute to Software Development Foundation project!

Article written by @LukasDev (can be contacted on our [Discord](/discord/)) Tip @LukasDev with Stratis [here](https://chainz.cryptoid.info/strat/address.dws?Sdyi3wDUV4zMvwLVU4EHby2rK93GjUNUaK)
{: .notice--info}