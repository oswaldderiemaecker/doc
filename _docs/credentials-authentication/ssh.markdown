---
layout:         doc
title:          "SSH Keys - Documentation"
category:       "credentials-authentication"
order:          2
excerpt:        "SSH Key support by continuousphp"
---
If you have private repositories on Bitbucket/Gitlab or a Satis/Toran Proxy with private dependencies,
you can use SSH Keys to access them during your build. Simply go to the *Build Settings* of your pipeline
configuration and enter your private SSH Keys to make them available in every container of your build. 

![SSH Keys](/assets/doc/credentials-authentication/ssh.png)