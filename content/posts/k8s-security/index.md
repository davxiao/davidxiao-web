---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "k8s Security"
subtitle: ""
summary: ""
authors: []
tags: []
categories: []
date: 2020-10-31T23:04:50-04:00
lastmod: 2020-10-31T23:04:50-04:00
featured: false
draft: false
profile: false
resources:
- src: file/cis-k8s-v161.pdf
  title: cis-k8s-v161
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

Over the past few months I've collected a few good resources regarding Kubernetes security.

I will add more as I learn.

## Reference

#### [Securing a Cluster](https://kubernetes.io/docs/tasks/administer-cluster/securing-a-cluster/)

This document covers topics related to protecting a cluster from accidental or malicious access and provides recommendations on overall security.

#### [CIS Kubernetes Benchmark version 1.6.1](file/cis-k8s-v161.pdf)

Released in October 2020, it provides prescriptive guidance for establishing a secure configuration posture for Kubernetes.

#### [Kubernetes security best practices](https://blog.sqreen.com/kubernetes-security-best-practices/)

It covers a few suggestions on what can you do to make your Kubernetes workloads more secure.

 - Disable public access

 - Implement role-based access control
 - Encrypt secrets at rest
 - Configure admission controllers
 - Implement networking policies
 - Configure secure context for containers
 - Segregate sensitive workloads
 - Scan container images
 - Enable audit logging
 - Keep your Kubernetes version up to date

#### [Kubernetes Security Best Practices](https://logz.io/blog/kubernetes-security/)

It discusses the special security concerns arising in Kubernetes environments, and best practices in properly setting up the k8s environment to mitigate vulnerabilities:

- work with namespaces for authentication, authorization and access control

- working with reliable docker images and updating relevant software
- defining resource quotas to avoid resource cannibalization
-setting up network policies for proper segmentation and traffic control
