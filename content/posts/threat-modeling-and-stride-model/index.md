---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Threat Modeling and STRIDE Model"
subtitle: ""
summary: "This post is my collection of articles related to threat modeling and Microsoft STRIDE threat model."
authors:
  - david-xiao
tags:
  - cybersecurity
  - threat-modeling
  - stride
categories:
  - Threat Modeling
date: 2020-09-09
lastmod: 2020-09-09
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

TLDR: This post is my collection of articles related to threat modeling and Microsoft STRIDE threat model.

## What Is Threat Modeling

At a high level, threat modeling is a process of putting the "bad guy" hat on and conducting an security assessment over a system (such as a website or a mobile app) to identify and prioritize threats and mitigations.

A more complete definition can be found on [Wikipedia](https://en.wikipedia.org/wiki/Threat_model).

There are many ways to do threat modeling. Depending on the types of system and workloads that are in scope, the applicable threats can vary a lot.

For example, threat modeling over a set of highly scalable workloads deployed on MS Azure might start from a threat library that includes portential threats relevant to Azure services in use, while assessing over a simple web application hosted on on-prem data center might start from a different set of threats relevant to host based security and OWASP Top 10.

## What Is STRIDE

STRIDE is both a threat model framework and a methodology that developed and adopted in Microsoft over the years.

STRIDE stands for:

- **Spoofing**: Impersonating something or someone else.

- **Tampering**: Modifying data or code.
- **Repudiation**: Claiming to have not performed an action.
- **Information Disclosure**: Exposing information to someone not authorized to see it.
- **Denial of Service**: Deny or degrade service to users.
- **Elevation of Privilege**: Gain capabilities without proper authorization.

Microsoft suggests the following approach when conducting a threat modeling:

{{< figure src="https://docs.microsoft.com/en-us/azure/security/develop/media/threat-modeling-tool-getting-started/sdlapproach.png" title="Threat Modeling Process" >}}

## Most Current Articles

[Threat Modeling](https://www.microsoft.com/en-us/securityengineering/sdl/threatmodeling). A high level overview.

[Azure Threat Modeling Tool](https://docs.microsoft.com/en-us/azure/security/develop/threat-modeling-tool), the framework and the tool. 02/16/2017

## Older References

[STRIDE chart](https://www.microsoft.com/security/blog/2007/09/11/stride-chart/) 09/11/2007

[Threat Modeling, once again](https://docs.microsoft.com/en-us/archive/blogs/larryosterman/threat-modeling-once-again) 08/30/2007

[Threat Modeling again. Drawing the diagram](https://docs.microsoft.com/en-us/archive/blogs/larryosterman/threat-modeling-again-drawing-the-diagram) 08/31/2007

[Threat Modeling Again, STRIDE](https://docs.microsoft.com/en-us/archive/blogs/larryosterman/threat-modeling-again-stride) 09/04/2007

[Threat Modeling Again, STRIDE Mitigations](https://docs.microsoft.com/en-us/archive/blogs/larryosterman/threat-modeling-again-stride-mitigations) 09/05/2007

[Threat Modeling Again, What does STRIDE have to do with threat modeling](https://docs.microsoft.com/en-us/archive/blogs/larryosterman/threat-modeling-again-what-does-stride-have-to-do-with-threat-modeling) 09/07/2007

[Threat Modeling Again, STRIDE per Element](https://docs.microsoft.com/en-us/archive/blogs/larryosterman/threat-modeling-again-stride-per-element) 09/10/2007

[Threat Modeling Again, Threat Modeling PlaySound](https://docs.microsoft.com/en-us/archive/blogs/larryosterman/threat-modeling-again-threat-modeling-playsound) 09/11/2007
