---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Explain Border Gateway Protocol in 5 Minutes"
subtitle: "In plain English"
summary: "This post attempts to explain the Border Gateway Protocol in plain English: what it is; how it works at a high level and some of the threats from a security perspective."
profile: false
authors: 
  - david-xiao
tags:
  - bgp
  - cybersecurity
categories: 
  - Information Security
date: 2020-10-30
lastmod: 2020-10-30
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

This post attempts to explain the BGP network protocol in plain English and answer those questions: 

- what is it?

- how it works at a high level?

- what are the security concerns?

## In a Nutshell

BGP is a routing method that enables the internet to function.

Today, BGP is widely used to route IP traffic between large scale inter-connected networks. For example, when routing between corporate networks or connecting on-prem data center to a cloud provider's edge location, BGP is typically considered *de facto* protocol.

Before diving into BGP, let's start with a simple definition of network routing:

> The **name** of a resource indicates what we seek;
> 
> an **address** indicates where it is; 
> 
> and a **route** tells us how to get there.
> 
> *- John F. Shoch*

## Pre-BGP Era

In the pre-BGP era when internet was much smaller, separate networks like universities usually each maintain its own static routing table - meaning the routing table on each network is manually updated by the admin and contains IP prefixes the admin is made aware of.

When the scale of internet grows, the number of inter-connected networks increases and the inter-network topology started evolving from a tree-like topology to a mesh-like toplogy to allow for redundancy and scalability.

{{< figure src="network-topology.png" title="tree-like network topology vs full-mesh network topology <br>*credits:* [Ziv Leyes](https://www.imperva.com/blog/author/ziv/)" >}}

Over time, those changes altogether made the static routing no longer a sustainable approach.

Another new type of construct that emerged is what's called Autonomous System (AS) architecture.

An AS can be an Internet Service Provider, a university or an entire corporate network, including multiple locations (IP addresses). Each AS is represented by a unique number called an ASN.

At first, AS became the new level that inter-network operates on and routes against. As the internet continues to grow, it calls for a new protocol that can dynamically exchange routes information and decides on the routes depends on many other factors.

## BGP Routing

In June 1989, the first version of a new routing protocol, known as the Border Gateway Protocol, was formalized.

BGP operates on OSI Layer 4, in other words, it understands IP address. 

BGP is designed to exchange routing and reachability information between AS on the Internet. But it can also be used on private networks. A dedicated line between on-prem data center and a public cloud service provider (in AWS, it's called [DirectConnect](https://aws.amazon.com/directconnect/faqs/)) can take advantage of BGP routing as well.

{{< figure src="bgp-routing.png" title="BGP Route Propagation between Neighboring Domains<br>*credits:* [Ziv Leyes](https://www.imperva.com/blog/author/ziv/)" >}}

Every AS in BGP has its border router that speaks the "BGP language" and connects to other AS. Sometimes the border router is also called BGP speaker.

As of August 2019 there are [over 92,000](https://en.wikipedia.org/wiki/Autonomous_system_(Internet)) assigned ASN over the internet.

## BGP operations

BGP neighbors, called peers, are established by manual configuration among routers to create a TCP session on port 179.

By design, routers running BGP accept advertised routes from other BGP routers by default. This allows for automatic and decentralized routing of traffic across the Internet.

## BGP security considerations

Unfortunately BGP is not security by design.

> Due to the extent to which BGP is embedded in the core systems of the Internet, and the number of different networks operated by many different organizations which collectively make up the Internet, correcting this vulnerability is a technically and economically challenging problem.
> 
> *- Wikipedia*

Cisco has published an article regarding protecting BGP:[Protecting Border Gateway Protocol for the Enterprise](https://tools.cisco.com/security/center/resources/protecting_border_gateway_protocol)

The main threats identified in the article above:

- BGP Route Manipulation

  A malicious device alters the contents of the BGP routing table thus prevent traffic from reaching its intended destination without acknowledgement or notification.

- BGP Route Hijacking

  A rogue BGP peer maliciously announces a victim's prefixes in an effort to reroute some or all traffic to itself for untoward purposes. For example, to view traffic that the router would otherwise not be able to read.

- BGP Denial of Service (DoS)

  A malicious host sends unexpected or undesirable BGP traffic to a victim in an attempt to expend all available BGP or compute resources results in a lack of resources for legit BGP traffic processing.

- Misconfiguration

  Inadvertent mistakes among BGP peers can also have a disruptive impact on a router's BGP process. Security controls should be applied to mitigate impacts from such kinds of incidents.

## Reference

[BGP for Humans: Making sense of Border Gateway Protocol](https://imperva.com/blog/bgp-routing-explained/), Ziv Leyes

[Internet Routing and Traffic Engineering](https://aws.amazon.com/blogs/architecture/internet-routing-and-traffic-engineering/), AWS Architecture Blog

[A Practical Guide to Troubleshooting with Traceroute](RAS_traceroute_N45.pdf)

[Border Gateway Protocol](https://en.wikipedia.org/wiki/Border_Gateway_Protocol), Wikipedia
