---
layout:         doc
title:          "PhpBench - Documentation"
category:       "testing"
logo:           phpbench
order:          6
excerpt:        "Test your application with PhpBench."
---

[PhpBench](http://www.phpbench.com/) allows you to benchmark explicit scenarios independently of the application context, and to run these scenarios multiple times in order to obtain a degree of confidence about the stability of the results. As a tool it is analogous to the test framework PHPUnit, but instead of tests it runs benchmarks and generates reports.

## Installation
You can use PhpBench on continuousphp by installing it via composer. Simply add it to your *composer.json* and update your *composer.lock* by running a *composer update*.

```yaml
"require-dev": {
  "phpbench/phpbench": "~0.10.0"
}
```

## Configuration
When you create a new pipeline, continuousphp will attempt to configure the tests automatically by looking for *phpbench.json* files in your repository. The available test locations will then be listed in the second step of the pipeline configuration. Of course, you can always modify or delete the configuration proposed by continuousphp.

**IMPORTANT:** You don't need to run PhpBench using a shell command or a Phing target. continuousphp takes care of starting the tests for you.

![phpbench configuration](/assets/doc/testing/phpbench/configuration.png)

## Executing tasks

You can specify tasks to be executed before running the tests. This can be very useful to initialize external resources like [databases](/documentation/databases) or third-party services. Currently continuousphp supports [Phing](https://www.phing.info/), but others will be supported soon (shell commands, ...).

**IMPORTANT:** Tasks can be defined at different places in your workflow:

1. During the creation of the package to be tested

2. During the tests

3. During the creation of the package to be deployed

## Build

As soon as the testing package is created, the tests will start:

![phpbench build start](/assets/doc/testing/phpbench/build-start.png)

![phpbench build end](/assets/doc/testing/phpbench/build-end.png)

And that's it! You just configured your application to be tested with PhpBench!