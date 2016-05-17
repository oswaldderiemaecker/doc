---
layout:         doc
title:          "Solr - Documentation"
category:       "indexing"
order:          1
excerpt:        "Solr support by continuousphp"
---
[Solr](http://lucene.apache.org/solr/) is supported by continuousphp containers.

## Specification 

Our containers run Solr 5.5.0.

## Building your index

A Drupal example

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project name="CPHP Solr5 Phing Tasks" default="build" basedir=".">
<target name="build-index">
   <echo message="Building Solr index"/>
   <exec dir="${project.basedir}"
         command="sudo curl -o /opt/solr/search_api_solr-7.x-1.10.tar.gz https://ftp.drupal.org/files/projects/search_api_solr-7.x-1.10.tar.gz;
sudo tar xvzf /opt/solr/search_api_solr-7.x-1.10.tar.gz -C /opt/solr;
sudo /opt/solr/bin/solr create_core -c drupal;
sudo mkdir /opt/solr/server/solr/drupal;
sudo rsync -avz /opt/solr/search_api_solr/solr-conf/5.x/ /opt/solr/server/solr/drupal/conf/"
         checkreturn="true"
         passthru="true"/>
</target>

<target name="restart-solr">
   <exec dir="${project.basedir}"
         command="sudo /opt/solr/bin/solr restart"
         checkreturn="true"
         passthru="true"
         spawn="true"/>
</target>

<target name="solr-status">
   <exec dir="${project.basedir}"
         command="sudo /opt/solr/bin/solr status"
         checkreturn="true"
         passthru="true"/>
</target>

<target name="solr-query-test">
   <exec dir="${project.basedir}"
         command="sudo curl http://localhost:8983/solr/drupal/select?q=tid:0"
         checkreturn="true"/>
</target>
</project>
```
