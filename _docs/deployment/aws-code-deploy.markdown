---
layout:         doc
title:          "AWS CodeDeploy - Documentation"
category:       "deployment"
logo:           aws-code-deploy
order:          3
excerpt:        "Deploy your applications to AWS EC2 in a few clicks."
---

[AWS CodeDeploy](https://aws.amazon.com/codedeploy) allows you to deploy your applications directly to Amazon EC2 instances.

## Specify your AWS credentials
To deploy to your AWS account, continuousphp needs an IAM key and secret. You can specify them in the 1st step of the project
setup or simply when adding a new pipeline to your existing project.

![AWS IAM credentials](/assets/doc/deployment/aws-code-deploy/iam-credentials.png)

## Configure the Packaging
After having configured the tests in the second step, you can now enable AWS CodeDeploy in the Packaging configuration.

![AWS packing](/assets/doc/deployment/aws-code-deploy/packaging.png)

## Configure the Deployment
Once you have added the IAM credentials and you defined AWS CodeDeploy as packaging method, you can configure the deployment in the 4th step
of the project/pipeline setup.

![AWS deployment](/assets/doc/deployment/aws-code-deploy/destination.png)

## Add the appspec.yml file to your repository
AWS CodeDeploy needs to know, how to deploy your application (source folder, destination folder, scripts to execute, ...).
To do this, you must add an **appspec.yml** file at the root of your repository. The documentation on how to write this file
can be found in the [AWS CodeDeploy documentation](http://docs.aws.amazon.com/codedeploy/latest/userguide/app-spec-ref.html).

**IMPORTANT:** Make sure that the IAM credentials you provide only have the strict minimum of permissions. Example: The
provided IAM credentials shouldn't be able to erase your S3 storage.

And that's it! You just configured your application to be deployed with AWS CodeDeploy.