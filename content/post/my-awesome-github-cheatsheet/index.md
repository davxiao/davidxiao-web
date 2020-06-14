---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "My Awesome GitHub CheatSheet"
subtitle: ""
summary: "An awesome list of useful git slash github commands I compiled over time."
profile: false
authors:
  - david-xiao
tags:
  - github
  - git
  - cheatsheet
categories:
  - Coding
date: 2020-06-12
lastmod: 2020-06-12
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false
  caption: 'Image credit: [**Unsplash**](https://unsplash.com/photos/842ofHC6MaI)'

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

## Onboarding an existing project to GitHub

If the project is not yet initialized by git, do the following first

```bash
git init ;
# then create a .gitignore file if needed
git add . ;
git status -u ; # will show you all the files to be committed.
git commit -m "init commit" ;
```

Then connect it to GitHub

```shell
git remote add origin git@github.com:davxiao/my-proj.git ;
git branch --set-upstream-to=origin/master master ;
git pull origin master --allow-unrelated-histories ;
git push ;
```
