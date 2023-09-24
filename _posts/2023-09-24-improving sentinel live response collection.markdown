---
title: improving sentinel live response collection
date: 2023-09-24 13:48:00 Z
categories:
- DFIR
- incident Response
- SIEM
---

## Intro
Sentinel have live response capabilities to do collection of devices in case of DFIR needs. The problem is sometime the collection is not as complete as need, and to customize this a script (or executable) is needed. Although I won't share any detailed script but here is how similar problem was solved and what obstacle I found.

<!--more-->
## What we did
These are several things that we use to improve sentinel live response collection capabilities:

* using KAPE to collect

* create power shell to download, and later run KAPE with required parameters

* use some sort of blob storage in cloud to store KAPE executable and later collection result

## Obstacles 
* KAPE run pretty okay, but in the end decided to use batch mode instead of compound target in KAPE. This is to ensure collection run as efficient as possible.

* One the problem is trying to group folders from the same machine, because batch upload to blob storage will have each batch command in separate folder. Although KAPE -s3kp switch will allow folder grouping, the problem is the switch won't take environment variables (unlike --tsource/ --tdest switch) so no way to group collection result via machine name.

* to solve this, power shell is used to write this environment variables to KAPE batch file in -s3kp switch.

## Result
KAPE run pretty okay, and I think this is a solution well done.

Aside than those problems, doing this at scale is another problem to solve. This I believe will require API access and impossible to create workaround via power shell.

## Other
* more on KAPE:[Kape manual](https://ericzimmerman.github.io/KapeDocs/)