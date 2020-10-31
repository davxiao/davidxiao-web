---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "What Is Zero Trust"
subtitle: "why it's emerging in security"
summary: "This is the first post of a series that attempts to discuss Zero Trust in security from a conceptual and implementation perspective."
profile: false
authors: 
  - david-xiao
tags:
  - zerotrust
  - cybersecurity
categories: 
  - Information Security
date: 2020-10-31
lastmod: 2020-10-31
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

TL;DR

This is the first post of a series that attempts to discuss Zero Trust in security from a conceptual and implementation perspective.

## What is Zero Trust

Zero Trust security is a model where app components and microservices are considered discrete from one another  - meaning no component or microservice trusts any other. It also mandates that input from any source are malicious until validated otherwise. 

Zero Trust starts with not trusting the underlying internal network fabric, and extending to things such as input and output validation at app and microservice level. 

Additional efforts can include designing a defense-in-depth approach to protect against individual components, microservices, or identities compromise.

Conventional network security seek to build a secure perimeter – everything within the perimeter is trusted and anything outside the perimeter is not. 

By contrast, a Zero Trust system evaluates actions to reduce the risk of unauthorized access to data and resources.

## Emerging Technologies

Context-Aware Access

[To be continued]

## Reference

[How to think about Zero Trust architectures on AWS](https://aws.amazon.com/blogs/publicsector/how-to-think-about-zero-trust-architectures-on-aws/), AWS Public Sector Blog
