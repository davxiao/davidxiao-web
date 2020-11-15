---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Differences Between AWS Route 53 Failover and AWS ELB LB"
subtitle: "what you need to know"
summary: "This post provides clarity over the differences between AWS R3 failover and ELB LB."
authors:
  - david-xiao
profile: false
tags:
  - route-53
  - elb
  - failover
  - health-check
categories:
  - Cybersecurity
date: 2020-11-07
lastmod: 2020-11-07
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

## Single Region vs. Global

ELB provides single-region failover and load balancing while Route 53(R53) operates at DNS level so that can failover across regions.

## Instant Failover

ELB routes traffic off an unhealthy target (like an EC2) immediately when it is detected while R53 operates at DNS level, so when a client endpoint have cached DNS results in their DNS resolvers, it will not be effectively redirected until it passes TTL.

## AWS vs Non-AWS

ELB performs failover within the AWS target groups where only AWS resources can be registered. R53 operates at DNS level and is resource type agnostic.
