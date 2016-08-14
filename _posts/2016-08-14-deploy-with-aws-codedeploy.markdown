---
layout:         post
title:          "Deploy with AWS CodeDeploy"
date:           2016-08-14 15:24:11
author:         'oswaldderiemaecker'
image:          '/assets/2015-03-23/pipelines-share.png'
hasDescription: true
categories:     tutorial
---
This tutorial explains how to configure continuousphp and AWS to deploy with AWS CodeDeploy.

<!--more-->

- [Set-up the AWS environment accounts](#set-up-the-aws-environment-accounts)
- [Set-up the package S3 bucket](#set-up-the-package-s3-bucket)
- [Set-up the IAM permissions](#set-up-the-iam-permissions)

## Set-up the AWS environment accounts

You may plan to deploy your application to differents environments like testing, preproduction, production based on your deployment strategy.
AWS recommends to use the principle of the separation of responsibilities, for this you should create as many AWS account as environment you might requires.

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

To start we are going to create the IAM policy for an IAM user to grant permission to upload the package to your bucket **"mycompany-package-testing"** and communicate with CodeDeploy to deploy it.

**To create the policy for your test user**

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

Now Let's create an IAM user with an Access Key with the policy we've just created.  

1. Sign in to the Identity and Access Management (IAM) console at https://console.aws.amazon.com/iam/.
2. In the navigation pane, choose Users and then choose Create New Users. 
3. Enter the following user: **deployment.testing**
4. Generate an access key this user at this time with select Generate an access key for each user.
5. Choose Create.
6. Attach the policy **testingDeployment** to our user **deployment.testing**. 

For more information, visit the [AWS IAM User documentation](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html) and the [Tutorial: Create and Attach Your First Customer Managed Policy](http://docs.aws.amazon.com/IAM/latest/UserGuide/tutorial_managed-policies.html)

Now you can [start using continuousphp](https://continuousphp.com/)  
We do hope you will enjoy using it for your projects. Any question, don’t hesitate to ask us using the chat button!
