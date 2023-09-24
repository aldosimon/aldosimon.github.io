---
title: improving sentinel live response collection
date: 2023-09-24 13:48:00 Z
categories:
- DFIR
- incident Response
- SIEM
---

Throughout my experience using sentinel, I felt that sentinel live response collection is not very good. This post document how I try to solve the problem and obstacles I found. 

<!--more-->
#### Intro
Sentinel have live response capabilities to do collection of devices in case of DFIR needs. 

The problem is sometime the collection is not as complete as needed, and to customize this, a script (or executable) is needed. 

Although I won't share any detailed script but here is how similar problem was solved and what obstacle I found.
 
#### What we did
What basically we need to do is using power shell to do prep work and KAPE to do most of the heavy lifting. These are several steps taken to do that:
* run live response, download customized power shell script.

* power shell script was then used to download KAPE,  run KAPE with required parameters, and upload collection result.

* blob storage was used to store KAPE executable and later the collection result.

#### Obstacles
Here are obstacles found in implementing the solution and some work around used to deal with it:
 
* KAPE run pretty okay, but in the end decided to use batch mode instead of compound target in KAPE. This is to ensure collection run as efficient as possible.

* One the problem is trying to group folders from the same machine, because batch upload to blob storage will have each batch command in separate folder. 
Although KAPE -s3kp switch will allow folder grouping, the problem is the switch won't take environment variables (unlike --tsource/ --tdest switch) so no way to group collection result using machine name.

* Power shell is used to solve this by writing environment variables straight to KAPE batch file in -s3kp switch instead of using %m internal KAPE variables.

* Another thing I found is KAPE turns to zombie after finishing task, which is annoying but since collection probably done only once, I don't think this is a big problem.

#### Result
KAPE run pretty okay, and I think all in all this is a pretty good solution.

Aside than those problems, doing this at scale is another problem to solve. This I believe will require API access and impossible to create workaround via power shell.

#### Other
* more on KAPE:[KAPE manual](https://ericzimmerman.github.io/KapeDocs/)