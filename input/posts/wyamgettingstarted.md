Title: Wyam Blog Getting Started(cheetsheet)
Published: 11/25/2019
Tags:
    - wyam
    - Introduction
    - Basics
---


Installing
============
[Github link](https://github.com/Wyamio/Wyam) For Wyam Source

[Wyam Docs](https://wyam.io/docs/usage/obtaining) 

The easiest way to install Wyam is via the global tool package named Wyam.Tool. You can install it like this

```shell
dotnet tool install -g Wyam.Tool
```

Scaffolding Blog Recipe
========================
[Docs](https://wyam.io/recipes/blog/overview)

```shell
wyam new -r Blog
```

Building
=========
```shell
wyam -r Blog
```
To select a theme
```shell
wyam -r Blog -t CleanBlog
```

AppVeyor for Continuous Integration
=====================================
[link here](https://wyam.io/docs/deployment/appveyor)

Deploy To Github Pages
========================
[link](https://help.github.com/en/github/working-with-github-pages/creating-a-github-pages-site)

After configuring add new .md page in /input/post and commit project will get build and publish to github pages