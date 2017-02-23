---
layout:         doc
title:          "AWS SNS - Documentation"
category:       "notification"
logo:           aws-sns
order:          5
excerpt:        "Get notifications about your builds via AWS SNS (Simple Notification Service)."
---

## Enable AWS SNS notifications in your pipeline

You can enable AWS SNS notifications in your pipeline by specifying the *Topic ARN* and the events upon which you want to notify.
The *Topic ARN* can be found in your SNS settings in the AWS console.

Available events are :

1. Build creation
2. Build finished successfully
3. Build failed
4. Build has been deployed

![AWS SNS notification configuration](/assets/doc/notification/aws-sns/aws-sns-configuration.png)

## HTTP Payload

Notifications are sent in JSON format. The payload will have the following format:

```json
{
  "pusher": "\<the pusher's username\>",
  "result": "\<success&#124;failed&#124;warning\>",
  "deployed": "2016-12-08T15:00:00+02:00",
  "package_url": "\<The temporary URL of your application package\>",
  "test_package_url": "\<The temporary URL of the package used for the tests\>",
  "build_id": "\<The build ID\>",
  "provider": "\<The provider's unique key, e.g. 'git-hub' or 'bitbucket'\>",
  "repository": "\<owner/repository-name\>",
  "pipeline": "\<The pipeline ID, e.g. 'refs/heads/master' or 'ALL REFS'\>"
}
```

And that's it! You just configured AWS SNS notifications on your Deployment Pipeline.