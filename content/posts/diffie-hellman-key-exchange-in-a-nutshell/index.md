---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Diffie-Hellman Key Exchange in a Nutshell"
subtitle: "Explain how the key exchange works in plain language"
summary: "DH key exchange is a critical component in virtually every PKI implementation. Having a working knowledge of what it is and how it works would help in understanding PKI as a whole."
profile: false
authors:
  - david-xiao
categories:
  - Encryption
  - CyberSecurity
tags:
  - diffiehellman
  - keyexchange
date: 2020-10-12
lastmod: 2020-10-12
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

{{< youtube M-0qt6tdHzk >}}

## Step by Step

1. Alice and Bob agree publicly on a prime modulus 17, and a generator 3

2. Alice selects a private random number 15
3. Alice calculates 3^15 mod 17 (three to the power fifteen mod seventeen), sends the result 6 publicly to Bob
4. Bob selects a private random number 13
5. Bob calculates 3^13 mod 17, sends the result 12 publicly to Alice
6. Alice takes Bob's public result and raise it to the power of her own private number and mod it, i.e. calculates 12^15 mod 17, the result is 10
7. Bob does the same procedure as Alice, i.e. calculates 6^13 mod 17, the result is 10 (the same)
8. An analysis of how calculation in step 6 and step 7 are done shows that either side actually did the same calculation with the exponents in a different order. In mathematics, when you flip the exponent, the result doesn't change.

{{< figure src="diffi-hellman-calc.png" title="Exponents in a different order" >}}
