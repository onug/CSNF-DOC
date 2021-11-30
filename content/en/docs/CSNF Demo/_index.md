---
title: 'CSNF demo-service'
linkTitle: 'CSNF demo-service'
weight: 60
description: >
  The CSNF demo-service is a typescript application that provides minimal functionality to allow security researchers and security developers to become familiar with the CSNF Decorator. The application can be easily deployed using Docker. 
---

{{% pageinfo %}}
This page explains how to configure the demo-service in your environment.
{{% /pageinfo %}}

Join our growing <a href="https://csnfonugslackcom.slack.com">Slack</a> community to participate in the CSNF Conversation

# CSNF demo environment

### About the demo-service application
The csnf demo-service now runs in docker. The best place to go for information on how to configure and run the CSNF demo environment would be to the github repo. To make use of the repo, you'll need to clone it locally - if you don't know how to do that, gee the Github [Cloning a repository](https://docs.github.com/en/github/creating-cloning-and-archiving-repositories/cloning-a-repository) documentation.

### Pre-requisites for the demo-service
The demo service requires access to a Splunk environment, specifically an http event collector, or HEC, to catch events from the CSNF dispatcher. In order to run the demo-service you will need to provide the access token that can be used by the Splunk HEC.

The demo service also requires access to an http client, like Postman or Insomnia that can be used to send http requests containing JSON payloads.  

### How to configure the demo-service on your laptop
The CSNF demo-service [README.md](https://github.com/onug/CSNF/blob/main/demo-service/README.md) goes into greater detail on how to configure the demo environment.

### Contributing to the CSNF demo-service
If you want to contribute to the repo, see the [CONTRIBUTING.md](https://github.com/onug/CSNF/blob/main/demo-service/CONTRIBUTING.md) file.

### CSNF demo-service Change History
To see what's changed in the current version, see [CHANGES.md](https://github.com/onug/CSNF/blob/main/demo-service/CHANGES.md)