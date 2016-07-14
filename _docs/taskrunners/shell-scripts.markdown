---
layout:         doc
title:          "Shell Scripts - Documentation"
category:       "taskrunners"
order:          1
excerpt:        "Shell Script support by continuousphp"
---
You can execute tasks on continuousphp by running simple shell scripts.

You can either call any available command like *cd*, *mkdir*, *rm*, ... or you can call script files from your repository. Example :

```
cd my-script-dir && ./my-script.sh
```

## Before creating the Testing package

You can run scripts before the Testing package is created. Every change made by these scripts at this location will potentially
be included in the Testing package. You can specify them in the *Build Settings* (step 1) of the pipeline configuration.

However, at this stage, don't try to interact with databases or other system dependencies, because the tests will run in a
separate container. Please check the next section <a href="#before-running-the-tests">Before running the tests</a> for creating and initializing databases.

![Shell Scripts - Testing Package](/assets/doc/taskrunners/shell-scripts/shell-scripts-testing-package.png)

## Before running the tests

You can run scripts before the execution of each testing framework (PHPUnit, Behat, ...). These scripts won't have any
influence on the Testing or Deployment packages. Go to Test Settings (step 2) of the pipeline configuration to add scripts prior
to test execution.

At this tage, you can create and initialize databases. However, changes on your source code won't have an impact on the Testing
or Deployment package.

![Shell Scripts - Before Tests](/assets/doc/taskrunners/shell-scripts/shell-scripts-before-tests.png)

## Before creating the Deployment package

You can run scripts before the Deployment package is created. Every change made by these scripts at this location will potentially
be included in the Deployment package. You can specify them in the *Package Settings* (step 3) of the pipeline configuration.

However, at this stage, don't try to interact with databases or other system dependencies, because the tests will run in a
separate container. Please check the previous section <a href="#before-running-the-tests">Before running the tests</a> for creating and initializing databases.

![Shell Scripts - Deployment Package](/assets/doc/taskrunners/shell-scripts/shell-scripts-deployment-package.png)