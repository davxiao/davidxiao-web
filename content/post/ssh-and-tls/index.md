---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "SSH and TLS: Differences and Similarities"
subtitle: ""
summary: "Review the differences and similarities between the two protocols from an architecture and security perspective."
authors:
  - david-xiao
tags:
  - tls
  - ssh
categories:
  - Cybersecurity
date: 2020-09-01
lastmod: 2020-09-01
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

Both TLS and SSH are security protocols aimed to solve a specific set of problems.

TLS is the transport layer of HTTPS protocol while SSH is designed to replace plaintext Telnet protocol.

Architecture wise, TLS is relatively simple: It has a handshake protocol that does the authentication and agrees on a session key that will be used to encrypt the rest of the communication.

SSH is more [complicated](https://en.wikipedia.org/wiki/Secure_Shell#Architecture) than TLS. It has the following main components:

- transport layer;

- user authentication layer;
- connection layer;

Six SSH related RFC are published in relate to SSH: 4251, 4252, 4253, 4254, 425 and 4256.

## SSH Transport Layer

Transport layer handles key exchange, server authentication and sets up encryption, compression and integrity verification. It exposes to the upper layer an programmatic interface for sending and receiving plaintext data. The transport layer also arranges for key re-exchange, usually after 1 GB of data has been transferred or after 1 hour has passed, whichever occurs first.

## User Authentication Layer

It handles client authentication and provides a number of authentication methods. Widely used user-authentication methods include password, publickey, keyboard-interactive, GSSAPI authentication which allows SSH to authenticate using external mechanisms such as Kerberos 5 or NTLM, providing single sign-on capability to SSH sessions.

## Connection Layer

It defines the concept of channels in SSH. A single SSH connection can host multiple channels simultaneously, each transferring data in both directions. Standard channel types include: shell for terminal shells; SFTP and exec requests (including SCP transfers); direct-tcpip for client-to-server forwarded connections; forwarded-tcpip for server-to-client forwarded connections etc.
