---
layout:         doc
title:          "Phing - Documentation"
category:       "taskrunners"
order:          2
excerpt:        "Phing support by continuousphp"
---
You can execute tasks on continuousphp by using [Phing](https://www.phing.info/).

**P**Hing **I**s **N**ot **G**NU make. It's a PHP project build system or build tool based on [​Apache Ant](http://ant.apache.org). You can do anything with it that you could do with a traditional build system like GNU make, and its use of simple XML build files and extensible PHP *"task"* classes make it an easy-to-use and highly flexible build framework.

Phing comes packaged with numerous out-of-the-box operation modules (tasks), and an easy-to-use OO model to extend or add your own custom tasks.

Phing provides the following features:

* Simple XML buildfiles
* Rich set of provided tasks
* Easily extendable via PHP classes
* Platform-independent: works on UNIX, Windows, Mac OSX
* No required external dependencies
* Built for PHP5

## Before creating the Testing package

You can run scripts before the Testing package is created. Every change made by these scripts at this location will potentially
be included in the Testing package. You can specify them in the *Build Settings* (step 1) of the pipeline configuration.

However, at this stage, don't try to interact with databases or other system dependencies, because the tests will run in a
separate container. Please check the next section <a href="#before-running-the-tests">Before running the tests</a> for creating and initializing databases.

![Phing - Testing Package](/assets/doc/taskrunners/phing/phing-testing-package.png)

## Before running the tests

You can run scripts before the execution of each testing framework (PHPUnit, Behat, ...). These scripts won't have any
influence on the Testing or Deployment packages. Go to Test Settings (step 2) of the pipeline configuration to add scripts prior
to test execution.

At this tage, you can create and initialize databases. However, changes on your source code won't have an impact on the Testing
or Deployment package.

![Phing - Before Tests](/assets/doc/taskrunners/phing/phing-before-tests.png)

## Before creating the Deployment package

You can run scripts before the Deployment package is created. Every change made by these scripts at this location will potentially
be included in the Deployment package. You can specify them in the *Package Settings* (step 3) of the pipeline configuration.

However, at this stage, don't try to interact with databases or other system dependencies, because the tests will run in a
separate container. Please check the previous section <a href="#before-running-the-tests">Before running the tests</a> for creating and initializing databases.

![Phing - Deployment Package](/assets/doc/taskrunners/phing/phing-deployment-package.png)

## Writing Targets

To show you how to use Phing targets, let's make an example for resetting a MySQL database.

To start, place a *build.xml* file in your repository. The skeleton of this file should look like this :

```xml
<project name="my-project" default="help" basedir=".">
    "Place your targets here ..."
</project>
```

First, we need to drop the existing database, if there is any :

```xml
<target name="drop-db">
    <echo file="./data/db/drop.sql">DROP DATABASE IF EXISTS `${db.name}`;</echo>
    <pdosqlexec url="mysql:host=localhost" userid="root">
        <transaction src="./data/db/drop.sql"/>
    </pdosqlexec>

    <delete file="${project.basedir}/data/db/drop.sql" quiet="true"/>
</target>
```

Second, we need a target to create the new database :

```xml
<target name="init-db" description="Create Database and Grants">
    <echo file="${project.basedir}/data/db/create.sql">
        CREATE DATABASE IF NOT EXISTS ${db.name};
        GRANT USAGE ON *.* TO '${db.username}'@'%' IDENTIFIED BY '${db.password}';
        GRANT UPDATE,CREATE,REFERENCES,ALTER,LOCK TABLES,CREATE VIEW,CREATE
        ROUTINE,TRIGGER,INSERT,DELETE,DROP,INDEX,CREATE TEMPORARY TABLES,EXECUTE,SHOW VIEW,ALTER ROUTINE,SELECT ON
        `${db.name}`.* TO '${db.username}'@'%';
    </echo>

    <pdosqlexec url="mysql:host=localhost" userid="root">
        <transaction src="${project.basedir}/data/db/create.sql"/>
    </pdosqlexec>

    <delete file="${project.basedir}/data/db/create.sql" quiet="true"/>
</target>
```

And last but not least, we can initialize the database (in this example, we use Doctrine) :

```xml
<target name="db-migration" description="Update the database version">
    <exec command="${doctrine.bin} migrations:migrate --no-interaction --quiet" passthru="true"/>
</target>
```

We can regroup multiple targets as dependencies of another target :

```xml
<target name="reset-db" description="Reset the Database" depends="drop-db, init-db, db-migration"/>
```

You can now choose the *reset-db* target in the list of Phing Targets in the configuration of your pipelines!

### Using Phing Variables

Phing Targets can be modified with Variables. They can be specified when calling Phing (in the pipeline configuration)
or you can place them in a `build.properties` file located in your repository. This file could look like :

```ini
version=1.0.0
author.name=Pascal Paulis
author.email=pascal.paulis@continuousphp.com

db.host=localhost
db.name=my-database
db.username=dbuser
db.password=abcd1234

doctrine.bin=${dir.vendor}/bin/doctrine-module
```

To load this property-file, you need to include it in your `build.xml` file :

```xml
<project name="my-project" default="help" basedir=".">
    <property file="./build.properties" />
</project>
```

### Using Environment Variables in Phing Targets

You can easily use Environment Variables in Phing Targets. If you specified a variable, let's say, `MY_ENV_VAR`, you
can use it in a Phing target by calling it with `${env.MY_ENV_VAR}`.