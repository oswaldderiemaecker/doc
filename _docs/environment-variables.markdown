---
layout:         doc-toc
title:          "Environment Variables - Documentation"
category:       "environment-variables"
logo:           environment-variables
order:          19
excerpt:        "Support for Environment Variables by continuousphp."
---
continuousphp supports using environment variables at every stage of your builds :

* [When creating the Testing Package](/documentation/environment-variables#when-creating-the-test-package)
* [When creating the Deployment Package](/documentation/environment-variables#when-creating-the-deployment-package)
* [During the tests](/documentation/environment-variables#during-the-tests)
* [During the deployment](/documentation/environment-variables#during-the-deployment)
* [Built-in variables by continuousphp](/documentation/environment-variables#built-in-environment-variables)

## Encryption

Environment Variables can (optionally) be encrypted. Pay attention that, once a variable is encrypted, you can no longer obtain
it's value. An encrypted Environment Variable can only be decrypted during a build by continuousphp's workers. Encrypted
Environment Variables will be masked in the build output.

## When creating the Test Package

You can use Environment Variables during the creation of the Testing Package. Simply go to the *Build Settings* (step 1
of the pipeline configuration) and add one or more Environment Variables :

![Build Settings Env Vars](/assets/doc/environment-variables/build-settings.png)

## When creating the Deployment Package

You can use Environment Variables during the creation of the Deployment Package. Simply go to the *Package Settings* (step 3
of the pipeline configuration) and add one or more Environment Variables :

![Package Settings Env Vars](/assets/doc/environment-variables/package-settings.png)

## During the tests

You can use Environment Variables during the tests. Simply go to the *Test Settings* (step 2
of the pipeline configuration), then open the configuration of the testing framework and add one or more
Environment Variables :

* [PHPUnit](/documentation/testing/phpunit#environment-variables)
* [Behat](/documentation/testing/behat#environment-variables)
* [phpspec](/documentation/testing/phpspec#environment-variables)
* [PHPCS](/documentation/testing/phpcs#environment-variables)
* [Codeception](/documentation/testing/codeception#environment-variables)
* [phpbench](/documentation/testing/phpbench#environment-variables)

## During the deployment

You can use Environment Variables in the continuousphp [Script Deployment](/documentation/deployment/script#environment-variables).

## Built-In Environment Variables

continuousphp provides you with a set of built-in environment variables that you can use at every stage in your
Pipeline :

* ***CONTINUOUSPHP*** : continuousphp (can be used by your app to identify the environment)
* ***CPHP_BUILD_ID*** : The ID of the current build
* ***CPHP_GIT_COMMIT*** : The git commit ID of the current build
* ***CPHP_GIT_REF*** : The git reference of the current build
* ***CPHP_BUILT_BY*** : The username of the person who started the build
* ***CPHP_PR_ID*** : The Pull Request number of the current build (empty if it's no Pull Request)