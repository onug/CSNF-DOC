---
title: 'CSNF Quickstart'
linkTitle: 'CSNF Quickstart'
weight: 60
description: >
  The CSNF Quickstart is an example application that you can use to get up and running quickly with the CSNF Framework.
---

{{% pageinfo %}}
CSNF Quickstart: Coming Soon
{{% /pageinfo %}}

Join our growing <a href="https://csnfonugslackcom.slack.com">Slack</a> community to participate in the CSNF Conversation

# Welcome
This quickstart guide will walk you through the steps to setup the CSNF and its Decorator. 

# Pre-requisites
Verify that you have nodejs, NPM package manager and Typescript installed. You can do this by verifying from the command line.

```bash
node --version v16.11.0
npm --version 8.0.0
tsc -- version Version 3.8.3
```

# Install the CSNF and all dependencies locally

Clone the from the CSNF repository. THis contains the csnf core modules and a demo called demo-service

```bash
git clone https://github.com/onug/CSNF.git
```
Next change to the csnf directory and run npm install to install the dependencies, and also install the node static types.

```bash
npm install
npm i @types/node
```

Once all the dependencies are installed you can build the csnf framework.

```bash
npm run build
npm run test
npm run coverage
npm run prerelease
```

When you are done you should have a ./dist directory containing compiled ouptut.
Now cd to the demo-service directory and compile the typescript by running the tsc command

```bash
tsc
```
Now you can start the server using npm. You should see some csnf dictionaries registering in system output along with an http listener.

```bash
npm run start
[2021-11-01T07:17:34.692] [INFO] server - listening on http://localhost:3000
```
