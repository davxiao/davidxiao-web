---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Deploy a web app within 5 minutes"
subtitle: "on Google Cloud AppEngine"
summary: "Learn how to deploy a demo Python 3 web application on Google Cloud AppEngine. AppEngine is a managed platform on Google Cloud that allows customers to quickly deploy a scalable application without provisoning infrastructure."
toc: true
profile: false
authors: 
  - david-xiao
tags:
  - appengine
categories: 
  - Cybersecurity
date: 2021-07-05
lastmod: 2021-07-05
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

Deploy a sample Python 3 web application on Google Cloud AppEngine in 5 minutes. See 
[App Engine Quickstart](https://cloud.google.com/appengine/docs/standard/python3/quickstart) for reference.

## Steps

Open the cloud shell on Google Cloud Console and run the following commands:

```console
👉 cd

👉 gcloud app create
...

👉 git clone https://github.com/GoogleCloudPlatform/python-docs-samples

👉 cd ~/python-docs-samples/appengine/standard_python3/hello_world
 
👉 (optional) cloudshell workspace ~/python-docs-samples/appengine/standard_python3/hello_world

👉 virtualenv --python python3 ~/envs/hello_world
 
👉 pip3 install -r requirements.txt
 
👉 python3 main.py
    * Serving Flask app 'main' (lazy loading)
    * Environment: production
      WARNING: This is a development server. Do not use it in a production deployment.
      Use a production WSGI server instead.
    * Debug mode: on
    * Running on http://127.0.0.1:8080/ (Press CTRL+C to quit)
    * Restarting with stat
    * Debugger is active!
    * Debugger PIN: 111-142-068

👉 (optional) click on the Web Preview button on cloud shell to open a new browser window to see the hello world app

👉 gcloud app deploy
    Services to deploy:
    descriptor:                  [/home/david/python-docs-samples/appengine/standard_python3/hello_world/app.yaml]
    source:                      [/home/david/python-docs-samples/appengine/standard_python3/hello_world]
    target project:              [********]
    target service:              [default]
    target version:              [202107********]
    target url:                  [https://********.appspot.com]
    target service account:      [App Engine default service account]

    Do you want to continue (Y/n)?  y

    Beginning deployment of service [default]...
    Created .gcloudignore file. See `gcloud topic gcloudignore` for details.
    ╔════════════════════════════════════════════════════════════╗
    ╠═ Uploading 6 files to Google Cloud Storage                ═╣
    ╚════════════════════════════════════════════════════════════╝
    File upload done.
    Updating service [default]...done.
    Setting traffic split for service [default]...done.
    Deployed service [default] to [https://********.appspot.com]

    You can stream logs from the command line by running:
    $ gcloud app logs tail -s default

    To view your application in the web browser run:
    $ gcloud app browse
```

{{% alert note %}}
By default, the new application is publicly accessible.

There are many ways to protect web applications from unauthorized access. On Google Cloud, [BeyondCorp Enterprise](https://cloud.google.com/beyondcorp) is an enterprise scale [zero trust platform](https://cloud.google.com/blog/topics/developers-practitioners/what-zero-trust-identity-security) that enables customers to define fine-grained access level policies leveraging security signals from a variety of data sources including device, user identity profile, network and third-party security solution.
{{% /alert %}}
