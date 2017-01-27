---
layout:         doc
title:          "MongoDB - Documentation"
category:       "nosql"
order:          1
excerpt:        "MongoDB support by continuousphp"
---
[MongoDB](https://www.mongodb.org/) is supported by continuousphp. It uses the [official MongoDB Docker images](https://hub.docker.com/_/mongo/).

## Specification 

MongoDB containers are available for each activity in your build. To enable one of them, simply add one of the following
environment variables with an arbitrary value in your pipeline configuration:

* ***CPHP_SERVICE_MONGO_24*** (latest 2.4.x)
* ***CPHP_SERVICE_MONGO_3*** (latest 3.x)
* ***CPHP_SERVICE_MONGO_30*** (latest 3.0.x)
* ***CPHP_SERVICE_MONGO_32*** (latest 3.2.x)
* ***CPHP_SERVICE_MONGO_34*** (latest 3.4.x)

E.g. if you need `MongoDB 3.4.x` in your Behat tests, go to the Testing Settings (step 2 of the Pipeline) and add the
environment variable `CPHP_SERVICE_MONGO_32 = 1` to the Behat configuration.

### PHP Extensions

Our PHP testing containers implement both the `mongo` and `mongodb` extensions.

## Connecting to MongoDB

There are no Authentication settings.
