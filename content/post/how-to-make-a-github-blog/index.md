---
# Name of the article
title: "How to Make a Github Blog"

# Quick description
description: Not so hard as you think, but takes some time to play with it

# Author of the article
author: Master Pi

# Date created
date: 2021-09-10

# Article's tags
tags: 
    - hugo
    - blog
    - github

# Article's categories: Blog, Project or Guideline
categories:
    - Guideline

# Allow share?
socialShare: true

# Useful to link articles together for "See also" part
series: 

# is Math included? Default: false
math: false

# Cover image of the article
image: cover.png

# Appears as the tail of the output URL.
slug: "how-to-make-a-github-blog"
---

Hi, this is my way to make a Github blog for my self, which will contain blogs, some projects, and some tips from my experience. I am a webasite newbie, so it took me almost 2 days to setup from start to finish, and it's pretty easy. Here is how I made it.

# Initial setup
## Create repositories
First, let take a look at what a [Github Page](https://pages.github.com/) is, and how to create one. You should be able to create a repository for your Github Page named `username.github.io`, where username is your Github username.

The repository above will store the content of your website, but we will create another repository where the website development/authoring will take place, for example, `Personal-Blog`.   

## Install Hugo

There are multiple ways to install **Hugo**:
- Install from binary releases on [Github](https://github.com/gohugoio/hugo/releases). I reccommend installing the **extended** version, because some themes will run only on this version.
- On a Mac with `Homebrew` installed, run `brew install hugo`
- On Windows with `Chocolatey`: `choco install hugo[-extended] -confirm`

# Create a site
## Initialize a new site
Let's clone 2 repositories we created above to start:
```
git clone https://github.com/username/Personal-Blog.git
```
Replace `username` with your *username* and `Personal-Blog` with your *repository's name*.

Goto the repository's root directory, and start with `hugo new site .`with `.` will tell Hugo to create the site in the current directory.

You will see the folders and files like in this structure:
```
site_root
    |-- archetypes
    |-- assets
    |-- content
    |-- data
    |-- layouts
    |-- static
    |-- themes  
    |-- config.toml
```
The detail explaination for each folder is published on [Hugo documentation](https://gohugo.io/getting-started/directory-structure/), and it is necessary to understand it before continue.

## Install theme
Next, we wll pick a theme at [Hugo theme website](https://themes.gohugo.io/) and install in `themes` folder. I will choose **Hugo Stack Theme** for the demonstration.

Copy the theme's github path, and install:
```
git submodule add https://github.com/CaiJimmy/hugo-theme-stack themes/hugo-theme-stack
```
Go to the installed theme directory, you will see the `exampleSite` folder. Copy its content and paste into `content` folder. It stores all page and post we create for the website, so we will use it to test the new site.

Replace `config.toml` by `config.yaml` copied above, and modify this file as in [this intruction](https://docs.stack.jimmycai.com/).

## Publish to Github Page
After you add a post or a site, it's time to push into the repository and publish. But first, we have to link the Github page repository to the current development repository:
```
git submodule add -b master https://github.com/username/username.github.io.git public
```

It will clone the repository into `public` folder, as in Hugo documentation about directory structure.
Check again with `git remote -v`:
```
origin	https://github.com/username/username.github.io.git (fetch)
origin	https://github.com/username/username.github.io.git (push)
```
Then publish the site by running `./deploy.sh`:
```
#!/bin/sh

# If a command fails then the deploy stops
set -e

printf "\033[0;32mDeploying updates to GitHub...\033[0m\n"

# Create commit message
msg="rebuilding site $(date)"
if [ -n "$*" ]; then
	msg="$*"
fi

# Build the project.
echo ""
echo ""
echo "Committing changes to $(pwd)"
hugo -D

# Go To Public folder
cd public

# Add 'public' (Github Pages repo) changes to git and commit/push.
echo ""
echo ""
echo "Committing changes to $(pwd)"
git add .
git commit -m "$msg"
git push origin master

# Add this repos changes to git and commit/push. First 'cd' out of public
cd ..
echo ""
echo ""
echo "Committing changes to $(pwd)"
git add .
git commit -m "$msg"
git push origin master
```
# Modification
You can also modify the theme to suit your need. All documentations is on these websites:
- https://docs.stack.jimmycai.com/
- https://gohugo.io/documentation/

Here is my work:
- [The blog sourece code](https://github.com/MasterPi-2124/Personal-Blog)
- [The Github Page repository](https://github.com/MasterPi-2124/MasterPi-2124.github.io)