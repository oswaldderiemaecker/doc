---
layout:         doc
title:          "GitLab - Documentation"
category:       "vcs"
logo:           gitlab
order:          4
excerpt:        "GitLab support on continuousphp"
---

Find below a description of how to use continuousphp with [GitLab](https://gitlab.com).

## Link your account
To link your account to continuousphp, you just need to log in using GitLab.

![GitLab login](/assets/doc/vcs/login.png)

Once you're logged in to GitLab, you'll need to authorize continuousphp to access your data. This is a requirement
in order to use continuousphp.

![GitLab authorization](/assets/doc/vcs/gitlab/authorize.png)

## Add your groups
Once your GitLab account is linked to continuousphp, you can access all your repositories including
your group repositories.

## Setup your repository
To set up an existing repository, you just need to select it from the omnibar and follow the setup instructions.
Once it's done, you can update your settings through the project page.

When a repository is setup, continuousphp will create a deployment key and a hook for your project. If you have removed
this hook or the deployment key, you can recreate it through the "Reset hook" button in the project page.

![Reset hook](/assets/doc/vcs/reset-hook.png)

## Merge Requests
continuousphp supports GitLab Merge Requests. You can make Merge Requests between different forks or between different branches of
the same repository/fork.