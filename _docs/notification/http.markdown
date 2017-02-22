---
layout:         doc
title:          "HTTP - Documentation"
category:       "notification"
logo:           http
order:          4
excerpt:        "Get notifications about your builds via HTTP requests."
---

## Enable HTTP notifications in your pipeline

You can enable HTTP notifications in your pipeline by specifying the destination URL and the events upon which you want to notify.

Available events are :

1. Build creation
2. Build finished successfully
3. Build failed
4. Build has been deployed

![HTTP notification configuration](/assets/doc/notification/http/http-configuration.png)

## HTTP Payload

Notifications are sent as HTTP POST requests. The payload will have the following format:

* ***pusher*** : Username of the pusher
* ***state*** : in-progress&#124;complete
* ***result*** : success&#124;failed&#124;warning
* ***deployed*** : 2016-12-08T15:00:00+02:00
* ***package_url*** : The temporary URL of your application package
* ***test_package_url*** : The temporary URL of the package used for the tests
* ***build_id*** : The build ID
* ***provider*** : The provider's unique key, e.g. "git-hub" or "bitbucket"
* ***repository*** : owner/repository-name
* ***pipeline*** : The pipeline ID, e.g. "refs/heads/master" or "ALL REFS"

And that's it! You just configured HTTP notifications on your Deployment Pipeline.