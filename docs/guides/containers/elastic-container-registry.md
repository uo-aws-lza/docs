---
title: Elastic Container Registry
parent: Containers
---

# Overview

Let's set up an Elastic Container Registry (ECR) to host our private docker images for use with other services such as ECS.


## Prerequisites

Make sure you've configured your CLI using our [aws-cli guide](../../getting-started/aws-cli.md). 
You should be able to run the following command, replacing with your profile name, and it should give you some long hashed output:

`aws ecr get-login-password --region us-west-2 --profile PROFILE`

TBD
