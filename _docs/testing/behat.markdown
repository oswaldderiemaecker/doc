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

```json
"require-dev": {
  "behat/behat": "~3.1"
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

## Parallelization

You can parallelize your Behat tests by splitting them up into multiple `*.yml` files. Once the tests are splitted up,
simply add all the `yml` files to your Behat configuration in the pipeline. continuousphp will then automatically start a
new activity for each `yml` file and PHP version.

## Downloads

When the build is finished, you can download the JUnit report that continuousphp automatically generated. Simply go to the build list and click the Download button to see which downloads are available for your builds.

<div class="row panel callout warning clearfix">
  <h2 class="left"><i class="fa fa-exclamation-triangle"></i></h2>
  Currently, only Behat 2.x supports the creation of a JUnit report. JUnit support for Behat 3.x will be available with Behat 3.1 that is currently in RC state.
</div>

## UI Testing with Selenium and Chrome

In this example we will use the [Selenium Standalone Server](/documentation/browser-ui-testing/selenium-server/) and the
*Chrome* webbrowser to run UI tests. Both components are installed by default in the continuousphp containers.

### Setup
Besides Behat, you need to install the [Selenium WebDriver](http://mink.behat.org/en/latest/drivers/selenium2.html) and [Mink extension](http://mink.behat.org/en/latest/at-a-glance.html) by adding them to your `composer.json` file:
 
```json
"require-dev": {
  "behat/mink": "~1.7.0",
  "behat/mink-extension": "*",
  "behat/mink-browserkit-driver": "*",
  "behat/mink-selenium2-driver": "*"
}
```

You can now configure Behat to use the *Chrome* webbrowser through the Selenium2 WebDriver by adding the
following lines to your `behat.yml` file:
 
```yaml
default:
  suites:
    defalut:
      contexts:
        - FeatureContext
        - Behat\MinkExtension\Context\MinkContext
  extensions:
    Behat\MinkExtension:
      browser_name: chrome
      base_url: http://localhost
      sessions:
        default:
          selenium2:
            wd_host: "http://selenium:4444/wd/hub"
```

You can now create and run scenarios based on test steps from the `MinkContext`. Example:

```
Feature: Navigating through the Justice League's secret website
  In order to use the Justice League's secret website
  As a member of the Justice League
  I need to authenticate myself and be able to navigate through the website

  Scenario: Successfully login to the JLA's secret website
    Given I am on "https://www.batman.com"
    When I fill in "username" with: "bwayne"
    And I fill in "password" with: "Sup3rS3cr3t"
    And I press "Log In"
    Then I should be on "/super-villain-overview.html"
```

## UI Testing with Selenium and PhantomJS

Instead of using a real browser like e.g. *Chrome*, you can use a headless browser like
[PhantomJS](/documentation/browser-ui-testing/phantomjs/) to run your UI tests. In this example we use the Selenium WebDriver
to pilot PhantomJS as a replacement for real browser. Although the continuousphp containers are equippend with the
[Selenium Standalone Server](/documentation/browser-ui-testing/selenium-server/), in this type of setup it won't be used.

### Setup
Besides Behat, you need to install the [Selenium WebDriver](http://mink.behat.org/en/latest/drivers/selenium2.html) and [Mink extension](http://mink.behat.org/en/latest/at-a-glance.html) by adding them to your `composer.json` file:
 
```json
"require-dev": {
  "behat/mink": "~1.7.0",
  "behat/mink-extension": "*",
  "behat/mink-browserkit-driver": "*",
  "behat/mink-selenium2-driver": "*"
}
```
 
You can now configure Behat to use PhantomJS as a headless browser through the Selenium2 WebDriver by adding the
following lines to your `behat.yml` file:
 
```yaml
default:
  suites:
    defalut:
      contexts:
        - FeatureContext
        - Behat\MinkExtension\Context\MinkContext
  extensions:
    Behat\MinkExtension:
      base_url: http://localhost
      sessions:
        default:
          selenium2:
            wd_host: "http://127.0.0.1:8643"
```

You can now create and run scenarios based on test steps from the `MinkContext`. Example:

```
Feature: Navigating through the Justice League's secret website
  In order to use the Justice League's secret website
  As a member of the Justice League
  I need to authenticate myself and be able to navigate through the website

  Scenario: Successfully login to the JLA's secret website
    Given I am on "https://www.batman.com"
    When I fill in "username" with: "bwayne"
    And I fill in "password" with: "Sup3rS3cr3t"
    And I press "Log In"
    Then I should be on "/super-villain-overview.html"
```

## Environment Variables

You can use Environment Variables to configure your testing environment. Simply go to the *Test Settings* (step 2
of the pipeline configuration), open the Behat configuration and add one or more Environment Variables:

![Behat Env Vars](/assets/doc/testing/behat/env-vars.png)

### Encryption

Environment Variables can (optionally) be encrypted. Pay attention that, once a variable is encrypted, you can no longer obtain
it's value. An encrypted Environment Variable can only be decrypted during a build by continuousphp's workers. Encrypted
Environment Variables will be masked in the build output.