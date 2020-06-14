---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Notes from My Journey in React.js"
subtitle: ""
summary: "The Good, ~~the Bad and the Ugly~~ I've learned from it."
profile: false
authors:
  - david-xiao
tags:
  - react.js
  - javascript
  - learning
categories:
  - Coding
date: 2020-06-13
lastmod: 2020-06-13
featured: false
draft: true

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false
  caption: 'Image credit: [**Unsplash**](https://unsplash.com/photos/d1eaoAabeXs)'

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

The guides I've read and followed through:

- 👉 The [step-by-step guide](https://reactjs.org/docs/hello-world.html) on reactjs.org

## Basics

- Elements are the smallest building blocks of React apps.

  An element describes what you want to see on the screen: `const element = <h1>Hello, world</h1>;`

  Unlike browser DOM elements, React elements are plain objects, and are cheap to create. React DOM takes care of updating the DOM to match the React elements.

- Components let you split the UI into independent, reusable pieces, and think about each piece in isolation.

