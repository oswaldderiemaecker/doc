---
layout:         post
title:          "Deploy on Clever-Cloud"
date:           2016-12-20 15:24:11
author:         'oswaldderiemaecker'
image:          '/assets/2016-12-20/clevercloud.png'
hasDescription: true
categories:     tutorial
---

![Deploy on Clever-Cloud](/assets/2016-12-20/clevercloud.png)

This tutorial explains how to configure continuousphp to deploy a PHP application on Clever-Cloud.

<!--more-->

- [Set up your Clever-Cloud app](#set-up-your-clevercloud-app)
- [Set up continuousphp](#set-up-continuousphp)
  - [Prepare the Sample Application](#prepare-the-sample-application)
  - [Application Project Setup in continuousphp](#application-project-setup-in-continuousphp)
  - [Deployment pipeline Configuration](#deployment-pipeline-configuration)
- [Deploy the apps](#deploy-the-apps)
- [Notes](#notes)

## Set up your Clever-Cloud app

This tutorial explains the deployment in a testing environment. You can use it as a base for any other environment you might need.

So let's start and create a Clever-Cloud application for your testing environment.

**Set up a new account:**

1. Open [https://www.clever-cloud.com/](https://www.clever-cloud.com/), and then choose **Sign Up Free**.
2. Click on **Add an Application**
3. Select **Create a brand new app**
4. Select **PHP**
5. Select **Git**
6. Click **Next** to choose the default instance type and autoscaling value
7. Type in the name of your application: **demo-zf-apigility-phinx**, and choose the region that suits your needs.
8. Select **MySQL** as your database
9. Fill in the name of your MySQL addon: **mysql-demo-zf-apigility-phinx**, and choose the zone that suits your needs.
10. Upload your public SSH key, then click add
11. Keep the environment variables, then click next
12. Fork the [Sample Application](https://github.com/continuousdemo/clevercloud-demo-zf-apigility-phinx) 
13. Take note of the git remote & push command and run them to add the clever remote repository in order to deploy the Application for the first time
14. Read the logs of the deployment
15. Go to Application Information and take a note of the deployment URL.

**IMPORTANT:** Now let's remove the git remote repository. We don't want that any push triggers a deployment. It's continuousphp that will deploy it after having tested and packaged the application.

```bash
git remote rm clever
```

## Set up continuousphp

### Prepare the Sample Application

Let's configure continuousphp to deploy our [Sample Application](https://github.com/continuousdemo/clevercloud-demo-zf-apigility-phinx).

Because we are going to configure continuousphp in testing environment, we need to create a develop branch, for which we will create a deployment pipeline.

**Preparing our Sample Application**

Create the **develop** branch:

```bash
git checkout -B develop
git push origin develop
```

This application include the following files:

* [composer.json](https://github.com/continuousdemo/clevercloud-demo-zf-apigility-phinx/blob/master/composer.json) with our dependencies (Zend Framework 2.x / Apigility / Phinx)
* [build.xml](https://github.com/continuousdemo/clevercloud-demo-zf-apigility-phinx/blob/master/build.xml) with the phing targets
* [phinx.yml.dist](https://github.com/continuousdemo/clevercloud-demo-zf-apigility-phinx/blob/master/phinx.yml.dist) with the Phinx Database migration configuration
* [behat.yml](https://github.com/continuousdemo/clevercloud-demo-zf-apigility-phinx/blob/master/behat.yml) with the Behat configuration
* [tests/phpunit.xml](https://github.com/continuousdemo/clevercloud-demo-zf-apigility-phinx/blob/master/tests/phpunit.xml) with the PHPUnit configuration
* [ccbuild.sh](https://github.com/continuousdemo/clevercloud-demo-zf-apigility-phinx/blob/master/ccbuild.sh) with the Clever-Cloud build script file
* [clevercloud/php.json](https://github.com/continuousdemo/clevercloud-demo-zf-apigility-phinx/blob/master/clevercloud/php.json) with the Clever-Cloud configuration and hook file

These are key files to set up your application installation, testing and deployment. Feel free to take a look at them to get a better understanding.

### Application Project Setup in continuousphp

**Set up our application project in continuousphp**

1. Type in the omnibar the name of the application: **clevercloud-demo**
2. The fork should appear in the Project List (if not, please wait a bit for the project list to update or simply logout/login)
3. Click on **Setup**
4. Click on **Setup Repository**
5. Click on **+** to add a deployment pipeline
6. Select the **develop** branch

### Deployment pipeline Configuration

**To configure your deployment pipeline**
 
1. In the Build Settings (Step 1):
   1. In the **PHP VERSIONS**, select the PHP versions: **5.6** / **7.0**
   2. Add the SSH private Key for the public key you uploaded in your clever application settings
      * Name: id_rsa
      * Private SSH Key: Your private key
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
   2. Click on **Next** to move to the Deployment Settings
4. In the Deployment Settings (Step 4):
   1. Check the **enable deployment for successful builds** checkbox 
   2. Click on **+** on the **SCRIPT** panel
   3. Complete the script:
      * Name: Push-to-CleverCloud
      * Apply to: push
      * In the Script box:

```bash
# Print the Package Path and untar the package builded by continuousphp
echo $PACKAGE_PATH

# Untar the continuousphp package
mkdir ./clevercloud-demo-zf-apigility-phinx
tar xzf $PACKAGE_PATH -C ./clevercloud-demo-zf-apigility-phinx

# Removing .git and removing composer, or clevercloud will run a composer update in production
cd ./clevercloud-demo-zf-apigility-phinx && rm -rf .git && rm composer.json composer.lock && rm .gitignore

# Initialise Git
cd ./clevercloud-demo-zf-apigility-phinx && git init && git config --global user.email "cphp_build@example.com" && git config --global user.name "cphp_build" && git add -A

# Commit
cd ./clevercloud-demo-zf-apigility-phinx && git commit -m "Removing composer files" --quiet

# Add the clever git remote repository with Deployment URL
cd ./clevercloud-demo-zf-apigility-phinx && git remote add clever git+ssh://git@push-par-clevercloud-customers.services.clever-cloud.com/app_aeac2554-a390-4995-bb78-86c86b8b1c39.git

# Move the private key so git push can use it
mv /home/cphp/.ssh/id_rsa.key /home/cphp/.ssh/id_rsa
cd ./clevercloud-demo-zf-apigility-phinx && git push -f -u clever master
```

## Deploy the app

**Deploy the application with continuousphp**

1. Click on the **Play** button on the upper right corner of the project page
2. Select the **develop** branch
3. The build is started. It will create the testing and dist package, then run the tests (Behat and PHPUnit) for the chosen PHP versions, and finally deploys the application.
4. In the deploy console, you should see **Deployment successfully started**
5. Login to Clever Cloud and go to your application Activity & Logs to see the details.

You can now modify the code with your favorite editor. For example, edit the file module/Application/view/layout/layout.phtml, add some text, save the file, commit and push it:

```bash
git checkout develop
git add module/Application/view/layout/layout.phtml
git commit -m "Modifying the layout"
git push
```

Now, everytime you push to the develop branch, your develop pipeline is triggered and continuousphp builds, tests and deploys your application upon a successful build.

## Notes

* This tutorial is an example and should not be used as is for production use. 
* A big Thank you to [Stéphane Jouvray](https://github.com/stephanejouvray) from [Interact-IV](http://www.interact-iv.com/) for the Apigility/ZF skeleton.

If you like to know more about production configuration or have questions about this tutorial, don't hesitate to contact us using the chat button!
