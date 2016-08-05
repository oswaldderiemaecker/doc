---
layout:         doc
title:          "PhantomJS - Documentation"
category:       "browser-ui-testing"
order:          1
excerpt:        "PhantomJS support by continuousphp"
---
continuousphp supports [PhantomJS](http://phantomjs.org/). It is a powerful headless Browser with a JavaScript API. It
has fast and native support for various web standards: DOM handling, CSS selector, JSON, Canvas, and SVG.

<div class="row panel callout warning clearfix">
  <h2 class="left"><i class="fa fa-exclamation-triangle"></i></h2>
  Our containers are pre-installed with the version 2.1.1 of PhantomJS. No need to install it yourself!
</div>

## Installation & Usage
PhantomJS runs in every container on continuousphp as a daemon. You can reach it by using the address `http://127.0.0.1:8643`.

Check our documentation pages for examples:

* [UI Testing with Behat, Selenium & PhantomJS](/documentation/testing/behat#ui-testing-with-selenium-and-phantomjs)