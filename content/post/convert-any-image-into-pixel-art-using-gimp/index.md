---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Convert Any Image Into Pixel Art Using Gimp"
subtitle: "it's quick and easy"
summary: "If you like to add some vintage like retro game feeling to your website, try use pixel art images might be a good idea. Here's a quick howto."
authors: 
  - david-xiao
profile: false
tags: 
  - pixelart
  - gimp
categories: 
  - website
date: 2020-11-09
lastmod: 2020-11-09
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

If you like to add some vintage like retro game feeling to your website, try use pixel art images might be a good idea. Here's a quick howto.

## Install GIMP

GIMP is an open source image processing program available on macOS, Linux and Windows. Visit https://www.gimp.org/ to download and install.

If you are like me who uses macOS and have the mighty [homebrew](https://brew.sh/) package manager installed, simply run:

```
brew install gimp
```

## Start Converting

Start the gimp, open your image-to-be-converted, then from the menu select `Filters/Blur/Pixelize...`

On the dialog box, you want to adjust `Block Width` and preview the results.

That's it!

{{< figure src="pixelize.png" title="Adjust the Block Width" >}}
