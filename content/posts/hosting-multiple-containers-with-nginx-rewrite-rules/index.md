---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Hosting Multiple Apps With Nginx Rewrite Rules"
subtitle: ""
summary: "A reverse proxy such as Nginx will come in handy if you need to host multiple apps on a single domain. Here's a 5-minute how-to."
profile: false
authors:
  - david-xiao
tags:
  - self-hosting
  - nginx
  - nginx rewrite
categories:
  - Homelab
  - Site-Building
date: 2020-06-10
lastmod: 2020-06-10
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false
  caption: 'Image credit: [**Unsplash**](https://unsplash.com/photos/H_GGPWxJ1SE)'

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

{{< my-dropcap Suppose >}} you plan on hosting multiple API endpoints on one domain, make it `api.your-domain.com`, one way to do it is to put each endpoint under a distinctive path, each one would look like:

```bash
    https://api.your-domain.com/myapp1/
```

{{< figure src="rewrite.png" title="Tunneling through a Nginx reverse proxy" >}}

Configure a reverse proxy such as Nginx and set up rewrite rules can get it done quickly.

See below my example:

{{< gist davxiao c4f16ebbcdb2ec0701bcaad24640d12c >}}

Depending on your Linux distro and Nginx, most would need to put the conf file under `/etc/nginx/sites-available/` directory and create a symbol link in `/etc/nginx/sites-enabled/`.

Restart nginx service by `sudo systemctl restart nginx ;` and it's working!
