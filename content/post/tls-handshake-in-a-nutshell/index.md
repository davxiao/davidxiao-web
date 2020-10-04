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
lastmod: 2020-09-10
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
    A[Client establishes a TCP connection to the server] -->B[Client sends Hello and list of cipher suites including TLS version] -->C[Server sends Hello, selected suite and certificate] --> D[Client validates certificate]
    D --> E[Client and server starts key exchange process. <br/>RSA and Diffie-Hellman are two common KEP algogirhtms]
    E --> F{Key Exchange Protocol}
    F -->|RSA| G[When both client and server have <br/>the client random, the server random, <br/>and the premaster secret, each side <br/>independently combine these three inputs<br/> to come up with the session keys. <br/>They should both arrive at the same value]
    F -->|DH| H[Both client and server independently <br/>agree on the same secret value using DH algorithm<br/> usually accompanied by RSA signature]
    G -->I[Regardless of which KEP was used, <br/>the rest of the session uses the agreed key, an ephemeral symmetric key, <br/>to encrypt the communication both ways going forward]
    H -->I
```

Read more about DH [here](https://en.wikipedia.org/wiki/Diffie%E2%80%93Hellman_key_exchange#Cryptographic_explanation)

## Key Takeaways About the KEPs

👉 DH achieves forward secrecy while RSA does not.

👉 DH handshake takes longer than RSA.

## What Else You Need To Know about TLS

- TLS 1.0 and TLS 1.1 are no longer secure and should be avoided. A best practice is to use TLS version is 1.2 or later at the time of writing.

- HTTPS means "HTTP over TLS".

- Both SSH and TLS are purpose-built for secure communication over the Internet, but they are very different in many ways. Check out [my another post]({{< ref "/post/ssh-and-tls" >}}) where I explain the differences between the two.

## Glossary

### Cipher Suite

A cipher suite is a set of algorithms. It usually contain include: a key exchange algorithm, a bulk encryption algorithm, and a message authentication code (MAC) algorithm.

For example, `TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` means:

- ECDHE_RSA indicates the key exchange algorithm being used.

- AES_128_GCM indicates the block cipher being used to encrypt the message stream, together with the block cipher mode of operation.
- SHA256 indicates the message authentication algorithm which is used to authenticate a message.

### ECDHE_RSA key exchange algorithm

In a nutshell, it is ECDHE signed by RSA. Signing defeats man-in-the-middle attack. See detail [here](https://en.wikipedia.org/wiki/Elliptic-curve_Diffie-Hellman)
