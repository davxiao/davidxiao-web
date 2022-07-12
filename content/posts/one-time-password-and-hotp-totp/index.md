---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "One Time Password, HOTP and TOTP"
subtitle: ""
summary: "All you need to know about OTP from a security perspective."
aauthors:
  - david-xiao
tags:
  - otp
categories:
  - Cybersecurity
date: 2020-09-10
lastmod: 2020-09-10
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

OTP is very common in today's MFA implementation.

## HOTP

HOTP stands for HMAC-based One-time Password algorithm. It computes the value with the following inputs:

- A cryptographic hash method, H (default is SHA-1)

- A secret key, K, which is an arbitrary byte string, and must remain private
- A counter, C, which counts the number of iterations
- A HOTP value length, d (6–10, default is 6, and 6–8 is recommended)

## TOTP

TOTP stands for Time-based One-time Password algorithm (TOTP). It is an extension of HOTP that generates a one-time password (OTP) by instead taking uniqueness from the current time.

More often than not, time is downsampled into larger durations (e.g., 30 seconds) to allow for validity between the parties.

To establish TOTP authentication, the authenticatee and authenticator must pre-establish both the HOTP parameters and the following TOTP parameters:

- T0, the Unix time from which to start counting time steps (default is 0)

- TX, an interval which will be used to calculate the value of the counter CT (default is 30 seconds)
