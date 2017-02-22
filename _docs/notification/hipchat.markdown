---
layout:         doc
title:          "HipChat - Documentation"
category:       "notification"
logo:           hipchat
order:          2
excerpt:        "Get notifications about your builds on HipChat."
---

[HipChat](https://www.hipchat.com/) is a cloud-based team collaboration tool with many features: persistent & searchable chat rooms (channels), as well as direct messaging. HipChat integrates with a large number of third-party services and supports community-built integrations.

Here is a step-by-step guide to integrate continuousphp with HipChat :

## Create a custom Integration on HipChat
To send build notifications on HipChat, you first need to create a [Custom Integration (Webhook)](https://blog.hipchat.com/2015/02/11/build-your-own-integration-with-hipchat/).

Open the menu on the top right corner and select the `Integrations` entry:

![HipChat Incoming Webhook config](/assets/doc/notification/hipchat/config-hipchat-1.png)

Click on `+ Install new integrations` :

![HipChat Incoming Webhook config](/assets/doc/notification/hipchat/config-hipchat-2.png)

Then open `Build your own integration` :

![HipChat Incoming Webhook config](/assets/doc/notification/hipchat/config-hipchat-3.png)

Give your integration a name like `continuousphp` :

![HipChat Incoming Webhook config](/assets/doc/notification/hipchat/config-hipchat-4.png)

On the resulting page, copy the URL :

![HipChat Incoming Webhook config](/assets/doc/notification/hipchat/config-hipchat-5.png)

## Enable HipChat notifications in your pipeline
After you created the custom Integration on HipChat, you can use it in your Deployment Pipeline. Simply go to the last step of the Pipeline configuration **Deployment & Notification settings**. There you can add the Integration URL and choose the events upon which you want to be notified.

![HipChat Incoming Webhook config](/assets/doc/notification/hipchat/config-hipchat-6.png)

And that's it! You just configured HipChat notifications on your Deployment Pipeline.