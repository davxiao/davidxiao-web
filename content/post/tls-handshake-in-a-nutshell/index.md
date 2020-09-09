---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "TLS Handshake in a Nutshell"
subtitle: "tcp, tls and difference between key exchange algorithms"
summary: "A quick overview of TLS handshake"
profile: false
authors:
  - david-xiao
tags:
  - cybersecurity
  - tls
categories:
  - Information Security
  - TLS
date: 2020-08-31
lastmod: 2020-08-31
diagram: true
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

At a high level, the following occurs during a TLS handshake:

```mermaid
graph TD;
    A[Client establishes a TCP connection to the server] -->B[Client negotiates and agrees on TLS version and <br/>cipher suite] -->C[Clients validates server's certificate so as to confirm the server<br/> is who it says it is]
    C --> D[Client and server agrees on key exchange protocol. <br/>RSA and Diffie-Hellman are two common KEP algogirhtms]
    D --> E{Key Exchange Protocol}
    E -->|RSA| F[When both client and server have <br/>the client random, the server random, <br/>and the premaster secret, each side <br/>independently combine these three inputs<br/> to come up with the session keys. <br/>They should both arrive at the same value]
    E -->|DH| G[Both client and server independently <br/>agree on the same secret value using data <br/>exchanged in plaintext only]
    F -->H[Regardless of which KEP was used, <br/>the rest of the session uses the agreed key, an ephemeral symmetric key, <br/>to encrypt the communication both ways going forward]
    G -->H
```

Read more about DH [here](https://en.wikipedia.org/wiki/Diffie%E2%80%93Hellman_key_exchange#Cryptographic_explanation)

## Key Takeaways About the KEPs

👉 DH achieves forward secrecy while RSA does not.

👉 DH handshake takes longer than RSA.

## What Else You Need To Know about TLS

- TLS 1.0 and TLS 1.1 are no longer secure and should be avoided. A best practice is to use TLS version is 1.2 or later at the time of writing.

- HTTPS means "HTTP over TLS".

- Both SSH and TLS are purpose-built for secure communication over the Internet, but they are very different in many ways. Check out [my another post]({{< ref "/post/ssh-and-tls" >}}) where I explain the differences between the two. Thanks!
