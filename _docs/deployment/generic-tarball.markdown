---
layout:         doc
title:          "Generic Tarball deployment - Documentation"
category:       "deployment"
logo:           generic-tarball
order:          1
excerpt:        "A generic deployment type that lets you handle the reception of the package."
---

This deployment type allows you to deploy to your servers by sending them a URL to the application package.

## Configure the deployment
To configure the deployment, you only need to specify the URL(s) to your server(s). If the build succeeds, your servers
will be notified with a HTTP POST request containing the package URL. This URL is valid for 30 minutes.

![Generic Tarball](/assets/doc/deployment/generic-tarball/configuration.png)

<div class="row panel callout warning clearfix">
  <h2 class="left"><i class="fa fa-exclamation-triangle"></i></h2>
  For security reasons, we strongly encourage you not to trust the package URL contained in the hook.
  Instead, get a new download URL from the API for this specific build, in order to prevent deployment injection.
  Or simply use our <a href="https://github.com/continuousphp/deploy-agent">Deploy Agent</a> to take care of your
  deployments.
</div>

### Install Continuous Deploy Agent

Simply install the deploy agent on your server and setup your application for deployment:

* Download [Composer](https://getcomposer.org/download/): `curl -sS https://getcomposer.org/composer.phar -o composer.phar`
* Install the Deploy Agent: `php composer.phar create-project continuousphp/deploy-agent`
* Start using the agent: `cd deploy-agent && ./agent`

[Learn more](https://github.com/continuousphp/deploy-agent/blob/master/README.md)

### Implement the webhook

The POST request that continuousphp sends to your servers contains the following fields:

* ***package_url*** : The temporary URL of your application package
* ***test_package_url*** : The temporary URL of the package used for the tests
* ***build_id*** : The build ID
* ***provider*** : The provider ID, *e.g. 'git-hub' or 'bitbucket'*
* ***repository*** : The URI of your repository, *e.g. 'continuousphp/deploy-agent'*
* ***pipeline*** : The pipeline used in the current build, *e.g. 'refs/heads/master'*

And that's it! You just configured the generic tarball deployment for your application.

