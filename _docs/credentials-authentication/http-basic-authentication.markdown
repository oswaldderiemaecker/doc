---
layout:         doc
title:          "HTTP Basic Authentication - Documentation"
category:       "credentials-authentication"
order:          3
excerpt:        "HTTP Basic Authentication support by continuousphp"
---
If you have private repositories on Bitbucket/Gitlab or a Satis/Toran Proxy with private dependencies,
you can use HTTP Basic Authentication to access them during your build. Simply go to the *Build Settings*
of your pipeline configuration and enter your HTTP Basic credentials to make your dependencies available
during your build.

![HTTP Basic Authentication](/assets/doc/credentials-authentication/http-basic-authentication.png)