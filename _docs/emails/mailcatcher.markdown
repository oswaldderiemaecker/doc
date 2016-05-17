---
layout:         doc
title:          "MailCatcher - Documentation"
category:       "emails"
logo:           mailcatcher
order:          1
excerpt:        "Handle emails during tests with MailCatcher."
---

[MailCatcher](http://mailcatcher.me/) is a tool that makes Email handling during automated tests very easy, simply by redirecting emails to it's built-in SMTP server. 

## Installation
MailCatcher is pre-installed on all continuousphp containers. You can start it by adding a Phing target to your *build.xml* file:

```xml
<target name="start-mailcatcher">
    <echo message="Start the mailcatcher"/>
    <exec dir="${project.basedir}"
          command="mailcatcher"
          checkreturn="true"
          passthru="true"/>
</target>
```

**IMPORTANT:** Because every activity on continuousphp runs in a different container, you need to start MailCatcher in the testing step of your pipeline. Go to step 2 and open the configuration of your testing framework. There you can add Phing targets that will run just before the tests start. If you start MailCatcher during the creation of the testing or deploy package (step 1 or 3 in the pipeline config), MailCatcher won't be availble during the tests. 

## Configuration
To use MailCatcher you have to tell your application to use smtp://localhost:1025 to send all it's Emails.