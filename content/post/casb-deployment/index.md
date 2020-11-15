---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "CASB Deployment"
subtitle: "common deployment architectures and use cases"
summary: "Cloud Access Security Broker (CASB) is considered a common solution to mitigate \"shadow IT\" and data exfiltration risks on many organization's journey to cloud."
profile: false
authors: 
  - david-xiao
tags:
  - casb
  - cybersecurity
categories: 
  - Information Security
date: 2020-11-01
lastmod: 2020-11-01
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

Cloud Access Security Broker (CASB) is considered a common solution to mitigate [shadow IT](https://en.wikipedia.org/wiki/Shadow_IT) and data exfiltration risks on many organization's migrating to cloud journey.

This post discussed some common deployment architecture and use cases respectively.

## CASB and Context Aware Access

CASB can mitigate data exfiltration risks from insider - it can correlate the download of sensitive data from a managed cloud service with the upload of that data to a personal cloud service.

Context Aware Access is focusing on mitigating authentication and authorization risks and promoting a "Zero Trust" access model in VPN-less settings.

## Reverse proxy

A reverse proxy deployment steers browser-based cloud traffic from managed cloud apps to the CASB Cloud.

{{< figure src="casb-reverse-proxy.png" title="Reverse Proxy Deployment" width="500px" >}}

A CASB in reverse proxy mode proxies all traffic to and from cloud apps. Unlike a forward proxy, the endpoint or network does not need to be managed. 

A common approach is to leverage the organization's identity solution (IdM), for example [Okta](https://www.okta.com/partners/netskope/), to route traffic through the CASB reverse proxy following authentication. In this way, all traffic bound for a cloud service is pervasively steered over to the CASB proxy.

Reverse proxy is essentially the only deployment architecture that supports both managed and unmanaged devices accessing cloud apps.

**Challenges**

It supports web browser traffic only – native apps or sync clients may have non-web authentication methods. (e.g. JWT or other forms of API token)

## Forward proxy

CASB forwarder is deployed on-premises as a VM that steers local cloud and web traffic to the CASB Cloud.

{{< figure src="casb-forward-proxy.png" title="Forward Proxy Deployment" width="500px" >}}

There are two approaches:

If the organization has an existing secure web gateway, a common practice is to configure proxy chaining to the upstream CASB forward proxy.

If no secure web gateway exists, an endpoint agent can be deployed to route cloud traffic through the forward proxy.

**Challenges**

It does not work well for either unmanaged devices or users that are accessing cloud over non-corporate network, e.g. at home.

## API

Use API connectors to connect CASB to cloud apps like Office 365, Box, Salesforce that offer APIs to support visibility and policy enforcement.

{{< figure src="casb-api.png" title="API Deployment Architecture" width="500px" >}}

Typical APIs such as audit trails of user activity, content inspection, user permissions on storage and security settings can be leveraged by CASB to enable visibility and policy enforcement.

**Challenges**

- Does not discover "Shadow IT".

- CASB vendor may not support Cloud apps that end user currently uses or choose to use in the future.

- API based capabilities vary for each Cloud app.

## Log collection

CASB can be configured to parse log traffic from a perimeter device. This provides cloud service discovery capabilities. Logs can be uploaded directly to the CASB Cloud or an on-premises log parser can be deployed to continuously send log data to the CASB.

{{< figure src="casb-log-collection.png" title="Log Collection Deployment" width="500px" >}}

**Challenges**

Logs may not be support off-the-shelf, customization may be required.

## Reference

[Wikipedia: CASB](https://en.wikipedia.org/wiki/Cloud_access_security_broker)

[CASB Deployment Options (Netskope)](https://www.netskope.com/products/deployment-options)
