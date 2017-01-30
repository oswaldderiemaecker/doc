---
layout:         doc
title:          "Oracle XE - Documentation"
category:       "databases"
order:          5
excerpt:        "Oracle XE support by continuousphp"
---

continuousphp uses the [official Oracle Linux Docker image](https://hub.docker.com/_/oraclelinux/).

## Specification

An Oracle XE 11 container is available for each activity in your build. To enable it, simply add an environment variable `CPHP_SERVICE_ORACLE_XE_11` with an arbitrary value in your pipeline configuration. E.g. if you need Oracle in your Behat tests, go to the Testing Settings (step 2 of the pipeline) and add the environment variable to the Behat configuration.

__The currently supported version is Oracle Express Edition 11.__
__Our PHP testing containers implement the OCI8 extension.__

## Connecting to Oracle-XE-11

Host, authentication and default database settings:

<table>
  <tr>
    <td>host:</td><td>oracle-xe-11</td> 
  </tr>
  <tr>
    <td>database:</td><td>XE</td> 
  </tr>
  <tr>
    <td>username:</td><td>ORACLE</td> 
  </tr>
  <tr>
    <td>password:</td><td>ORACLE</td>
  </tr>
</table>


## Caution

<div class="row panel callout warning clearfix">
 <h2 class="left"><i class="fa fa-exclamation-triangle"></i></h2>
 By activating Oracle XE, your build will take more time because the container needs some time to prepare the database and the tablespace.
</div>

