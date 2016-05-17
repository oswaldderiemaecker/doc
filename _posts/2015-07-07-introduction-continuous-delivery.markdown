---
layout:         post
title:          "An introduction to Continuous Delivery"
date:           2015-07-08 10:00:00
author:         'fdewinne'
hasDescription: true
categories:     tutorial
---

French magazine Programmez! asked Frederic Dewinne, our CTO, to write an introduction to Continuous Delivery.
This article is now available in English.

<!--more-->

Continuous Integration is a well-developed practice for other languages but fairly recent within the PHP ecosystem.
Continuous Delivery allows you to deliver new features quickly and frequently without any regression of your application.
To do so, this practice reduces manual tasks, automatically executes tests and creates builds.

A lot of Continuous Integration solutions exist but only a very few are 100% dedicated to PHP.
Most of them are multi-language and don't take specific needs of PHP developers into account. In this article,
we will present everything you need to know in order to implement CI in your projects.
We will use continuousphp as an example.

##Practices

There are different practices, in which CI is, that will help you to implement your Quality Assurance processes.
Let's take some time to present them all:

###Continuous Integration (CI)

Continuous Integration is a practice which implies continuous merging of branches into a common branch.
It is part of the Extreme Programming (XP) methodology, an agile method,
focused on the realisation aspect of an application.
Implementing Continuous Integration requires that the code base is built and tested for every commit,
in order to prevent any regression, linked to integration issues.
This new version of the code base can then be deployed on an integration server,
so that additional manual tests can be performed. 

###Continuous Delivery (CD)

Continuous Delivery goes a step further than Continuous Integration.
Everything that applies to this practice is also valid for this one.
An additional concept is to be added: packaging.
Every commit will trigger a workflow (the build) which will build code base, test it,
and create the package that can be delivered. Every commit can be delivered at any given time.

###Continuous Deployment

Continuous Deployment puts the most extreme workflow into practice.
While implementing all the Continuous Delivery principles, every stable build is deployed into a production environment.
This way, every new and finalized feature will be available for all end users, the fastest way possible.
This practice accelerates Business Value delivery into production.
An excellent Quality Assurance is key to put this into practice.


![Continuous Practices Comparison](/assets/2015-07-07/comparison.png)


##Let’s start

In this example, we will focus on Continuous Deployment and study every step to implement it.

###The Build

The first thing to consider is the management of software dependencies.
Which ones are needed to run tests and which ones will be mandatory for the application to perform?
No need to bother with test frameworks on a production server. Instead, we will make sure that third party libraries
used by your application are available, in their relevant version. To do so, [Composer](https://getcomposer.org) will be
our best friend.

Then comes automation. Several options can be considered but we have to remember that both production and development
environments should be covered within the same build. If you already use continuousphp (lucky you!),
Composer is natively supported and will create 2 packages: one for tests with all development dependencies,
the other for deployment without these additional dependencies. If you don’t, you will have to handle Composer the same way
you would do with other configuration dependencies below.

Software dependencies are now handled. But we are not done yet! In some cases, we will have to create/modify configuration
files, compile static files (CSS, JavaScript...). To do so, we will use [Phing](https://www.phing.info/), a tool allowing
you to automate tasks.
Phing is written in PHP... for PHP. It will be easy to extend Phing if additional tasks are needed, even though,
a large number of tasks already exist.

![Build example](/assets/2015-07-07/build.png)

###Tests

Continuous Deployment cannot be explained without mentioning tests! As for unit tests, several frameworks exist but
the most used is [PHPUnit](https://phpunit.de/). As unit tests don’t need either a database nor other system dependencies, it will be easy
to implement a continuous integration pipeline. We can let the build server do its job and execute.

Functional tests will be the tricky part to implement. As they can depend on some system dependencies,
we will have to provision these dependencies in order to execute tests. In this case, Phing is a useful tool which will
allow us to easily manage this step, needed for tests execution. If we are to provision a database, we will have to cut
this step into two parts. The first one to create/update the scheme alongside the application life cycle,
via migration tools ([Doctrine Migration](http://www.doctrine-project.org/projects/migrations.html),
[Phinx](https://phinx.org/), [DBDeploy](http://dbdeploy.com/)...), et the second one to insert fixtures (test data).

To write these tests, Behat is a must-have tool as this is the only one allowing you to perform Behavior Driven Development
in PHP. [Gherkin](https://github.com/cucumber/cucumber/wiki/Gherkin), used to write tests, makes them accessible.
They can be used as a specification base for different features.

###Deployment

This step is the aim for every application. It means to make it functional and available on an application server.
Creating a package is mandatory if one wants to execute a deployment or rollback to a specific version and its dependencies.
This package can be used to deploy a new version on existing servers, deploy the current version to new servers
(i.e. in a Cloud environment) or to a previous version if a rollback is needed. In the end, a package is a simple compressed
file (.zip, .tar.gz...) with the complete application and its installation/migration scripts.

Now that our package is available, we have to install it on one or more application server(s). Several deployment tools
working with packages exist. [Zend Server](http://www.zend.com/en/products/server) is probably the most comprehensive
in the PHP ecosystem, but other tools, such as the new [Amazon CodeDeploy](http://aws.amazon.com/codedeploy/) are really
promising. Moreover, it is possible to manage these deployments with provisioning tools ([Chef](https://www.chef.io/chef/),
[Puppet](https://puppetlabs.com/), [Ansible](http://www.ansible.com/)...).

##Deployment pipeline

Now we have to use a build platform ([Jenkins](https://jenkins-ci.org/), [continuousphp](https://continuousphp.com)...)
in order to implement all the previous steps and obtain a deployment pipeline which will trigger a build after every commit.
By creating different pipelines according to branches/tags as origin, we can work with an agile methodology and build
environments on the fly.
However, a platform allowing you to execute several parallel builds is needed.
Parallelization will prevent you from being slowed down after each commit.

![Configuration of a Deployment Pipeline](/assets/2015-03-23/tests.png)

As a conclusion, we can consider that implementing a deployment pipeline is not that complicated compared
to a continuous integration pipeline but is much more flexible as it handles every step of project management. Moreover,
it allows developers to get involved in the deployment part and to better understand the DevOps practice.
We can now also deliver added value to the application for the final user, faster and with less downtime.

###Further readings:

- [continuousphp tutorial](http://bitly.com/tutocontinuousphp)
- [Continuous Delivery: Reliable Software Releases through Build, Test, and Deployment Automation (Addison-Wesley Signature Series - Martin Fowler) - Jez Humble et Al., 2010](http://www.amazon.com/Continuous-Delivery-Deployment-Automation-Addison-Wesley/dp/0321601912/)
- [The Phoenix Project: A Novel about IT, DevOps, and Helping Your Business Win – Gim Kim et Al., 2014](http://www.amazon.com/The-Phoenix-Project-Helping-Business/dp/0988262509/)


This article was first published in French in the [June 2015 issue of Programmez ! magazine](http://www.programmez.com/magazine/article/industrialisation-php-en-continu).
