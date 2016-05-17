---
layout:     policy
title:      "Security Policy"
excerpt:    "Discover how continuousphp takes care about security"
permalink:  /security-policy/
---

This Site offers access to continuousphp or any other Continuous’s proprietary tools (as described herein and variously throughout, the “Continuous Services”).

##Overview

We fully understand and recognize, that the security of your source code and configuration data is important, as it
forms the base of your and our endeavours. Therefore we put a lot of effort and thought into providing a secure
infrastructure for you to use.

##System Security

For every project you add to the Continuous Services we create an SSH Key that is itself encrypted strongly and only
decrypted shortly before being used in the build virtual container. For every build we start a new and clean virtual
container. All changes you make (including file system changes) are stored in an ephemeral container which is removed as
soon as your build finishes (pack, tests and deployment). For your convenience, your builds are stored in a secured S3
bucket for future needs (e.g. roll back, code analysis). We highly recommend not to use production and/or any sensitive
data in your builds.   
All communication between your browser and our website is SSL encrypted, so is all communication to our queue system.
All communication to the build virtual container is done over SSH.

##Continuous Team won't read your code

Continuous will never read your code without your consent. This might happen in case of a support request or if you want
something debugged by our engineers on your end. Never will a person from outside the Continuous Team works on a support
request you submit.

##Authorization for your source code repository

To build your application, we need to check out your code from your source code repository. Currently we support GitHub,
Bitbucket, Atlassian Stash, with others coming soon. You sign up for the Continuous Services via your source code
repository provider as soon as you connect a repository with the Continuous Services. When connecting Continuous
Services, you must tell your source code repository provider that you allow us to check out your private repositories.  
You can revoke permission in your source code repository provider settings
([GitHub Settings](https://github.com/settings/applications), Bitbucket Settings...) and by removing the continuousphp's
authorized application keys and service hooks from your continuousphp projects' configuration pages.

##Services

Our whole infrastructure is based on Amazon Web Services or services built on top of it. Amazon Web Services is one of
the most trusted, tried and tested hosting services out there (read more about their
[Security Policy](http://aws.amazon.com/security/)). The services we use are (non-exhaustive list):

- Amazon EC2 
- Amazon S3 

Additionally for collecting Metrics (but without any sensitive data) we use (non-exhaustive list):

- Google Analytics
- Zoho suite

##Source code access

As outlined in our [Terms of Service](https://continuousphp.com/terms-of-service/) we only access your source code for
a build or support request. We do not have any way to access your repository outside of our build environment.

##Content access

You understand that the technical processing and transmission of the Continuous Services, including your content, may be
transferred and involve:  
(a) transmissions over various networks; and   
(b) changes to conform and adapt to technical requirements of connecting networks or devices.

Continuous implements security procedures to help protect your data from security attacks. However, you understand that
the use of the Continuous Services necessarily involves transmission of your data over networks that are not owned,
operated or controlled by us, and we are not responsible for any of your data lost, altered, intercepted or stored
across such networks. We cannot guarantee that our security procedures will be error-free, that transmissions of your
data will always be secure or that unauthorized third parties will never be able to defeat our security measures or
those of our third party service providers.

##Contact

If you have any further questions you can send an email to [info@continuousphp.com](mailto:info@continuousphp.com)
