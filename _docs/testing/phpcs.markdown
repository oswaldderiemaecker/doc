---
layout:         doc
title:          "PHPCS - Documentation"
category:       "testing"
logo:           PHPCS
order:          4
excerpt:        "Test your application with PHPCS."
---

[PHPCS](https://github.com/squizlabs/PHP_CodeSniffer) is a tool that tokenizes PHP, JavaScript and CSS files to detect violations of a defined coding standard.

## Installation
You can use PHPCS on continuousphp by installing it via composer. Simply add it to your *composer.json* and update your *composer.lock* by running a *composer update*.

```json
"require-dev": {
  "squizlabs/php_codesniffer": "~2.5.0"
}
```

## Configuration
When you create a new pipeline, continuousphp will attempt to configure the tests automatically by looking for *phpcs.xml* files in your repository. The found configuration files will then be listed in the second step of the pipeline configuration. Of course, you can always modify or delete the configuration proposed by continuousphp.
As for all other testing frameworks on continuousphp, you can choose if the tests with PHPCS will block the deployment on failure or if the build goes into a warning state if the tests fail.

Keep in mind that PHPCS does a static analysis of your code. So PHPCS will only run on one of your chosen PHP versions.

**IMPORTANT:** You don't need to run PHPCS using a shell command or a Phing target. continuousphp takes care of starting the tests for you.

![PHPCS configuration](/assets/doc/testing/phpcs/configuration.png)

## Executing tasks

You can specify tasks to be executed before running the tests. This can be very useful to initialize external resources like [databases](/documentation/databases) or third-party services. Currently continuousphp supports [Phing](https://www.phing.info/), but others will be supported soon (shell commands, ...).

**IMPORTANT:** Tasks can be defined at different places in your workflow:

1. During the creation of the package to be tested

2. During the tests

3. During the creation of the package to be deployed

## Build

As soon as the testing package is created, the tests will start:

![PHPCS build start](/assets/doc/testing/phpcs/build.png)

And that's it! You just configured your application to be tested with PHPCS!

## Downloads

When the build is finished, you can download the PHPCS reports that continuousphp automatically generated:

1. The full report

2. The info report

3. The gitblame report

Simply go to the build list and click the Download button to see which downloads are available for your builds.