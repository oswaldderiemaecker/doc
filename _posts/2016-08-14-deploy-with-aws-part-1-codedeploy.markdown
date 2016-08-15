---
layout:         post
title:          "Deploy with AWS - Part 1 : CodeDeploy"
date:           2016-08-14 15:24:11
author:         'oswaldderiemaecker'
image:          '/assets/2015-03-23/pipelines-share.png'
hasDescription: true
categories:     tutorial
---
This tutorial explains how to configure continuousphp and AWS to deploy an PHP application with AWS CodeDeploy.

<!--more-->

- [Set-up the AWS environment accounts](#set-up-the-aws-environment-accounts)
- [Set-up the package S3 bucket](#set-up-the-package-s3-bucket)
- [Set-up the IAM permissions](#set-up-the-iam-permissions)
- [Set-up the PHP EC2 autoscaling](#set-up-the-php-ec2-autoscaling)
  - [Set-up the infrastructure](#set-up-the-infrastructure)
  - [Set-up the AWS CodeDeploy Agent](#set-up-the-aws-codedeploy-agent)
- [Set-up CodeDeploy](#set-up-codedeploy)
- [Set-up continuousphp](#set-up-continuousphp)
- [Deploy the apps](#deploy-the-apps)
- [Notes](#notes)

## Set-up the AWS environment accounts

Based on your deployment strategy you may plan to deploy your application in differents environments like testing, preproduction and production.
AWS recommends the separation of responsibilities, for this you should create as many AWS account as environment you might require.

This tutorials explain the deployment in a testing environment. You can use it as a base for any other environments you might need.

So let's start and create an AWS account for your testing environment.

**To set up a new account:**

1. Open [https://aws.amazon.com/](https://aws.amazon.com/), and then choose Create an AWS Account.
2. Follow the online instructions.

## Set-up the package S3 bucket

You are now ready to create the S3 bucket in which the continuousphp package will be uploaded for AWS CodeDeploy.

**To create a bucket**

1. Sign into the AWS Management Console and open the Amazon S3 console at https://console.aws.amazon.com/s3.
2. Click Create Bucket.
3. In the Create a Bucket dialog box, in the Bucket Name box, enter the bucket name, in this example we will use **"mycompany-package-testing"**.
4. In the Region box, select a region from the drop-down list.
5. Click Create.

When Amazon S3 successfully creates your bucket, the console displays your empty bucket **"mycompany-package-testing"** in the Buckets panel. 

For more information, visit the [AWS S3 Bucket documentation](http://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html)

## Set up the IAM permissions

To start we are going to create an IAM policy to grant continuousphp the permission to upload the package to your bucket **"mycompany-package-testing"** and communicate with CodeDeploy to deploy it.

**To create the User policy**

1. Sign in to the IAM console at https://console.aws.amazon.com/iam/ with your user that has administrator permissions.
2. In the navigation pane, choose Policies.
3. In the content pane, choose Create Policy.
4. Next to Create Your Own Policy, choose Select.
5. For Policy Name, type **testingDeployment**.
6. For Policy Document, paste the following policy.

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
            "Resource": "arn:aws:s3:::mycompany-package-testing/*"
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

7\. Choose Validate Policy and ensure that no errors display in a red box at the top of the screen. Correct any that are reported.

8\. Choose Create Policy.

Now let's create an IAM user with an Access Key and attach the policy we've just created.  

**To create a new User and attach the User policy**

1. Sign in to the Identity and Access Management (IAM) console at https://console.aws.amazon.com/iam/.
2. In the navigation pane, choose Users and then choose Create New Users. 
3. Enter the following user: **deployment.testing**
4. Generate an access key this user at this time with select Generate an access key for each user.
5. Choose Create.
6. Attach the policy **testingDeployment** to our user **deployment.testing**. 

And now let's create the CodeDeploy policy which grant to CodeDeploy the permissions to get informations about the infrastructure EC2 Instance.

**To create the CodeDeploy policy**

1. Sign in to the IAM console at https://console.aws.amazon.com/iam/ with your user that has administrator permissions.
2. In the navigation pane, choose Policies.
3. In the content pane, choose Create Policy.
4. Next to Create Your Own Policy, choose Select.
5. For Policy Name, type **CodeDeploy**.
6. For Policy Document, paste the following policy.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "autoscaling:CompleteLifecycleAction",
        "autoscaling:DeleteLifecycleHook",
        "autoscaling:DescribeAutoScalingGroups",
        "autoscaling:DescribeLifecycleHooks",
        "autoscaling:PutLifecycleHook",
        "autoscaling:RecordLifecycleActionHeartbeat",
        "ec2:DescribeInstances",
        "ec2:DescribeInstanceStatus",
        "tag:GetTags",
        "tag:GetResources"
      ],
      "Resource": "*"
    }
  ]
}
```
7\. Choose Validate Policy and ensure that no errors display in a red box at the top of the screen. Correct any that are reported.

8\. Choose Create Policy.

And finally let's create the CodeDeploy Role and attach the CodeDeploy policy.

**To create a new Role and attach the CodeDeploy policy**

1. Sign in to the Identity and Access Management (IAM) console at https://console.aws.amazon.com/iam/.
2. In the navigation pane, choose Roles and then choose Create New Roles. 
3. Enter the following user: **CodeDeploy**
5. Choose Create.
6. Attach the policy **CodeDeploy** to our **CodeDeploy** role. 

For more information, visit the [AWS IAM User documentation](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html) and the [Tutorial: Create and Attach Your First Customer Managed Policy](http://docs.aws.amazon.com/IAM/latest/UserGuide/tutorial_managed-policies.html)

## Set-up the PHP EC2 autoscaling

We have set-up all the IAM permissions needed for continuousphp to put its builds package to the S3 bucket, and trigger a deployment with CodeDeploy. Also CodeDeploy has a Role that grant it access to your EC2 informations like Instance IDs, Tags, Autoscaling.

Let's create our infrastructure, for this we will use a CloudFormation template.
 
### Set-up the infrastructure

### Set-up the AWS CodeDeploy Agent 

## Set-up CodeDeploy

To set up CodeDeploy follow the following steps:

1. Sign in to the AWS Management Console and open the AWS CodeDeploy console at https://console.aws.amazon.com/codedeploy.
2. If the Applications page does not appear, on the AWS CodeDeploy menu, choose Applications.
3. Choose Create New Application.
4. In the Application Name box, type the application's name: **mycompany_app**. (In an AWS account, an AWS CodeDeploy application name can be used only once per region. You can reuse an application name in different regions.)
5. In the Deployment Group Name box, type a name that describes the deployment group: **testing**.
6. In the list of tags, select the tag type and fill in the Key and Value boxes with the value of the key-value pair you will use to tag the instances.
As you begin adding key-value pair information, a new row appears for you to add another key-value pair if desired. You can repeat this step for up to 10 key-value pairs.
7. In the Deployment Config list, choose the deployment configuration: **OneAtATime**
8. In the Service Role ARN box, choose an Amazon Resource Name (ARN): **arn:aws:iam::00000000000:role/CodeDeploy** 

For more information, visit the [AWS CodeDeploy Deployments](http://docs.aws.amazon.com/codedeploy/latest/userguide/deployment-steps.html) and [Create an Application with AWS CodeDeploy](http://docs.aws.amazon.com/codedeploy/latest/userguide/how-to-create-application.html)

## Set-up continuousphp

## Deploy the apps 

## Notes

Now you can [start using continuousphp](https://continuousphp.com/)  
We do hope you will enjoy using it for your projects. Any question, don’t hesitate to ask us using the chat button!
