---
layout:         post
title:          "Deploy on Heroku"
date:           2016-12-20 15:24:11
author:         'oswaldderiemaecker'
image:          '/assets/2016-12-20/heroku.png'
hasDescription: true
categories:     tutorial
---
This tutorial explains how to configure continuousphp to deploy a PHP application on Heroku.

<!--more-->

- [Set-up your Heroku app](#set-up-your-heroku-app)
- [Set-up continuousphp](#set-up-continuousphp)
  - [Prepare the Sample Application](#prepare-the-sample-application)
  - [Application Project Set-up in continuousphp](#application-project-set-up-in-continuousphp)
  - [Deployment pipeline Configuration](#deployment-pipeline-configuration)
- [Deploy the apps](#deploy-the-apps)
- [Notes](#notes)

## Set-up your Heroku app

This tutorial explains the deployment in a testing environment. You can use it as a base for any other environment you might need.

So let's start and create an Heroku application for your testing environment.

**Set up a new account:**

1. Open [https://www.heroku.com/](https://www.heroku.com/), and then choose **Sign Up Free**.
2. Follow the sign-in process
3. Fork the [Sample Application](https://github.com/oswaldderiemaecker/heroku-demo-zf-apigility-phinx) 
4. Type: cd heroku-demo-zf-apigility-phinx
5. Now, let's configure our project with the Heroku CLI, in your console:
  1. Type: heroku login 
  2. Provide the login and password you used during sign up
  3. To get a Token, type: heroku auth:token
  4. Take note of the Token we will use it in continuousphp later in this tutorial
  5. To logout from heroku, type: heroku logout
  6. Type: export HEROKU_API_KEY=xxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx 
  7. Before creating a Free Ignite mysql database, you first need to add your credit card details, goto the [Heroku Billing Dashboard](https://dashboard.heroku.com/account/billing)
  8. To create a Free Ignite mysql database type: heroku addons:create cleardb:ignite --app heroku-demo-zf-apigility-phinx
  9. To get the database url, type: heroku config --app heroku-demo-zf-apigility-phinx | grep CLEARDB_DATABASE_URL 
  10. Take note of the database URL we will use it in continuousphp later in this tutorial
6. Now let's configure the Heroku Buildpacks
  1. Goto Heroku website, in the **heroku-demo-zf-apigility-phinx** [App Settings](https://dashboard.heroku.com/apps/heroku-demo-zf-apigility-phinx/settings)
  2. Goto **Buildpacks**
  3. Remove the default **heroku/php** Heroku Buildpack
  4. Click on **Add buildpacks**
  5. In the **Buildpack URL** type: https://github.com/continuousphp/heroku-buildpack-php

Your Heroku app is now setup, please keep your **Heroku Token** and **database url** to use later in this tutorial. Let's now set continuousphp.

## Set-up continuousphp

### Prepare the Sample Application

Let's configure continuousphp to deploy our [Sample Application](https://github.com/continuousdemo/heroku-demo-zf-apigility-phinx).

Because we are going to configure continuousphp in testing environment, we need to create a develop branch, for which we will create a deployment pipeline.

**Preparing our Sample Application**

Create the **develop** branch:

```bash
git checkout -B develop
git push origin develop
```

This application include the following files:

* [composer.json](https://github.com/continuousdemo/heroku-demo-zf-apigility-phinx/blob/master/composer.json) with our dependencies (Zend Framework 2.x / Apigility / Phinx)
* [build.xml](https://github.com/continuousdemo/heroku-demo-zf-apigility-phinx/blob/master/build.xml) with the phing targets
* [phinx.yml.dist](https://github.com/continuousdemo/heroku-demo-zf-apigility-phinx/blob/master/phinx.yml.dist) with the Phinx Database migration configuration
* [behat.yml](https://github.com/continuousdemo/heroku-demo-zf-apigility-phinx/blob/master/behat.yml) with the Behat configuration
* [tests/phpunit.xml](https://github.com/continuousdemo/heroku-demo-zf-apigility-phinx/blob/master/tests/phpunit.xml) with the PHPUnit configuration
* [Procfile](https://github.com/continuousdemo/heroku-demo-zf-apigility-phinx/blob/master/ccbuild.sh) with the Heroku build script file
* [heroku/php.json](https://github.com/continuousdemo/heroku-demo-zf-apigility-phinx/blob/master/heroku/php.json) with the Heroku configuration and hook file

These are key files to set-up your application installation, testing and deployment. Feel free to take a look at them to get a better understanding.

### Application Project Set-up in continuousphp

**Set-up our application project in continuousphp**

1. Type in the omnibar the name of the application: **heroku-demo**
2. The fork should appear in the Project List (if not please wait a little that projects list update or simply logout/login)
3. Click on **Setup**
4. Click on **Setup Repository**
5. Click on **+** to add a deployment pipeline
6. Select the **develop** branch

### Deployment pipeline Configuration

**To configure your deployment pipeline**
 
1. In the Build Settings (Step 1):
   1. In the **PHP VERSIONS**, select the PHP versions: **5.6** / **7.0**
2. In the Test Settings (Step 2):
   1. continuousphp automatically discovers that you have a `behat.yml` and `phpunit.xml` in your repository and creates the testing configuration for you.
   2. Click on the **Behat** configuration panel. In the **PHING** section, select the following Phing Targets: **init-db** and **setup**
   3. Still in the **PHING** section, add the following variables: 
      * db.host: 127.0.0.1
      * db.port: 3306
      * db.dbname: skeleton
      * db.username: root
      * db.password:
   4. Click on **Next** to move to the Package Settings
3. In the Package Settings (Step 3):
   1. Select **Script**
   2. In the **PHING** section, select the following Phing Targets: **setup-phinx** and **generate-config**
      * db.host: $MYSQL_ADDON_HOST
      * db.port: $MYSQL_ADDON_PORT
      * db.dbname: skeleton
      * db.username: $MYSQL_ADDON_USER
      * db.password: $MYSQL_ADDON_PASSWORD
   2. Click on **Next** to move to the Deployment Settings
4. In the Deployment Settings (Step 4):
   1. Check the **enable deployment for successful builds** checkbox 
   2. Click on **+** on the **SCRIPT** panel
   3. Complete the script:
      * Name: Push-to-Heroku
      * Apply to: push
      * In the Script box:
```bash
# Print the Package Path and untar the package builded by continuousphp
echo $PACKAGE_PATH

mkdir ./heroku-demo-zf-apigility-phinx
tar xzf $PACKAGE_PATH -C ./heroku-demo-zf-apigility-phinx
cd ./heroku-demo-zf-apigility-phinx

# Install Heroku CLI
sudo apt-get update
sudo apt-get install software-properties-common
sudo apt-get install apt-transport-https
sudo add-apt-repository "deb https://cli-assets.heroku.com/branches/stable/apt ./"
curl -L https://cli-assets.heroku.com/apt/release.key | sudo apt-key add -
sudo apt-get update
sudo apt-get install heroku

# Install heroku-builds
heroku plugins:install heroku-builds

# Empty the gitignore & Deploy the app
cd ./heroku-demo-zf-apigility-phinx && rm -fv .gitignore && touch .gitignore && heroku builds:create --version "added foo feature" -a heroku-demo-zf-apigility-phinx
```
   4\. In the **Environment Variables**, add a variable called **HEROKU_API_KEY** with the Heroku Token you got in the **Set up a new account** steps. **Ensure to click on the Secured Variable Checkbox**.

## Deploy the apps

**Deploy the application with continuousphp**

1. Click on the **Play** button on the top right of the project
2. Select the **develop** branch
3. The build is started. It will create the testing and dist package, then run the tests (Behat and PHPUnit) for the choosen PHP versions, and finally it deploys the application.
4. In the deploy console, you should see **Deployment successfully started**
5. Login to Clever Cloud and go to your application Activity & Logs to see the details.

You can now modify your code with your favorite editor. For example, edit the file module/Application/view/layout/layout.phtml, add some text, save the file, commit and push it:

```bash
git checkout develop
git add module/Application/view/layout/layout.phtml
git commit -m "Modifying the layout"
git push
```

Now, everytime you push to the develop branch, your develop pipeline is triggered and continuousphp builds, tests and deploys your application upon a successful build.

## Notes

* This tutorial is an example and should not be used as is for production use. 

If you like to know more about production configuration or have questions about this tutorial, do not hesitate to contact us using the chat button!
