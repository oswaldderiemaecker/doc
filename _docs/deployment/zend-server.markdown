---
layout:         doc
title:          "Zend Server Deployment - Documentation"
category:       "deployment"
logo:           zend-server
order:          2
excerpt:        "Deploy your applications to one or more Zend Servers."
---

[Zend Server](http://www.zend.com/en/products/zend_server) allows you to deploy your applications to one or more Zend Servers.

## Configure the deployment
To configure the deployment, you only need to specify the Zend Server URL, the application URL and the Zend Server API credentials. **Don't forget to add the port ":10081" or ":10082" to the server URL!** Otherwise, the deployment will most certainly fail.

![Zend Server configuration](/assets/doc/deployment/zend-server/configuration.png)

## Add the deployment.xml and deployment.properties files to your repository
Zend Server needs to know, how to deploy your application (source folder, destination folder, scripts to execute, ...).
To do this, you must add the **deployment.xml** and **deployment.properties** files at the root of your repository.
The documentation on how to write these files can be found in the [Zend Server documentation](http://files.zend.com/help/Zend-Server/content/the_xml_descriptor_file.htm).

### Create a deployment.xml file

Below you can find an example of a **deployment.xml** file:

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<!-- @See: http://files.zend.com/help/Zend-Server-7/zend-server.htm#understanding_the_package_structure.htm -->
<package xmlns="http://www.zend.com/server/deployment-descriptor/1.0" version="1.0">
    <type>application</type>
    <name>API</name>
    <summary>MyApp API</summary>
    <description>MyApp description ...</description>
    <version>
        <release>dev-master</release>
    </version>
    <appdir/>
    <docroot>public</docroot>
    <scriptsdir>scripts</scriptsdir>
</package>
```

### Create a deployment.properties file

Below you can find an example of a **deployment.properties** file:

```ini
appdir.includes = config,\
		data,\
		module,\
		public,\
		vendor,\
		build.properties,\
		migrate.properties,\
		composer.json,\
		composer.lock,\
		README.md,\
		scripts
scriptsdir.includes = scripts/deph.php,\
        scripts/pre_activate.php,\
        scripts/post_activate.php
```

## Zend Server Deployment Helper

The [Zend Server Deployment Helper](https://github.com/zend-patterns/ZendServerDeploymentHelper) is a Zend Framework 2 based set of classes which supports in creating reliable hook scripts for Zend Server Deployment.

**IMPORTANT:** Make sure that the *scripts/ZendDevOps* is NOT listed in the *scriptsdir.includes*, but instead add the *scripts* folder into *appdir.includes* section.


And that's it! You just configured your application to be deployed with Zend Server.