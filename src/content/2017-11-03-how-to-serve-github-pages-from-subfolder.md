---
category: tutorial
date: 2017-11-03 18:30:00 +1100
description: Learn how to convert your master branch into a subtree
layout: article.html
title: How to serve Github Pages from a subfolder
thumb: github-pages-thumb.jpg
---

## Introduction
I recently moved my personal website to [Github Pages](https://pages.github.com/) 
which was super simple with the exception of one hiccup. I wanted to be able to
serve my website from a subfolder which requires a subtree, but personal github 
pages only allow your website to be served from the master branch.

After a little experimenting I found that I was able to convert my master branch 
to be a subtree which only contained the files I wanted to serve. Below is 
a step by step process that you can follow to do the same. And if you need an 
example project you can just [use my website](https://github.com/stwilz/stwilz.github.io/)! :)

## How to turn master into a subtree
1. Clone your repo
    I know, der, but whatever. You're obviously going to need to pull down your repo to get started.
    ```
        git clone https://github.com/{repo}/{repo}.github.io.git
    ```
2. Create a develop branch
    Next, we are going to need a new branch to develop or code on. 
    ```
        git checkout -b develop
    ```
3. Set develop to be your primary branch

4. Delete the repositories master branch
    Don't worry, we will make a new one. But for now the master branch must die.
    ```
        git push origin --delete master
    ```
5. Build out your production files
    You assumably have your own build process so run your requires metalsmith/gulp/grunt/webpack
    command and build your static files out to whatever directory you want to use.

6. Commit your production files to master as a subtree
    Now stage, commit and push your files. Below I use the `dist` directory so if you are not using
    that naming convention you will need to update the command accordingly.
    ```
    git stage -A
    git commit -m "compiled files for release 1.0.0
    git subtree push --prefix dist origin master
    ```

## That's it!
If you are seeing an error at first don't freak out. Github pages sometimes does take a little while
to update.
