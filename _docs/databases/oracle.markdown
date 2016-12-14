---
layout:         doc
title:          "Oracle XE - Documentation"
category:       "databases"
order:          5
excerpt:        "Oracle XE support by continuousphp"
---

continuousphp uses the [official Oracle Linux Docker image](https://hub.docker.com/_/oraclelinux/). No need to install it yourself!

## Specification
Oracle is part of our additional container, That's mean is not automatically available, for enable Oracle container you must add specific
environement variable `CPHP_SERVICE_ORACLE_XE_11` to true value in your pipeline settings for each testing target (e.g Phpunit, Behat...)

__The Current versions supported is oracle express edition 11.__
__Our php testing containers implement the OCI8 extensions.__

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
Your build will cloud take longer than habit to launch because the Oracle container need some time to prepare the database and tablespace.

