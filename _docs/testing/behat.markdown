---
layout:         doc
title:          "Behat - Documentation"
category:       "testing"
logo:           behat
order:          2
excerpt:        "Test your application with Behat."
---

[Behat](http://docs.behat.org/) is a tool that makes behavior driven development (BDD) possible. With BDD, you write human-readable stories that describe the behavior of your application. These stories can then be auto-tested against your application.

## Installation
You can use Behat on continuousphp by installing it via composer. Simply add it to your *composer.json* and update your *composer.lock* by running a *composer update*.

```yaml
"require-dev": {
  "behat/behat": "~2.5"
}
```

## Configuration
When you create a new pipeline, continuousphp will attempt to configure the tests automatically by looking for *behat.yml* or *behat.yml.dist* files in your repository. The available test locations will then be listed in the second step of the pipeline configuration. Of course, you can always modify or delete the configuration proposed by continuousphp.
As for all other testing frameworks on continuousphp, you can choose if the tests on Behat will block the deployment on failure or if the build goes into a warning state if the tests fail.

**IMPORTANT:** You don't need to run Behat using a shell command or a Phing target. continuousphp takes care of starting the tests for you.

![behat configuration](/assets/doc/testing/behat/configuration.png)

## Executing tasks

You can specify tasks to be executed before running the tests. This can be very useful to initialize external resources like [databases](/documentation/databases) or third-party services. Currently continuousphp supports [Phing](https://www.phing.info/), but others will be supported soon (shell commands, ...).

**IMPORTANT:** Tasks can be defined at different places in your workflow:

1. During the creation of the package to be tested

2. During the tests

3. During the creation of the package to be deployed

## Build

As soon as the testing package is created, the tests will start:

![behat build start](/assets/doc/testing/behat/build-start.png)

![behat build end](/assets/doc/testing/behat/build-end.png)

And that's it! You just configured your application to be tested with Behat!

## Downloads

When the build is finished, you can download the JUnit report that continuousphp automatically generated. Simply go to the build list and click the Download button to see which downloads are available for your builds.

<div class="row panel callout warning clearfix">
  <h2 class="left"><i class="fa fa-exclamation-triangle"></i></h2>
  Currently, only Behat 2.x supports the creation of a JUnit report. JUnit support for Behat 3.x will be available with Behat 3.1 that is currently in RC state.
</div>