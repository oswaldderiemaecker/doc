---
layout:         doc
title:          "Virtuoso 7 - Documentation"
category:       "databases"
order:          6
excerpt:        "Virtuoso 7 support by continuousphp"
---

continuousphp uses the [official Virtuoso Docker image](https://hub.docker.com/r/tenforce/virtuoso/). No need to install it yourself!

## Specification
Virtuoso is part of our additional container, That's mean is not automatically available, for enable Virtuoso container you must add specific
environement variable `CPHP_SERVICE_VIRTUOSO_7` to true value in your pipeline settings for each testing target (e.g Phpunit, Behat...)

__The Current versions supported is Virtuoso 7.2.0.__

The Virtuoso settings is default provided by official container.
But in order to allowed you to import your RDF specification, the `DirsAllowed` value has been set to root path "/".

## Connecting to Virtuoso

Host, authentication and default database settings:

<table>
  <tr>
    <td>host:</td><td>virtuoso7</td> 
  </tr>
  <tr>
    <td>username:</td><td>dba</td> 
  </tr>
  <tr>
    <td>password:</td><td>dba</td>
  </tr>
</table>

