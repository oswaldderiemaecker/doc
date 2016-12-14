---
layout:         doc
title:          "Virtuoso 7 - Documentation"
category:       "databases"
order:          6
excerpt:        "Virtuoso 7 support by continuousphp"
---

continuousphp uses the [official Virtuoso Docker image](https://hub.docker.com/r/tenforce/virtuoso/).

## Specification

A Virtuoso 7 container is available for each activity in your build. To enable it, simply add an environment variable `CPHP_SERVICE_VIRTUOSO_7` with an arbitrary value in your pipeline configuration. E.g. if you need Virtuoso in your Behat tests, go to the Testing Settings (step 2 of the pipeline) and add the environment variable to the Behat configuration.

__The currently supported version is Virtuoso 7.2.0.__

The default Virtuoso settings are those of the official container.
But to allow you to import your RDF specification, the `DirsAllowed` value has been set to the root path "/".

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

