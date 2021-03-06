---
layout: post
title: Get started with Github
color: rgb(107,91,149)
tags: [git]
---

# Get started with Github

The github site is really nice for easy commits, there are multiple tools provided to manage that on multiple platform. ([Github Desktop](https://desktop.github.com/), [Kraken](https://support.gitkraken.com/how-to-install)).
However I wanted to document and explore the command line way of committing.

## Source

My main source will be from gitlab plus some precision over certain steps that I've encountered while doing them at a beginner level. But there's also a very good documentation made by github.

- [github help](https://help.github.com/)
- [Full documentation links](https://docs.gitlab.com/ce/README.html)
- [Start with basics](https://docs.gitlab.com/ce/gitlab-basics/README.html)
- [Workshop step by step](https://docs.gitlab.com/ce/university/training/user_training.html#committing) (my favourite)
- [Online tutorial](https://github.com/settings/keys)

## Configure your environment

Windows Os: Install [Git for Windows](https://git-for-windows.github.io) which is an emulated terminal with git.

MacOs: Type `git` in the Terminal application. If it's not installed, it will prompt you to install it.

Linux:
  - Debian: `sudo apt-get install git-all`
  - Red Hat: `sudo yum install git-all`

## Configure Git

One-time configuration of the Git client. Replace what's in `" "` by your information.

```bash
	git config --global user.name "Your Name"
	git config --global user.email you@example.com
```

All these information will be store in the git config file:

```bash
cat ~/.gitconfig

# This is Git's per-user configuration file.
[user]
	name = "Your Name"
	email = you@example.com

```
## Configure SSH Key

Type on the command prompt `cmd.exe` (on Windows, not in the git one). 

```bash
	ssh-keygen -t rsa -b 4096 -C "you@computer-name"
```

You will be prompted for the following information. Press enter to accept the defaults. Defaults appear in parentheses.

```bash
    Generating public/private rsa key pair.
    Enter file in which to save the key (/Users/you/.ssh/id_rsa):
    Enter passphrase (empty for no passphrase):
    Enter same passphrase again:
    Your identification has been saved in /Users/you/.ssh/id_rsa.
    Your public key has been saved in /Users/you/.ssh/id_rsa.pub.
    The key fingerprint is:
    39:fc:ce:94:f4:09:13:95:64:9a:65:c1:de:05:4d:01 you@computer-name
```    

Then find your rsa key in the repository (by default, from the command prompt on windows):

```bash
    cat ~/.ssh/id_rsa.pub
```

Copy it in the right place on [Github]((https://github.com/settings/keys)) or [Gitlab]((https://gitlab.com/profile/keys)) or via command line (for windows, on the git one, to copy something into the git bash on Windows use `insert`):

	ssh-rsa AAAAB3NzaC1yc2EAAAADAQEL17Ufacg8cDhlQMS5NhV8z3GHZdhCrZbl4gz you@example.com
