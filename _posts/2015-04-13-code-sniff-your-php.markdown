---
layout:         post
title:          "Code Sniff your PHP!"
date:           2015-04-13 08:00:00
author:         'fdewinne'
categories:     news
---
It's finally available!  
You can now check your code with [PHP_CodeSniffer](https://github.com/squizlabs/PHP_CodeSniffer)!

<!--more-->
Go to the settings of your project and simply add the PHP CS test activity in your continuousphp Deployment Pipeline
with the folder(s) you want to test. Optionally, you can add a phpcs.xml file at the root of your project
to use your own custom ruleset (see the
[PHP_CodeSniffer documentation](https://github.com/squizlabs/PHP_CodeSniffer/wiki/Advanced-Usage#using-a-default-configuration-file)
for more informations).
