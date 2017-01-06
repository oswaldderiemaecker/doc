---
layout:         doc
title:          "Bitbucket - Documentation"
category:       "vcs"
logo:           bitbucket
order:          3
excerpt:        "Bitbucket support on continuousphp"
---

Find below a description of how to use continuousphp with [Bitbucket](https://bitbucket.org).

## Link your account
To link your account to continuousphp, you just need to log in using Bitbucket.

![Bitbucket login](/assets/doc/vcs/login.png)

Once you're logged in to Bitbucket, you'll need to authorize continuousphp to access your data. This is a requirement
in order to use continuousphp.

![Bitbucket authorization](/assets/doc/vcs/bitbucket/authorize.png)

## Add your teams
Once your Bitbucket account is linked to continuousphp, you can access all your repositories including
your team repositories.

## Setup your repository
To set up an existing repository, you just need to select it from the omnibar and follow the setup instructions.
Once it's done, you can update your settings through the project page.

When a repository is setup, continuousphp will create a deployment key and a hook for your project. If you have removed
this hook or the deployment key, you can recreate it through the "Reset hook" button in the project page.

![Reset hook](/assets/doc/vcs/reset-hook.png)

<div class="row panel callout warning clearfix">
  <h2 class="left"><i class="fa fa-exclamation-triangle"></i></h2>
  Due to an old
  <a href="https://bitbucket.org/master/issue/5938/include-branch-and-tag-information-for" target="_blank">issue</a>
  on Bitbucket, if you use the old (deprecated) "POST services" to notify continuousphp about a push, we are unable to automatically
  start a new build when a tag is pushed.
  To fix this, simply hit the "Reset Hook" button in the project page. The old deprecated hook will be replaced by the new Bitbucket
  Webhook.
</div>

## Pull Requests
continuousphp supports Bitbucket Pull Requests. You can make Pull Requests between different forks or between different branches of
the same repository/fork.
