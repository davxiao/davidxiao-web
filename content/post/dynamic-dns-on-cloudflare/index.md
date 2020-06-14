---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Dynamic DNS on Cloudflare in 5 Minutes"
subtitle: ""
summary: ""
profile: false
authors:
  - david-xiao
tags:
  - self-hosting
  - cloudflare
  - ddns
  - dynamic-dns
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
  caption: 'Image credit: [**Unsplash**](https://unsplash.com/photos/gnyA8vd3Otc)'

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

You will need the following to get dynamic DNS working on Cloudflare:

- Cloudflare as your DNS provider. Migrating from your current DNS provider over to Cloudflare is easy and free whether you are using Godaddy, Namecheap or another one.

- Get API token set up on Cloudflare.
- Install [cloudflare-cli](https://github.com/danielpigott/cloudflare-cli).

{{< figure src="token.png" title="Cloudflare API token" >}}

{{% alert note %}}
Cloudflare Token is preferred way over API key as token enables added security by allowing to specify access level with permissions and resources.

Token can be disabled when not in use.
{{% /alert %}}

When those are met, a one-liner like the following will update A record `api.davidxiao.me` with your public internet IP.

*NOTE: If you are only using DDNS on Cloudflare and not using its CDN, remove `--activate` from the command below.*

```bash
    $ env CF_API_KEY='your-own-cloudflare-token-NOT-api-key' \
        CF_API_DOMAIN='your-own-TLD-such-as-davidxiao.me' \
        cfcli --activate --type A edit your-subdomain-such-as-api.davidxiao.me \
        `curl -s 'https://ip.seeip.org/'` ;
```

Finally, put the code it into crontab will automate the process.
