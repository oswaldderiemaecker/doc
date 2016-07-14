---
layout:         doc
title:          "AWS (Amazon Web Services) - Documentation"
category:       "credentials-authentication"
order:          1
excerpt:        "AWS credentials support by continuousphp"
---
You can use Micro-Services from AWS on continuousphp. Simply generate an Access Token and Secret
Access Key in [AWS IAM](https://aws.amazon.com/iam/) and inject them into your pipeline to
access all the services you need on AWS!

<div class="row panel callout warning clearfix">
  <h2 class="left"><i class="fa fa-exclamation-triangle"></i></h2>
  Our containers are equipped with the latest version of the <a href="https://aws.amazon.com/cli/" target="_blank">AWS Command Line Interface</a>. No need to install it yourself!
</div>

![AWS Credentials](/assets/doc/credentials-authentication/aws.png)

The credentials will be injected automatically into each container, so that you can access
Amazon Web Services at each stage of your build. If you want to deploy using
[AWS CodeDeploy](https://aws.amazon.com/codedeploy/) or [AWS Elastic Beanstalk](https://aws.amazon.com/elasticbeanstalk/),
take a look at our documentation section on deployment with [AWS CodeDeploy](/documentation/deployment/aws-code-deploy/)
or [AWS Elastic Beanstalk](/documentation/deployment/aws-elastic-beanstalk/)!
