---
layout:         post
title:          "Deploy with AWS - Part 1 : CodeDeploy"
date:           2016-08-14 15:24:11
author:         'oswaldderiemaecker'
image:          '/assets/2015-03-23/pipelines-share.png'
hasDescription: true
categories:     tutorial
---
This tutorial explains how to configure continuousphp and AWS to deploy a PHP application with AWS CodeDeploy.

<!--more-->

- [Set-up the AWS environment accounts](#set-up-the-aws-environment-accounts)
- [Set-up the package S3 bucket](#set-up-the-package-s3-bucket)
- [Set-up the IAM permissions](#set-up-the-iam-permissions)
- [Set-up the PHP EC2 autoscaling](#set-up-the-php-ec2-autoscaling)
  - [Creating the EC2 Key Pair](#creating-the-ec2-key-pair)
  - [Set-up the infrastructure](#set-up-the-infrastructure)
  - [Set-up the AWS CodeDeploy Agent](#set-up-the-aws-codedeploy-agent)
- [Set-up CodeDeploy](#set-up-codedeploy)
- [Set-up continuousphp](#set-up-continuousphp)
  - [Prepare the Sample Application](#prepare-the-sample-application)
  - [Application Project Set-up in continuousphp](#application-project-set-up-in-continuousphp)
  - [Deployment pipeline Configuration](#deployment-pipeline-configuration)
- [Deploy the apps](#deploy-the-apps)
- [Notes](#notes)

## Set-up the AWS environment accounts

Based on your deployment strategy you may plan to deploy your application in different environments like testing, staging and production.
AWS recommends the separation of responsibilities, and therefore you should create a separate AWS account for every environment you might require.

This tutorial explains the deployment in a testing environment. You can use it as a base for any other environment you might need.

So let's start and create an AWS account for your testing environment.

**Set up a new account:**

1. Open [https://aws.amazon.com/](https://aws.amazon.com/), and then choose *Create an AWS Account*.
2. Follow the online instructions.

## Set-up the package S3 bucket

You are now ready to create the S3 bucket in which the continuousphp package will be uploaded for AWS CodeDeploy to deploy.

**Create a bucket**

1. Sign-in to the AWS Management Console and open the Amazon S3 console at https://console.aws.amazon.com/s3.
2. Click *Create Bucket*.
3. In the *Create a Bucket* dialog box, fill in the *Bucket Name*. In this example we will use **"mycompany-package"**.
4. In the *Region* box, select a region from the drop-down list.
5. Click Create.

When Amazon S3 has successfully created your bucket, the console displays your empty bucket **"mycompany-package"** in the Bucket panel. 

For more information, visit the [AWS S3 Bucket documentation](http://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html)

## Set up the IAM permissions

To start we are going to create an IAM policy to grant continuousphp the permission to upload the package to your bucket **"mycompany-package"** and communicate with CodeDeploy to deploy it.

**Create the User policy**

1. Sign-in to the IAM console at https://console.aws.amazon.com/iam/ with your user that has administrator permissions.
2. In the navigation pane, choose *Policies*.
3. In the content pane, choose *Create Policy*.
4. Next to *Create Your Own Policy*, choose *Select*.
5. As *Policy Name*, type **testingDeployment**.
6. As *Policy Document*, paste the following policy.

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Stmt1438004530000",
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:PutObjectAcl",
                "s3:PutObject"
            ],
            "Resource": "arn:aws:s3:::mycompany-package/testing/*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "codedeploy:Batch*",
                "codedeploy:CreateDeployment",
                "codedeploy:Get*",
                "codedeploy:List*",
                "codedeploy:RegisterApplicationRevision"
            ],
            "Resource": "*"
        }
    ]
}
```

7\. Choose *Validate Policy* and ensure that no errors display in a red box at the top of the screen. Correct any that are reported.

8\. Choose *Create Policy*.

Now let's create an IAM user with an Access Key and attach the policy we've just created.  

**Create a new User and attach the User policy**

1. Sign-in to the Identity and Access Management (IAM) console at https://console.aws.amazon.com/iam/.
2. In the navigation pane, choose *Users* and then choose *Create New Users*. 
3. Enter the following user: **deployment.testing**
4. Generate an access key for this user at this time by selecting *Generate an access key for each user*.
5. Save the generated access key in a safe place.
6. Choose *Create*.
7. Edit the user, go to *Permission* and Attach the policy **testingDeployment** to our user **deployment.testing**. 

And now let's create the CodeDeploy Role which grants to CodeDeploy the permissions to get informations about the infrastructure's EC2 Instance.

**Create a new Role and attach the CodeDeploy policy**

1. Sign-in to the Identity and Access Management (IAM) console at https://console.aws.amazon.com/iam/.
2. In the navigation pane, choose *Roles* and then choose *Create New Roles*. 
3. Enter the following Role name: **CodeDeploy**
4. In the AWS Service Roles, select **AWS CodeDeploy**
5. Attach the AWSCodeDeployRole
6. Choose *Create*.

For more information, visit the [AWS IAM User documentation](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html) and the [Tutorial: Create and Attach Your First Customer Managed Policy](http://docs.aws.amazon.com/IAM/latest/UserGuide/tutorial_managed-policies.html)

## Set-up the PHP EC2 autoscaling

We have set-up all the IAM permissions needed for continuousphp to put its build packages into the S3 bucket, and trigger a deployment with CodeDeploy. Also CodeDeploy now has a Role that grants it access to your EC2 informations like Instance IDs, Tags and Autoscaling.

Let's now create our infrastructure. For this we will use this [CloudFormation template](https://github.com/oswaldderiemaecker/continuousphp-aws-cloudformation-template).

### Creating the EC2 Key Pair

To create your key pair using the Amazon EC2 console

1. Open the Amazon EC2 console at https://console.aws.amazon.com/ec2/. 
2. In the navigation pane, under *NETWORK & SECURITY*, choose *Key Pairs*.
3. Enter a name for the new key pair in the Key pair name field of the *Create Key Pair* dialog box, and then choose *Create*.
The private key file is automatically downloaded by your browser. The base file name is the name you specified as the name of your key pair, and the file name extension is *.pem*. Save the private key file in a safe place.
4. If you will use an SSH client on a Mac or Linux computer to connect to your Linux instance, use the following command to set the permissions of your private key file so that only you can read it :

```bash
chmod 400 my-key-pair.pem
```

### Set-up the infrastructure

First, we need to fork the sample application which include our CloudFormation template. The infrastructure is now a dependency of our application.

**Fork the Sample Application**

1. Login to your GitHub account.
2. Visit our [Sample Application](https://github.com/oswaldderiemaecker/aws-demo-zf-apigility-phinx).
3. Click on **Fork* button at the top right
4. Select where you want to fork the repository to
5. Clone the repository

Let's now create our Infrastructure by creating a CloudFormation Stack.

**Create a stack on the AWS CloudFormation console**

1. Log in to the AWS Management Console and select *CloudFormation* in the Services menu.
2. Create a new stack by using one of the following options :
  1. Click *Create Stack*. This is the only option if you have a currently running stack.
  2. Click *Create New Stack* in the CloudFormation Stacks main window. This option is visible only if you have no running stacks.
3. On the *Select Template* page
  1. Choose a template
  2. Select *Upload a template to Amazon S3* and select the AWS CloudFormation template on your local computer. Choose the [CloudFormation template](https://github.com/oswaldderiemaecker/continuousphp-aws-cloudformation-template) that you just forked.
4. Click Next to accept your settings and proceed by specifying the stack name and parameters.

**Specify the stack parameter values**

1. On the *Specify Details* page, type a stack name in the *Stack name* box.
2. In the *Parameters* section
  * InstanceType: t2.micro
  * KeyName: The EC2 Key Pair to allow SSH access to the instances
  * OperatorEMail: EMail address to notify if there are any scaling operations
  * S3AppsBucket: The S3 Bucket path **mycompany-package-testing/testing** that you created in [Set-up the package S3 bucket](#set-up-the-package-s3-bucket)
  * SSHLocation: The IP address range that can be used to SSH to the EC2 instances
3. When you are satisfied with the parameter values, click *Next* to proceed with setting options for your stack.
4. Click *Next Step* to proceed with reviewing your stack.
5. On the Review page, review the details of your stack.
6. After you have reviewed the stack launch settings and the estimated cost of your stack, click *Create* to launch your stack.

Your stack appears in the list of AWS CloudFormation stacks, with the status **CREATE_IN_PROGRESS**.

After your stack has been successfully created, its status changes to **CREATE_COMPLETE**.
 
### Set-up the AWS CodeDeploy Agent 

If you used the CloudFormation Template above you don't need to install the AWS CodeDeploy Agent, the Amazon Machine Image (AMI) used in the Template is preconfigured by continuousphp and includes it already.

If you want to make your own AMI please visit the [Install or Reinstall the AWS CodeDeploy Agent](http://docs.aws.amazon.com/codedeploy/latest/userguide/how-to-run-agent-install.html)

## Set-up CodeDeploy

We are almost there. Let's set up CodeDeploy with the following steps:

1. Sign in to the AWS Management Console and open the AWS CodeDeploy console at https://console.aws.amazon.com/codedeploy.
2. If the Applications page does not appear, on the AWS CodeDeploy menu, choose *Applications* then *Create New Application* or **Get Started Now** then select **Custom Deployment** and click on **Skip Walkthrought**.
3. In the *Application Name* box, type the application's name: **mycompany_app**. (In an AWS account, an AWS CodeDeploy application name can be used only once per region. You can reuse an application name in another region though.)
4. In the *Deployment Group* Name box, type a name that describes the deployment group: **testing**.
5. In the *Add Instances* section, set the Tag Type to AutoScaling Group and select your autoscaling group.
6. In the *Deployment Config* list, choose the deployment configuration: **OneAtATime**
7. In the *Service Role ARN* box, choose an Amazon Resource Name (ARN): **arn:aws:iam::00000000000:role/CodeDeploy** 

For more information, visit the [AWS CodeDeploy Deployments](http://docs.aws.amazon.com/codedeploy/latest/userguide/deployment-steps.html) and [Create an Application with AWS CodeDeploy](http://docs.aws.amazon.com/codedeploy/latest/userguide/how-to-create-application.html)

## Set-up continuousphp

### Prepare the Sample Application

Finally, let's configure continuousphp to deploy our [Sample Application](https://github.com/oswaldderiemaecker/aws-demo-zf-apigility-phinx).

As we are going to configure continuousphp in testing environment, we need to create a develop branch, for which we will create a deployment pipeline.

**Preparing our Sample Application**

Create the **develop** branch:

```bash
git checkout -B develop
git push origin develop
```

This application include the following files:

* [composer.json](https://github.com/continuousdemo/aws-demo-zf-apigility-phinx/blob/master/composer.json) with our dependencies (Zend Framework 2.x / Apigility / Phinx)
* [build.xml](https://github.com/continuousdemo/aws-demo-zf-apigility-phinx/blob/master/build.xml) with the phing targets
* [phinx.yml.dist](https://github.com/continuousdemo/aws-demo-zf-apigility-phinx/blob/master/phinx.yml.dist) with the Phinx Database migration configuration
* [behat.yml](https://github.com/continuousdemo/aws-demo-zf-apigility-phinx/blob/master/behat.yml) with the Behat configuration
* [tests/phpunit.xml](https://github.com/continuousdemo/aws-demo-zf-apigility-phinx/blob/master/tests/phpunit.xml) with the PHPUnit configuration
* [appspec.yml](https://github.com/continuousdemo/aws-demo-zf-apigility-phinx/blob/master/appspec.yml) with the CodeDeploy configuration and hooks
* [scripts/cleanup.sh](https://github.com/continuousdemo/aws-demo-zf-apigility-phinx/blob/master/scripts/cleanup.sh) the Appspec clean-up hook
* [scripts/migrate.sh](https://github.com/continuousdemo/aws-demo-zf-apigility-phinx/blob/master/scripts/migrate.sh) the Appspec migrate hook
* [scripts/restart-server.sh](https://github.com/continuousdemo/aws-demo-zf-apigility-phinx/blob/master/scripts/restart-server.sh) the Appspec restart server hook

These are key files to set-up your application installation, testing and deployment. Feel free to take a look at them to get a better understanding.

### Application Project Set-up in continuousphp

**Set-up our application project in continuousphp**

1. Type in the omnibar the name of the application: **aws-demo**
2. The fork should appear in the Project List (if not please wait a little that projects list update or simply logout/login)
3. Click on **Setup**
4. Click on **Setup Repository**
5. Click on **+** to add a deployment pipeline
6. Select the **develop** branch

### Deployment pipeline Configuration

**To configure your deployment pipeline**
 
1. In the Build Settings (Step 1):
   1. In the **PHP VERSIONS**, select the PHP versions: **5.6** / **7.0**
   2. In the **CREDENTIALS**, click on the **+**, add the AWS IAM Credential **deployment.testing** User Access Key and Secret Key we created in step [Set-up the IAM permissions](#set-up-the-iam-permissions)
      * Name: AWSCodeDeployDemo
      * Region: US West (Oregon)
      * Access Key: XXXXXXXXXXXXXXXXXXXXXXXXXXXXX
      * Secret Key: XXXXXXXXXXXXXXXXXXXXXXXXXXXXX
   3. Click on **Next** to move to the Test Settings
2. In the Test Settings (Step 2):
   1. continuousphp automatically discovers that you have a `behat.yml` and `phpunit.xml` in your repository and creates the testing configuration for you.
   2. Click on the **Behat** configuration panel. In the **PHING** section, select the following Phing Targets: **init-db** and **setup**
   3. Still in the **PHING** section, add the following variables: 
      * db.host: 127.0.0.1
      * db.port: 3306
      * db.name: skeleton
      * db.username: root
      * db.password:
   4. Click on **Next** to move to the Package Settings
3. In the Package Settings (Step 3):
   1. Select **AWS Code Deploy**
   2. Click on **Next** to move to the Deployment Settings
4. In the Deployment Settings (Step 4):
   1. Click on **+** on the **DESTINATIONS** panel
   2. Complete the destination:
      * Name: testing
      * Apply to: push
      * IAM Profile: The profile we created in Step 1.2
      * Application: mycompany_app
      * Group: testing
      * S3 Bucket: mycompany-package/testing
   3. Check the **enable deployment for successful builds** checkbox 

## Deploy the app

**Deploy the application with continuousphp**

1. Click on the **Play** button on the top right of the project
2. Select the **develop** branch
3. The build is started. It will create the testing and dist package, then run the tests (Behat and PHPUnit) for the choosen PHP versions, and finally it deploys the application.
4. In the deploy console, you should see **Deployment successfully started**
5. Login to the AWS console/AWS CodeDeploy to see the details.

You can now modify your code with your favorite editor. For example, edit the file module/Application/view/layout/layout.phtml, add some text, save the file, commit and push it:

```bash
git checkout develop
git add module/Application/view/layout/layout.phtml
git commit -m "Modifying the layout"
git push
```

Now, everytime you push to the develop branch, your develop pipeline is triggered and continuousphp builds, tests and deploys your application upon a successful build.

## Notes

* The CloudFormation template used in this tutorial is an example and should not be used for production use. 
* The Amazon Machine Image (AMI) provided in the CloudFormation is for study purposes.

If you like to know more about production configuration or have questions about this tutorial, do not hesitate to contact us using the chat button!
