---
layout:         doc
title:          "AWS Elastic Beanstalk - Documentation"
category:       "deployment"
logo:           aws-elastic-beanstalk
order:          4
excerpt:        "Deploy your applications to AWS EC2 in a few clicks."
---

[AWS Elastic Beanstalk](https://aws.amazon.com/elasticbeanstalk) allows you to deploy your applications directly to Amazon EC2 instances.

## Specify your AWS credentials
To deploy to your AWS account, continuousphp needs an IAM key and secret. You can specify them in the 1st step of the project
setup or simply when adding a new pipeline to your existing project.

![AWS IAM credentials](/assets/doc/deployment/aws-elastic-beanstalk/iam-credentials.png)

## Configure the Packaging
After having configured the tests in the second step, you can now enable AWS Elastic Beanstalk in the Packaging configuration.

![AWS packing](/assets/doc/deployment/aws-elastic-beanstalk/packaging.png)

## Configure the Deployment
Once you have added the IAM credentials and you defined AWS Elastic Beanstalk as packaging method, you can configure the deployment in the 4th step
of the project/pipeline setup.

![AWS deployment](/assets/doc/deployment/aws-elastic-beanstalk/destination.png)

For more information on how to use AWS Elastic Beanstalk, check out the [documentation](https://aws.amazon.com/documentation/elastic-beanstalk/).

**IMPORTANT:** Make sure that the IAM credentials you provide only have the strict minimum of permissions. Example: The
provided IAM credentials shouldn't be able to erase your S3 storage.

And that's it! You just configured your application to be deployed with AWS Elastic Beanstalk.