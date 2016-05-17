---
layout:         doc
title:          "phpbrew - Documentation"
category:       "php"
order:          9
excerpt:        "phpbrew support by continuousphp"
---
continuousphp use [phpbrew](https://github.com/phpbrew/phpbrew), most extensions are pre-compiled, if you need to
install a missing extension, for the selected PHP version(s) of your project pipeline, use a Phing target as below.

By example, to install a specific PECL extension, use the following Phing target:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<project name="contexts">
    <target name="install-uopz-php-ext" description="Install uopz PECL extension">
      <exec command="phpbrew ext install uopz" passthru="true"/>
    </target>
</project>
```
