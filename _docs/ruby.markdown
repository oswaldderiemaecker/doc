---
layout:         doc-toc
title:          "Ruby Versions - Documentation"
category:       "ruby"
logo:           ruby
order:          12
excerpt:        "Ruby versions supported by continuousphp."
---
continuousphp supports several Ruby versions.

Currently supported Ruby versions:

* 1.9.3-p551
* 2.0.0-p645
* 2.1.6
* 2.2.2

The 2.2.2 is the default.

You can switch Ruby version through [rbenv](http://rbenv.org/) and a Phing task in your build.xml as following:

```xml
<target name="bundler-exec">
   <echo message="Install dependencies"/>
   <exec dir="${project.basedir}"
         command="rbenv versions ; rbenv local 1.9.3-p551 ; rbenv version ; bundle install"
         checkreturn="true"
         passthru="true"/>
</target>
```
