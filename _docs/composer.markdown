---
layout:         doc-toc
title:          "Composer - Documentation"
category:       "composer"
logo:           composer
order:          3
excerpt:        "Dependency management with Composer."
---
[Composer](https://getcomposer.org/) is a tool for dependency management in PHP. It allows you to declare the libraries
your project depends on and it will manage (install/update) them for you. Continuousphp natively suppports Composer. It
automatically detects your Composer configuration when you create new pipelines and starts the dependency installation during
the build.

## Configuration

Go to the *Build Settings* (step 1 of the Pipeline configuration), and add the path to your `composer.json` file in the
dependencies section.

![Composer configuration](/assets/doc/composer/configuration.png)

### Testing and Deployment Package

continuousphp always creates 2 packages during a Build : The *Testing Package* that will be passed to the testing activities and
the *Deployment Package* that will (optionally) be deployed if the tests are successful. In both packages, continuousphp
installs the Composer dependencies.

To keep the *Deployment Package* as small as possible, packages from the `require-dev` section will only be installed in the
*Testing Package*.

### GitHub Token Authentication

When being connected with your GitHub account, continuousphp will automatically insert your GitHub Token into Composer to
authenticate and speedup the dependency installation.

### Composer Cache

The *Composer Cache* speeds up the Build process by caching the `vendor` folder created by the *composer install* command.

### Composer Hooks

continuousphp allows you to enable or disable the execution of
[Composer Hooks](https://getcomposer.org/doc/articles/scripts.md#command-events). If you choose to enable them, continuousphp
will search for `pre-install-cmd` and `post-install-cmd` entries in your `composer.json` file.

Both the `pre-install-cmd` and `post-install-cmd` will be executed before and after running `composer install` in the
Testing and Deployment Package. Additionally, the `post-install-cmd` will be executed a second time in the Test-activities
before the tests start. The reason for this is that the creation of the packages and the tests run in different containers
with different paths.

You can separately enable or disable the Composer Hooks for the *Testing Package* (step 1 of the pipeline configuration)
and for the *Deployment Package* (step 3).



