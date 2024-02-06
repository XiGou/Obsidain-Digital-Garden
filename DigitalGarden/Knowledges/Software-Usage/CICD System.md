---
date: 2024-02-06 01:14:31 +0800
title: "CICD System"
---

## Jenkins
Implemented by Java ，pipeline defined by Jenkinsfile, can achieve `CI as Code`.  
Master-Slave architecture.   
There are abundant plug-ins, but many of them are in a state of no maintenance,  are usable but have a lot of Warnings.  
The UI is a bit outdated, but the new version is barely evolving into modern society.  

## Drone

https://docs.drone.io/.  

Can easily integrate with github/gitea/bitbucket/gogs, the pipeline is defined by a yaml file, similar to github acitions.

## Gitea

Opensource Github Alternative, Can run github actions.

## Github Actions

Easy to use, github provide a 2C compute resource to run your actions, you don't need to maintain a linux server.
