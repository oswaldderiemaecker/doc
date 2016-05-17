---
layout:         doc
title:          "Generic Tarball deployment - Documentation"
category:       "deployment"
logo:           generic-tarball
order:          3
excerpt:        "A generic deployment type that lets you handle the reception of the package."
---

This deployment type allows you to deploy to your servers by sending them a URL to the application package.

## Configure the deployment
To configure the deployment, you only need to specify the URL(s) to your server(s). If the build succeeds, your servers
will be notified with a HTTP POST request containing the package URL. This URL is valid for 30 minutes. 

![Generic Tarball](/assets/doc/deployment/generic-tarball/configuration.png)

The POST request that continuousphp sends to your servers contains the following fields:

* ***package_url*** : *The temporary URL of your application package*
* ***test_package_url*** : *The temporary URL of the package used for the tests*
* ***build_id*** : *The build ID*

And that's it! You just configured the generic tarball deployment for your application.