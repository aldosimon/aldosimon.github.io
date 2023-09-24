---
title: improving sentinel live response collection
date: 2023-09-24 13:48:00 Z
published: false
categories:
- DFIR
- incident Response
- SIEM
---

## Intro
Sentinel have live response capabilities to do collection of devices in case of DFIR needs. The problem is sometime the collection is not as complete as we need, and to customize this a script (or executable) is needed. Here is how we solve similar problem.

<!--more-->
## What we did
These are several things that we use to improve sentinel live response collection capabilities:
* using KAPE to collect
* create power shell to run KAPE with required parameters
* use some sort of blob storage in cloud to store KAPE executable and later collection result

## Obstacles and how we deal


## Result