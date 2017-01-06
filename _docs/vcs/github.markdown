---
layout:         doc
title:          "GitHub - Documentation"
category:       "vcs"
logo:           git-hub
order:          2
excerpt:        "GitHub support on continuousphp."
---

Find below a description of how to use continuousphp with [GitHub](https://github.com).

## Link your account
To link your account to continuousphp, you just need to log in using GitHub.

![GitHub login](/assets/doc/vcs/login.png)

Once you're logged in to GitHub, you'll need to authorize continuousphp to access your data. This is a requirement
in order to use continuousphp.

![GitHub authorization](/assets/doc/vcs/github/authorize.png)

## Add your organizations
Once your GitHub account is linked to continuousphp, you can access all your repositories, including
your organization repositories, directly from continuousphp. However, if you can't see repositories of a specific organization, this organization
probably enabled the third-party application access restriction policy. In this case, you need to grant continuousphp access to
your organization:

1. Go to your GitHub personal settings > Applications.
2. Click on the "View" button.

![Applications](/assets/doc/vcs/github/applications.png)  

3. Then click on the "Grant access" button.

![Grant access](/assets/doc/vcs/github/grant.png)  

## Setup your repository
To setup an existing repository, you just need to select it from the omnibar and follow the setup instructions.
Once it's done, you can update your settings through the project page.

During the repository setup, continuousphp will create a deployment key and a hook for your project. If you have removed
this hook or the deployment key, you can recreate it through the "Reset hook" button in the project page.

![Reset hook](/assets/doc/vcs/reset-hook.png)

## Pull Requests
Any pull request made for a configured branch will start a new build using the specific Continuous Deployment Pipeline.
Keep in mind that the build of a pull request will create a deployment package (available through the API) but won't
execute the deployment step.

## GitHub Access Token
Your GitHub Token is used for all requests to the GitHub API (Cloning of the repository, installation of dependencies, ...) but
you can also use it directly by getting it from the environment variable **GITHUB_TOKEN**.
