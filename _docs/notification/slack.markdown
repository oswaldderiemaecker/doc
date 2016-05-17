---
layout:         doc
title:          "Slack - Documentation"
category:       "notification"
logo:           slack
order:          1
excerpt:        "Get notifications about your builds on Slack."
---

[Slack](https://slack.com/) is a cloud-based team collaboration tool with many features: persistent chat rooms (channels) organized by topic, as well as private groups and direct messaging. All content inside Slack is searchable, including files, conversations, and people. Slack integrates with a large number of third-party services and supports community-built integrations.

## Create an Incoming Webhook on Slack
To send build notifications on Slack, you first need to create an [Incoming Webhook](https://my.slack.com/services/new/incoming-webhook/).

![Slack Incoming Webhook](/assets/doc/notification/slack/slack-create-webhook.png)

## Enable Slack notifications in your pipeline
After you created the Incoming Webhook on Slack, you can use it in your Deployment Pipeline. Simply go to the last step of the Pipeline configuration **Deployment & Notification settings**. There you can add the Webhook URL and choose the events upon which you want to be notified.

![Slack configuration](/assets/doc/notification/slack/slack-configuration.png)

And that's it! You just configured Slack notifications on your Deployment Pipeline.