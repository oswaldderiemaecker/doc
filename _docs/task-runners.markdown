---
layout:         doc-toc
title:          "Task runners - Documentation"
category:       "taskrunners"
logo:           task-runner
order:          10
excerpt:        "Task runners supported by continuousphp."
---
In DevOps practices, task runners are very important.

At this point, continuousphp supports [Phing](https://www.phing.info) and native shell scripts. However,
other task runners like Gulp, Grunt, make... will be supported soon.

<div class="row panel callout warning clearfix">
  <h2 class="left"><i class="fa fa-exclamation-triangle"></i></h2>
  Always keep in mind that every build is split into different containers. Then, pay attention to define the tasks to run
  at the right stage of your build (provisioning, test or packaging) and never interact with database dependencies
  during a provisioning or packaging stage. This data won't be available in your testing containers!
</div>
