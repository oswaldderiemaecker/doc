---
layout:         post
title:          "Create your Continuous Delivery pipelines with continuousphp"
date:           2015-10-13 19:13:11
author:         'fdewinne'
image:          '/assets/2015-03-23/pipelines-share.png'
hasDescription: true
categories:     tutorial
---
Here comes the first of our series of tutorials where we will explain how to make the most out of continuousphp.  
This tutorial explains how to configure continuousphp for the first time.

Other tutorials will follow in the next few weeks, stay tuned!

If you are also interested in watching videos, you can also subscribe to our [YouTube channel](https://www.youtube.com/channel/UCXAhXvtNuXrZ6Xv4XQg7ZBA).

<!--more-->

- [Set up the repository](#set-up-the-repository)
- [Add a new deployment/delivery pipeline](#add-a-new-deploymentdelivery-pipeline)
- [Specify the PHP versions you want to test](#specify-the-php-versions-you-want-to-test)
- [Review your build settings](#review-your-build-settings)
  - [Credentials](#credentials)
  - [Task runners](#task-runners)
  - [Dependency managers](#dependency-managers)
- [Review your test settings](#review-your-test-configuration)
  - [Blocking tests](#blocking-tests)
  - [Task runners](#task-runners)
  - [Quality analysis tools](#quality-analysis-tools)
- [Package your application](#package-your-application)
- [Deploy your application](#deploy-your-application)
  - [Generic Tarball](#generic-tarball)
  - [Amazon CodeDeploy](#amazon-codedeploy)
  - [Zend Deployment](#zend-deployment)
- [Stay tuned about your builds](#stay-tuned—about-your-builds)
- [Create additional pipelines](#create-additional-pipelines)

To start, simply browse your repositories through the omnibar - you can select one repository to set it up as a new project.  
The **“My Projects”** area will list all your projects once they have been configured.

<img src="/assets/2015-03-23/select-repository.png"
     srcset="/assets/2015-03-23/select-repository@2x.png 1140w"
     alt="omnibar" />

<div class="row panel callout clearfix">
  <h3>GitHub users</h3>
  If you can't see repositories of a specific organization, this organization
  probably enabled the third-party application access restriction policy. Have look at the
  [GitHub documentation](/documentation/vcs/github/#add-your-organizations) to find out how to enable
  continuousphp for your organisation.
</div>

## Set up the repository

Now, you are on the project page where you will see your pipelines status and build history.
But, first of all, you have to configure your repository through the
&nbsp; <label class="label primary">Set up repository</label> &nbsp;
button. By doing this, continuousphp will add a new webhook to your repository to trigger new builds on push and pull
requests. It will also create a new deployment key to allow continuousphp's workers to clone your repository when builds
start.

> Only repository owners can execute this operation.

<img src="/assets/2015-03-23/setup.png"
     srcset="/assets/2015-03-23/setup@2x.png 1140w"
     alt="Setup repository" />

## Add a new deployment/delivery pipeline

Once your repository is set up, you can create as many deployment or delivery pipelines as needed.
What is a deployment pipeline?
It's way to break up your builds into stages. These stages will be be parallelized as much as possible to speed up your
build. The first stages will clone your repository, provision your application and create two packages. The first one
for testing purpose. The second one, for deployment. The testing package will be shared with the testing stages that will
run your tests in ephemeral containers. If all your tests pass, the deployment stage will trigger or execute the deployment
depending of the adapter you selected.

To create your first pipeline, just click the <span class="label round plus"><i class="fa fa-plus"></i></span> button
and select the starting point of your pipeline. You can choose a specific branch or special starting point like
**All branches** or **All tags** (eg. for production deployments)

> You need admin privilege on your repository hosting platform (GitHub, Bitbucket, GitLab) in order to create/update pipelines.

<img src="/assets/2015-03-23/create-pipeline.png"
     srcset="/assets/2015-03-23/create-pipeline@2x.png 1140w"
     alt="Create new pipeline" />

## Specify the PHP versions you want to test

At this time, your pipeline is almost ready to be saved. The only missing information is the PHP versions you want to
use to provision your application and execute your tests. You can choose as many PHP versions as you want. If you don't
find the one you use, just leave a message in the Chat window and we'll add it quickly. For every PHP version you'll select,
we'll run your test in a separate container in parallel. And to provision your application, we'll always use the oldest
version you chose to ensure retro compatibility of your deployment package.

<img src="/assets/2015-03-23/select-php-version.png"
     srcset="/assets/2015-03-23/select-php-version@2x.png 1140w"
     alt="Select PHP Version" />

## Review your build settings

Now, your pipeline is saved and pre-configured with the tools continuousphp found in your repository. However, as we
use your default branch to auto-discover the tools you use, you need to review and adapt it depending on your needs.

<img src="/assets/2015-03-23/build-settings.png"
     srcset="/assets/2015-03-23/build-settings@2x.png 1140w"
     alt="Review your build settings" />

### Credentials

When you're running a build, you could need credentials to access third party services. In this section, you can
define specific credentials (currently, only AWS IAM is supported. Just contact us if you need another one) to be
injected in the container before each activity execution. A simple example: You want to create a stack on Amazon AWS
using the API? Just define the credentials in this section and you'll be able to use the SDK all over the different
build stages.

### Task runners

At this time, continuousphp only supports Phing as task runner. If you want to use other tools like Grunt, Gulp or even
make, you need to drive them with Phing till we add native support for these tools. By the way, don't hesitate to
contact us if you absolutely need another task runner. Depending on demand, we will adapt our roadmap to better fit our
customer needs!

In this section, you can define which targets have to run before creating the test package. Keep in mind that tests
will run in another container. So, don't interact with database during this stage.

> If you specify Phing as dependency in your composer.json file, continuousphp will use the version you defined instead
> of ours.

### Dependency managers

continuousphp natively supports Composer. Other dependency managers will be supported soon but can currently be handled
through Phing targets. Just after cloning your repository, continuousphp will run the composer install command with your
development dependencies for the test package and without for the deployment package. In both case, continuousphp will
run composer with the option to generate your autoloader classmap. Additionally, you can specify basic http
authentication to allow composer accessing your private repositories on bitbucket, a Satis or a Toran proxy.


## Review your test configuration

Once you've reviewed the build settings, you should switch to the test settings. If we successfully discovered your
test framework configuration files, everything should be ok. However, you can add or remove testing framework to run
at your convenience.

> If you want to execute testing frameworks in your build, you must also add them as development dependency in your
> composer.json file.

<img src="/assets/2015-03-23/test-settings.png"
     srcset="/assets/2015-03-23/test-settings@2x.png 1140w"
     alt="Review your test settings" />

### Blocking tests
For each testing framework you want to execute, you can specify if its failure will block your deployment or not.
If not, failing tests will just set the build in a warning state instead of stopping its execution and blocking
the deployment.

### Task runners
As for the packaging stages, you can specify Phing targets to execute before running a specific testing framework. This
allows you to provision a test database before executing automated acceptance tests.

### Quality analysis tools
If you want to use Quality analysis tools (currently, only PHP CodeSniffer is supported), you must specify a PHP version
to use. By default, the smallest available version is selected.


## Package your application

From now you have a complete Continuous Integration pipeline. Let's go further and implement Continuous Delivery.
To take the step, choose a package format depending on how you'll deploy (you'll see later on the different options you
have) and set up packaging target you want to run before packaging like you did for the test package.

When you've completed this stage, you have a Continuous Delivery pipeline. Every push or pull request to a reference
corresponding to this pipeline will start a new build and create a package of your application. You can download these
packages through our [API](http://docs.continuousphp.apiary.io/) or with our
[SDK](https://github.com/continuousphp/sdk) or our [Phing tasks](https://github.com/continuousphp/phing-tasks).

<img src="/assets/2015-03-23/package-settings.png"
     srcset="/assets/2015-03-23/package-settings@2x.png 1140w"
     alt="Review your packaging settings" />


## Deploy your application

Well, it's nice to have a package of your application available for deployment. It would be really better if you could
deploy it easily. To enable this feature, simply turn on the "Deploy every non failed build" option in the last stage
of your deployment pipeline and define a destination depending of the package format you've selected. Let's have a look
to the available formats.

### Generic Tarball

This is definitely the most generic format we propose... but also the most flexible. It's just a *.tar.gz file of your
provisioned application. Once the package is written, we'll call a webhook to the destinations you've defined.

> Find more details about Generic Tarball deployment in our [documentation](/documentation/deployment/generic-tarball).

<img src="/assets/2015-03-23/generic-tarball.png"
     srcset="/assets/2015-03-23/generic-tarball@2x.png 1140w"
     alt="Generic Tarball deployment" />

### Amazon CodeDeploy

CodeDeploy is a deployment service from Amazon AWS available in the Amazon AWS cloud for free and on your on premise
server with a small additional fee.

> Find more details about CodeDeploy deployment in the [documentation](/documentation/deployment/aws-code-deploy).

<img src="/assets/2015-03-23/code-deploy.png"
     srcset="/assets/2015-03-23/code-deploy@2x.png 1140w"
     alt="Amazon CodeDeploy deployment" />

### Zend Deployment

Zend Deployment is a Zend Server component. It allows you to deploy an application on top of a Zend Server cluster
through its API. If you already use Zend Server, it's probably the easiest way for you to start using Continuous
Deployment

> Find more details about Zend Deployment in the [documentation](/documentation/deployment/zend-server).

<img src="/assets/2015-03-23/zend-server.png"
     srcset="/assets/2015-03-23/zend-server@2x.png 1140w"
     alt="Zend Deployment" />

## Stay tuned about your builds

Now, you have a Deployment pipeline up and running for the environment you've just configured. However, you'll
probably want to be notified if a failure occurs. Basically, you'll already have an email notification on every failing
build. Another option is to be notified through Slack. To do this, simply add a new slack notification configuration
and you'll be notified for all event types you've checked.

<img src="/assets/2015-03-23/slack.png"
     srcset="/assets/2015-03-23/slack@2x.png 1140w"
     alt="Slack" />

## Create additional pipelines

Typically, you can create as many pipelines you want depending of your branching model. If you use git-flow,
probably you'll have one pipeline for your production environment based on your tags, one for your User Acceptance Test
(UAT) based on your master branch (or a release branch) and one for your integration server based on your develop
branch.

<img src="/assets/2015-03-23/pipelines.png"
     srcset="/assets/2015-03-23/pipelines@2x.png 1140w"
     alt="Git flow" />

Now you can [start using continuousphp](https://continuousphp.com/)  
We do hope you will enjoy using it for your projects. Any question, don’t hesitate to ask us using the chat button!
