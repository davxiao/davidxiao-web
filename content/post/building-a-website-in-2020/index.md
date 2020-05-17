---
title: 'Building a website in 2020'
subtitle: 'with Hugo and Google Firebase'
summary: Create a static website with Hugo from the ground up  
date: 2020-04-16
lastMod: 
authors:
  - david-xiao
categories: []
tags: []
featured: false
draft: false

# Featured image (optional)
# To use, add an image named either `featured.jpg` or `featured.png` to your page's folder.
# Placement options: 1 = Full column width, 2 = Out-set, 3 = Screen-width
# Focal point options: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight
image:
  placement: 3
  caption: 'Image credit: [**Unsplash**](https://unsplash.com/photos/ukzHlkoz1IE)'
  focal_point: "Smart"
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

In this post I will talk about how I built this website from the ground up using Hugo as site generator and Firebase as hosting provider. I wish you'll find it helpful.

---

{{< my-dropcap Web >}} development has come a long way. Back in the late 90's when I first came across Internet, anyone who knows anything about [HTML](https://en.wikipedia.org/wiki/HTML) would be considered very technical.

Fast forward into 2020, social media and smart phone is ubiquitous, [web hosting services](https://en.wikipedia.org/wiki/Web_hosting_service) such as [Wix](https://www.wix.com/) and [Wordpress](https://wordpress.com/) have made [content creation](https://en.wikipedia.org/wiki/Content_creation) possible for anyone who wishes to create a website without much headache.

Why should you building a website from the ground up as opposed to using a web hosting service?

## Maybe it's for you

Building a website with a static site generator is not for everyone. George Cushen who is Hugo theme Academic's main contributor once put:

> *...(it would) require a basic understanding of using the command line in the Terminal (Mac/Linux) or Command Prompt (Windows) app on your computer. If you are not interested in this, perhaps this is not for you...*
>
> [Original post](https://georgecushen.com/create-your-website-with-hugo/)

But if going beyond content creation and learning more about building a website that is lightweight and secure, cost effective yet no [vendor lock-in](https://en.wikipedia.org/wiki/Vendor_lock-in) is your thing, then let's dive right in.

## Overview

First and foremost, let's take a look on what will be covered in the post.

1. Register a domain name.
  
2. Set up Hugo as static site generator.
3. Use a kick-starter theme to get going quickly.
4. Deploy the site to Firebase.
5. Add your domain to the new site.

## 1. Register a domain name

It's pretty straightforward. You come up with a great domain name. It is better to be concise and easy to remember. Be creative <3.

Then complete the domain registration on any [Domain Name Registrar](https://en.wikipedia.org/wiki/Domain_name_registrar) you prefer. I use [Google Domains](https://domains.google/) but there are other good choices such as [Namecheap](https://namecheap.com/) and [Go Daddy](https://godaddy.com/).

{{< figure src="googledomain.png" title="Register a domain name on Google Domains" >}}

## 2. Setting up Hugo

[Hugo](https://gohugo.io/) is a [static site generator](https://en.wikipedia.org/wiki/Web_template_system#Static_site_generators). There are many other site generators, I picked Hugo for a few reasons:

- It's [open source](https://en.wikipedia.org/wiki/Open_source) and backed by an active developer team and support community.
- It's a monolithic program with no external dependencies.
- It's production ready.

In a nutshell, Hugo renders content into HTML files and uploads the files onto your choice of hosting provider. Your content is what you write as content creator. Hugo takes content files written in [Markdown](https://en.wikipedia.org/wiki/Markdown) (`.md`), a format that is intended to be used by technical and non-technical writers alike. Since inception, Markdown has become the *de facto* format in content creation and blogging.

If you need to learn about Markdown syntax, there are good guides such as [Markdown Guide](https://www.markdownguide.org/).

### Installing Hugo

On macOS, I recommend using a package manager such as [Homebrew](https://brew.sh/) to manage third-party packages. With Homebrew installed, to install Hugo, just run:

    $ brew install hugo ;

All set. In case you need to check which Hugo version is installed, run `hugo version`. On my mac it returns `Hugo Static Site Generator v0.70.0/extended darwin/amd64`

For installing Hugo on Windows or Linux, refer to Hugo's documentation.

## 3. Use a kick-starter theme

Hugo has built-in theme mechanism that allows developers to quickly run a theme and see the results. It also provides all the necessary building blocks for user to personalize the theme. There are many themes available on Hugo, for my own website I use Academic Theme. It also comes with a [academic-kickstart repo](https://github.com/sourcethemes/academic-kickstart/) on github for teasers.

The easy way to get started is to just fork the repo, download the code and run it.

{{< figure src="forkrepo.png" title="Fork the kickstart into your own repo" >}}

Download the code:

    $ git clone <replace-it-with-your-own-repo-url> ;
    $ cd <your-repo-root-dir> ;
    $ git submodule update --init --recursive ; # get the latest Academic theme

Run Hugo to serve the test site:

    $ hugo server -D ;

Now visit [http://127.0.0.1:1313/](http://127.0.0.1:1313/) on your web browser and you should see the homepage.

Congrats! You've got your first Hugo website up and running on your local environment!

{{% alert note %}}
Hugo only binds to local network address for [security by default](https://en.wikipedia.org/wiki/Secure_by_default). If you need to test the site on another computer in your local network, run:

    $ hugo server -D --bind=0.0.0.0 ;

{{% /alert %}}

## 4. Deploy the new site to Firebase

There are many out there: [Google Firebase](https://http://firebase.google.com/), [GitHub Pages](https://pages.github.com/), [Netlify](https://www.netlify.com/) and [AWS Amplify](http://aws.amazon.com/amplify/) to name a few. Each one has its own offering. I picked Firebase as my hosting provider because they seem to offer a bit more on their free tier.

First, install Firebase CLI and (optional) Google Cloud SDK CLI.

- Firebase CLI. The recommended way is to run `npm i -g firebase-tools ;` See its [github repo](https://github.com/firebase/firebase-tools) for more detail. If you don't have `npm` installed yet, run: `brew install node ;`. npm will be installed alongside node.js.

- Google Cloud SDK CLI. Run `brew cask install google-cloud-sdk ;`

Next, go to [Firebase](https://firebase.google.com/) to set up an account and create a new Firebase project. Make sure it uses the default free tier plan which is called `Spark`. Be noted you need to specify GCP resource location under **Project Overview** in Firebase Dashboard after project is created. The location can not be changed afterwards, so choose something close to you would be wise.

{{< figure src="gcplocation.png" title="Specify resource location under Project Overview in Firebase Dashboard" >}}

### Set up service account authenication on Firebase

Authenticating with a service account allows you to use Firebase CLI to manage your Firebase project. Google has provided a step by step guide [here](https://firebase.google.com/docs/app-distribution/authenticate-service-account.md).

When authentication is set up, go to your project root directory and follow the recorded screens below to initialize firebase and deploy the very first version of your site onto firebase.

{{< asciinema-player id="asciicast-331048" src="https://asciinema.org/a/331048.js" >}}

Congratulations! Your website is online! You should find your Hosting URL at the end of the Firebase deploy output, it's typically something like: `https://your-project-id.web.app`

## 5. Add your domain to the new site

Go to **Hosting** on Firebase, click on "Add custom domain". Typically you wanted to add your root domain name and a sub domain name such as "www". For example, I added "davidxiao.me" for my website and added another entry for redirecting `www.davidxiao.me` to `davidxiao.me`

When it's complete, you will be able to visit your website by your custom domain regsitered on step 1.

{{< figure src="customdomain.png" title="Add custom domain to your website" >}}
