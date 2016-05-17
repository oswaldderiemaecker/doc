---
layout:         doc
title:          "phpspec - Documentation"
category:       "testing"
logo:           phpspec
order:          3
excerpt:        "Test your application with phpspec."
---

[phpspec](http://phpspec.readthedocs.org/en/latest/) is a tool which can help you writing tests designed by specification.

## Installation
You can use phpspec on continuousphp by installing it via composer. Simply add it to your *composer.json* and update your *composer.lock* by running a *composer update*.

```yaml
"require-dev": {
  "phpspec/phpspec": "~2.4"
}
```

## Configuration
When you create a new pipeline, continuousphp will attempt to configure the tests automatically by looking for *phpspec.yml* or *phpspec.yml.dist* files in your repository. The available test locations will then be listed in the second step of the pipeline configuration. Of course, you can always modify or delete the configuration proposed by continuousphp.
As for all other testing frameworks on continuousphp, you can choose if the tests on phpspec will block the deployment on failure or if the build goes into a warning state if the tests fail.

**IMPORTANT:** You don't need to run phpspec using a shell command or a Phing target. continuousphp takes care of starting the tests for you.

![phpspec configuration](/assets/doc/testing/phpspec/configuration.png)

## Executing tasks

You can specify tasks to be executed before running the tests. This can be very useful to initialize external resources like [databases](/documentation/databases) or third-party services. Currently continuousphp supports [Phing](https://www.phing.info/), but others will be supported soon (shell commands, ...).

**IMPORTANT:** Tasks can be defined at different places in your workflow:

1. During the creation of the package to be tested

2. During the tests

3. During the creation of the package to be deployed

## Build

As soon as the testing package is created, the tests will start:

![phpspec build start](/assets/doc/testing/phpspec/build-start.png)

![phpspec build end](/assets/doc/testing/phpspec/build-end.png)

And that's it! You just configured your application to be tested with phpspec!