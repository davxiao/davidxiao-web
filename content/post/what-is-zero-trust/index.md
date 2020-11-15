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

Zero Trust is a security model where app components and microservices are considered discrete from one another  - no component or microservice trusts any other by default.

Zero Trust by design does not trust the underlying internal network fabric, and extends it to things such as input and output validation at app and microservice level. For example, it mandates that input from any source is not trusted unless it can be verified otherwise.

Additional efforts can include designing a defense-in-depth approach to protect against individual components, microservices, or identities compromise.

While conventional network security seek to build a secure perimeter – everything within the perimeter is trusted and anything outside the perimeter is not, a Zero Trust system evaluates actions to reduce the risk of unauthorized access to data and resources.

## Zero Trust Principles

- Never trust, always verify

- Connecting from a particular network must not determine which services user can access
- Access to services is granted based on what we know about the user and the device
- All access to services must be authenticated, authorized, and encrypted

## Security Controls in Enforcing a Zero Trust Model

Verifying identity

One components of a Zero Trust system is the ability to verify a user’s identity before access is granted to the corporate network.

Verifying devices

Unmanaged devices are an easy entry point for bad actors, ensuring that only healthy devices can access critical applications and data is vital for enterprise security.

Verifying access

Some scenarios require users to work from unmanaged devices. With those situations in mind, it transitioned from a corporate network approach to internet-first access methods, with a final goal of internet-only access methods in sight. 

This strategy reduces users accessing the corporate network for most scenarios, and will establish a set of managed virtualized services that make applications and full functional desktop environments available to users with unmanaged devices.

## Emerging Technologies

Context-Aware Access

[To be continued]

## Reference

#### [How to think about Zero Trust architectures on AWS](https://aws.amazon.com/blogs/publicsector/how-to-think-about-zero-trust-architectures-on-aws/)

> Zero Trust goes beyond building a network boundary between each microservice in your architecture. Beyond strengthening your component perimeters; rethink threat sources and investment to protect against them. 
> 
> Zero Trust models are not always appropriate. It offers some security benefits, but it also introduces cost, complexity, and operational overhead for maintaining the overall system. 
> 
> When considering Zero Trust architecture, evaluate all five pillars of the AWS [Well-Architected framework](https://aws.amazon.com/architecture/well-architected/) to properly balance your needs.

#### [BeyondCorp at Google](https://cloud.google.com/beyondcorp/)

> BeyondCorp is Google's implementation of the zero trust security model.

#### [Transitioning to modern access architecture with Zero Trust](https://www.microsoft.com/en-us/itshowcase/transitioning-to-modern-access-architecture-with-zero-trust)

> Microsoft's view on "Zero Trust" is based on the principle: 
> 
> **never trust, always verify.**
> 
> It protects organizations by managing and granting access based on the continual verification of identities, devices and services.
