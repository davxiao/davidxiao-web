---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Secure Your HTTPS / SSL / TLS"
subtitle: ""
summary: "To site owners: Just because you've enabled HTTPS does not mean it's sound and secure. TLS v1.0 and v1.1 is unsecure and phasing out. "
profile: false
authors:
  - david-xiao
tags:
  - security
  - https
  - tls
categories:
  - Site-Building
date: 2020-06-11
lastmod: 2020-06-11
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false
  caption: 'Image credit: [**Webnames**](https://blog.webnames.ca/)'

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

If you own a personal site and like to make both your site and visitors secure, read this: Just because you've enabled HTTPS does not mean it's sound and secure.

TLS v1.0 and v1.1 are known to be vulnerable and should not be allowed on your site. It is a security best practice to make TLS v1.2 the minimum version allowed on your site.

For more detail, check out [this post](https://security.googleblog.com/2018/10/modernizing-transport-security.html) on Google Security Blog and [this post](https://developers.google.com/web/updates/2020/02/chrome-81-deps-rems#remove_tls_10_and_tls_11) on Google Chrome Browser Updates.

## Get a Test on your Site

You can use [SSLLabs](https://www.ssllabs.com/ssltest/) to conduct a quick test on your site.

{{< figure src="sslreport1.png" title="Initial test results" >}}

Click on any of the server will give you a brief explaination on the findings.

{{< figure src="sslreport2.png" title="See explaination here" >}}

I'm using Cloudflare as CDN for api.davidxiao.me, so I went on to the Cloudflare portal and updated the "Minimum TLS Version" to "TLS v1.2". Then performed a re-scan. It looks much better this time.

{{< figure src="sslreport3.png" title="The new results" >}}

Hope this is helpful!
