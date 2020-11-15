---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Differences between On-Prem Networking and Cloud Networking"
subtitle: "from a security, architecture and operational perspective"
summary: "This post discusses some of the major differences between on-prem data center networking and cloud networking."
authors:
  - david-xiao
profile: false
tags:
  - networking
  - cloud-networking
  - onprem-networking
categories:
  - Cybersecurity
date: 2020-11-06
lastmod: 2020-11-06
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

## Multiple Availablity Zone and Cross Region

When it comes to high availability and scalability, cloud networking has an edge over data center networking.

Multiple Region and Availablity Zone and peered VPCs can be provisioned on-demand almost instantly on cloud while setting up a conventional data center could take weeks if not months.

## Full Control over the Network Infrastructure

Cloud networking operates on the "Shared Responsibility Model" which means CSP manages the network infrastructure, things like VLAN tags are out of customer's control.

Certain networking capabilities such as multicast routing is also not supported by every cloud provider. It is changing though, at the time of writing, AWS just announced support for [multicast on transit gateways](https://docs.aws.amazon.com/vpc/latest/tgw/working-with-multicast.html).

## Single Tenant vs Multi-Tenant

Cloud network such as VPC typically runs on infrastructure that is shared with other customers while on-premises data center usually is owned by the organization.

## API and Compatibility

Cloud networking provides APIs that is an integral part of the Cloud service. For example, AWS VPC provides API that are integrated with EC2.

On-premises data centers use technologies of their choice. Some such as Cisco ACI and Nutanix provide their own set of APIs.

## Network Latency

On-premises networks can provide very low network latency. It's not hard to find port to port nano second switches on the market. For example, Cisco provides switches that have 39ns port-to-port latency.

CSPs like AWS are[catching up](https://aws.amazon.com/blogs/compute/low-latency-computing-with-aws-local-zones-part-1/) on this front but is nowhere near as good at the moment.

## Legacy Systems and BYOD

Legacy systems that are installed on the data center rely on specific HSM modules could be part of a mission critical system.

When planning on migrating such systems to cloud, "lift and shift" strategy don't work because most cloud service providers do not allow BYOD.

## IP Address Allocation

Cloud networking usually reserve a few IPs on each subnet for the cloud infrastructure. 

For example, AWS reserves first four IP addresses and the last IP address in each subnet CIDR block. See [detail](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html#vpc-sizing-ipv4)
