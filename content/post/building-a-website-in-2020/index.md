---
title: 'Building a website in 2020'
subtitle: 'with Hugo and Google Firebase'
summary: Create a static website with Hugo from the ground up  
date: 2020-04-16
authors:
  - david-xiao
draft: false

# Featured image (optional)
# To use, add an image named either `featured.jpg` or `featured.png` to your page's folder.
# Placement options: 1 = Full column width, 2 = Out-set, 3 = Screen-width
# Focal point options: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight
image:
  placement: 3
  caption: 'Image credit: [**Unsplash**](https://unsplash.com/photos/icErAzyU3fY)'
  focal_point: "Smart"
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

This is my first blog post, and I decided to write about how I built this website from the ground up. I wish you'll find it helpful.

# Is this for you or not

Web development has come a long way. Back in the late 90's when I first came across Internet, anyone who knows anything about [HTML](https://en.wikipedia.org/wiki/HTML) would be considered very technical.

In 2020, social media and smart phone is anywhere and everywhere, [web hosting services](https://en.wikipedia.org/wiki/Web_hosting_service) such as [Wix](https://www.wix.com/) and [Wordpress](https://wordpress.com/) have made [content creation](https://en.wikipedia.org/wiki/Content_creation) possible and accessible for anyone who wishes to create a website without heavylifting.

Then you may wonder, why would anyone care about building a website from the ground up?

Well, it is not for everyone.

But if you are like me who likes to go beyond content creation, who likes to give in your time and take a deep dive to learn more about building a website that is lightweight and secure, cost effective (totally free except the domain name annual fees) yet no [vendor lock-in](https://en.wikipedia.org/wiki/Vendor_lock-in), perhaps this post is for you.

# Let's dive right in

First and foremost, let's take a look on what I wanted to cover in this post.

- Register a domain name.
  
- Set up [Hugo](https://gohugo.io/) as static site generator. I picked Hugo for it's a relatively mature project supported by an active community.
- Use a kick-starter theme to get you going. There are many themes available on Hugo, In this post I will use Academic. It also comes with a [kick-start](https://github.com/sourcethemes/academic-kickstart/) on github.
- A hosting provider. There are many options there, [Google Firebase](https://http://firebase.google.com/), [Netlify](https://www.netlify.com/) and [AWS Amplify](http://aws.amazon.com/amplify/) to name a few. Each one has its own offering. I picked Firebase only because they seem to offer a bit more on their free tier.

## Register a domain name

It's pretty straightforward. Come up with a domain name for your site, such as `davidxiao.me`. You want to be creative <3. The name is better to be concise and easy to remember. 

Complete the domain registration on any [Domain Name Registrar](https://en.wikipedia.org/wiki/Domain_name_registrar) you prefer. I use [Google Domains](https://domains.google/) but there are other good choices such as [Go Daddy](https://godaddy.com/).

{{< figure src="googledomain.png" title="Register a domain name on Google Domains" >}}

## Setting up Hugo

Hugo is a static site generator. In a nutshell, Hugo renders your content into HTML files and helps deploying those HTML files to your choice of hosting provider.

A few words about content. It is what you write as content creator. Hugo takes content written in [Markdown](https://en.wikipedia.org/wiki/Markdown) (`.md`), a format that is designed for content creation and intended to be used by technical and non-technical writers alike. Since inception, Markdown has become the 'de facto' format in blogging. There are many good guides on Markdown syntax, e.g. [Markdown Guide](https://www.markdownguide.org/). I won't go further into Markdown detail in this post.

### Hugo installation

On macOS, I recommend using a package manager such as [`Homebrew`](https://brew.sh/) to manage third-party packages. With Homebrew installed, run:

    $ brew install hugo

and you are all set. In case you need to check which Hugo version is installed, run `brew version`. On my mac it is `Hugo Static Site Generator v0.70.0/extended darwin/amd64`. For installing Hugo on Windows or Linux, refer to Hugo's documentation.
