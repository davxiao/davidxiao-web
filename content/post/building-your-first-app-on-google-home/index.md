---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Building Your First App on Google Home in 10 minutes"
subtitle: "with Google Actions and Dialogflow"
summary: "This post will show you how to build a voice-activated app and deploy it to your Google Home device in 10 minutes. The app will respond to voice commands and even play personalized music for you! Not a bad way to play on someone's birthday, eh? :-)"
profile: false
authors:
  - david-xiao
tags:
  - cloud
  - GCP
  - Google-Home
  - coding
date: 2020-05-26
lastmod: 2020-05-26
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  caption: 'Image credit: [**Unsplash**](https://unsplash.com/photos/o9KZozGAKQo)'
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

***[TL;DR](https://en.wikipedia.org/wiki/Wikipedia:Too_long;_didn%27t_read)***

*This post will show you how to build a voice-activated app and deploy it to your Google Home device in 10 minutes.The app will respond to voice commands and even play personalized music for you! Not a bad way to play on someone's birthday, eh? :-)*

## Create a New Project on Google Actions Console

The app is built on Google Cloud using Google Actions and Dialogflow. If you don't have a Google Actions account, click [here](https://console.actions.google.com/) to create a new one. It's free.

When the account is created, go ahead and create a new project. Google Actions allows you to add Actions support to existing GCP projects, but we will create a new one to keep it simple.

TODO screenshot here

## Specify a Catchy Phrase for Your App

When your project is created, you need to specify a catchy phrase for the app so that every time when you say those "gateway words" to the Google Home device, it will activate the app for you. Go to your project dashboard and click on "Quick setup" followed by "Decide how your action is invoked", and put the phrase here. It may reject the phrase if it's too common or ambiguous, e.g. "Hello" is probably not a good one.

For example, my app is called "Hello Jukebox".

TODO screenshot here

## Add Actions to Your App

There is no secret here. The app is only be as smart as what it was told. In this app, it will respond to voice commands and act accordingly based on the Intents defined within the project. "Intent" is a term referring to a combination of command and its response.

With an app, developers can create as many intents as they want as long as no intent is stepping on one another's toes. For example, trying to create two separate intents both responding to the same command "what is my favoriate color" would probably be confusing to begin with.

Within an intent, developer can decide on what kind of responses it needs to have: it can be as simple as having Google Home say something or be more complicated with custom logic.

### Use Case 1: Simple Voice Commands and Text Responses

On the Actions dashboard, first scroll down until you see the Fulfillment section and click on "Edit in Dialogflow", then click on the Intents and start adding intents.

For example, you may want to create an intent called "special-intent", add "Do you know why today is so special" as voice command and add "Of course I know, David" as text responses to the intent. Those are what you would say to the app and what the app will say back respectively.

### Use Case 2: Implement custom logic using Cloud Function

The second use case is use enable Webhook in the intent and write a handler for it.

This approach allows you to implement custom logic for the intent. GCP supports either putting your custom code into a [Cloud Function](https://cloud.google.com/functions) or calling an external web service handler.

TODO screenshot

I will use the Cloud Functions for this example. Since it's serverless, we don't need to worry about resource or storage.

First, you need to enable "Webhook" on the intent that needs to run custom logic. Second, click on the Fulfillment on the left navbar, and enable Inline editor. Last, just copy n paste the following code example and click on save. That's it. The code example will first respond some text, then play an audio clip on Youtube. If you need to play a peronalized audio clip, you can replace the URL with your own thing, but then you have to deal with access control which is out of the scope of this post.

{{< gist davxiao 8d0d35d3c88078e5fc96114d02c8ee6b >}}

### Deploying to your Google Home Device

