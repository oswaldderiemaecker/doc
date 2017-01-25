---
layout:         doc
title:          "PHP lint - Documentation"
category:       "testing"
logo:           PHPlint
order:          8
excerpt:        "Test your application's syntax with PHP lint."
---
## Configuration
You can use php linting on continuousphp by simply enabling it in your Pipelines. Simply go to the *Testing Settings* (step 2 of the pipeline configuration)
and choose *PHP lint* from the available Testing tools. You can specify Phing targets or Shell scripts to run before the tests.
As for all other Testing Tools on continuousphp, you can choose if the tests with will block the deployment on failure or if the build goes into a warning state if the tests fail.

### Wildcards
You can use wildcards or entire folders to configure PHP lint, e.g. `src/*`, if you want to match multiple files at once. Folders will be parsed recursively.

**IMPORTANT:** You don't need to run `php -l` using a shell command or a Phing target. continuousphp takes care of starting the tests for you.

![PHP lint configuration](/assets/doc/testing/phplint/configuration.png)

## Executing tasks

You can specify tasks to be executed before running the tests. This can be very useful to initialize external resources like [databases](/documentation/databases) or third-party services. Currently continuousphp supports [Phing](https://www.phing.info/), but others will be supported soon (shell commands, ...).

**IMPORTANT:** Tasks can be defined at different places in your workflow:

1. During the creation of the package to be tested

2. During the tests

3. During the creation of the package to be deployed

## Build

As soon as the testing package is created, the tests will start:

![PHP lint build start](/assets/doc/testing/phplint/build.png)

And that's it! You just configured your application to be tested with PHP lint!

## Environment Variables

You can use Environment Variables to configure your testing environment. Simply go to the *Test Settings* (step 2
of the pipeline configuration), open the PHP lint configuration and add one or more Environment Variables:

![PHP lint Env Vars](/assets/doc/testing/phplint/env-vars.png)

### Encryption

Environment Variables can (optionally) be encrypted. Pay attention that, once a variable is encrypted, you can no longer obtain
it's value. An encrypted Environment Variable can only be decrypted during a build by continuousphp's workers. Encrypted
Environment Variables will be masked in the build output.