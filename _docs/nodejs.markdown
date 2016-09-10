---
layout:         doc-toc
title:          "Node.js Versions - Documentation"
category:       "nodejs"
logo:           nodejs 
order:          14
excerpt:        "Node.js versions supported by continuousphp."
---
continuousphp supports several Node.js versions.

Currently supported Node.js versions:

* v5.4.1 
* v4.2.4 (default version)
* v4.2.3
* v4.2.2
* v4.2.1
* v4.2.0
* v0.12.9

You can switch the Node.js version through [nvm](https://www.npmjs.com/package/nvm) and a Phing target in your *build.xml*:

```xml
<target name="npm-exec">
   <echo message="Install dependencies"/>
   <exec dir="${project.basedir}"
         command="nvm list ; nvm use v4.2.1 ; nvm version ; npm install"
         checkreturn="true"
         passthru="true"/>
</target>
```
