---
layout:         doc
title:          "PHPUnit - Documentation"
category:       "testing"
logo:           phpunit
order:          1
excerpt:        "Test your application with PHPUnit."
---

[phpunit](https://phpunit.de/manual/current/en/) is the most used unit testing tool in the PHP world.

## Installation
You can use PHPUnit on continuousphp by installing it via composer. Simply add it to your *composer.json* and update your *composer.lock* by running a *composer update*.

```json
"require-dev": {
  "phpunit/phpunit": "~5.1"
}
```

## Configuration
When you create a new pipeline, continuousphp will attempt to configure the tests automatically by looking for *phpunit.xml* or *phpunit.xml.dist* files in your repository. The available test locations will then be listed in the second step of the pipeline configuration. Of course, you can always modify or delete the configuration proposed by continuousphp.
As for all other testing frameworks on continuousphp, you can choose if the tests on PHPUnit will block the deployment on failure or if the build goes into a warning state if the tests fail.

**IMPORTANT:** You don't need to run PHPUnit using a shell command or a Phing target. continuousphp takes care of starting the tests for you.

![phpunit configuration](/assets/doc/testing/phpunit/configuration.png)

## Executing tasks

You can specify tasks to be executed before running the tests. This can be very useful to initialize external resources like [databases](/documentation/databases) or third-party services. Currently continuousphp supports [Phing](https://www.phing.info/), but others will be supported soon (shell commands, ...). However, PHPUnit is *unit testing* and it is not recommended using external resources like databases during the tests. After all, you are testing the smallest parts ("units") of your application and not entire uses cases. And you don't want your tests to fail because a third-party isn't available, right?

**IMPORTANT:** Tasks can be defined at different places in your workflow:

1. During the creation of the package to be tested

2. During the tests

3. During the creation of the package to be deployed

## Code Coverage

continuousphp automatically generates the code coverage of your PHPUnit tests. To accelerate the build, continuousphp runs the tests in two different, *parallel* activities. One having the code coverage disabled, and the other one with XDebug and code coverage enabled. The final (optional) deployment starts as soon as the tests without coverage are finished and successful.

To define what parts of your application should be tested, you can use PHPUnit's whitelist mechanism in your *phpunit.xml* file:

```xml
<testsuites>
    <testsuite name="Application test suite">
        <directory>ApplicationTest</directory>
    </testsuite>
</testsuites>
<filter>
    <whitelist processUncoveredFilesFromWhitelist="true">
        <directory suffix=".php">../src</directory>
        <exclude>
            <directory suffix=".php">../src/Application/Controller</directory>
            <directory suffix=".php">../src/Application/Form</directory>
        </exclude>
    </whitelist>
</filter>
```

For more information on how to configure PHPUnit, please check out the PHPUnit's [documentation](https://phpunit.de/manual/current/en/appendixes.configuration.html).

## php.ini

If you need to adapt php.ini settings, you can do this using the *<php>* tag in your phpunit configuration file. Check out PHPUnit's [documentation](https://phpunit.de/manual/current/en/appendixes.configuration.html#appendixes.configuration.php-ini-constants-variables) for more details.

## Build

As soon as the testing package is created, the tests will start:

![phpunit build start](/assets/doc/testing/phpunit/build-start.png)

![phpunit build end](/assets/doc/testing/phpunit/build-end.png)

And that's it! You just configured your application to be tested with PHPUnit!

## Downloads

When the build is finished, you can download the JUnit and Code Coverage (Clover) reports that continuousphp
automatically generated. Simply go to the build list and click the Download button to see which downloads are available for your builds.

## Environment Variables

You can use Environment Variables to configure your testing environment. Simply go to the *Test Settings* (step 2
of the pipeline configuration), open the PHPUnit configuration and add one or more Environment Variables:

![PHPUnit Env Vars](/assets/doc/testing/phpunit/env-vars.png)

### Encryption

Environment Variables can (optionally) be encrypted. Pay attention that, once a variable is encrypted, you can no longer obtain
it's value. An encrypted Environment Variable can only be decrypted during a build by continuousphp's workers. Encrypted
Environment Variables will be masked in the build output.