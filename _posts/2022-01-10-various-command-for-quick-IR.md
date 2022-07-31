---
layout: post
published: true
title: various command for quick IR
author: admin
comments: true
date: '2022-01-10 23:00'
categories:
  - DFIR
  - incident Response
  - shell
---
<s>Saya sedang mengerjakan sebuah investigation challenge di tryhackme.com,
dan ada </s> Beberapa command line yang saya pakai disana yang menurut saya cukup menarik untuk di dokumentasikan,
serta dapat dipakai sebagai script IR kilat di bagian awal asesmen, dan sebisa mungkin saya update terus.

<!--more-->
#### command line yang saya pakai

#####list usernames
```bash
net user
```
```bash
Get-LocalUser
```

#####list ADusername
```bash
Get-ADUser -Filter 'Name -Like "*"' | where Enabled -eq $True
```

#####list logged on user
```bash
Get-CimInstance –ClassName Win32_ComputerSystem | Select-Object Name, UserName, PrimaryOwnerName, Domain, TotalPhysicalMemory, Model, Manufacturer
```

#####last logon, group member, password settings, user full name, etc
```bash
net user [username]
```

#####show local group and/or members of groups
```bash
net localgroup
```
```bash
net localgroup "Administrators"
```
```bash
Get-LocalGroup
```

#####show ADgroups
```bash
Get-ADGroupMember Administrators | where objectClass -eq 'user'
```
```bash
Get-ADComputer -Filter "Name -Like '*'" -Properties * | where Enabled -eq $True | Select-Object Name, OperatingSystem, Enabled
```

#####list running programs (and certain programs only)
```bash
tasklist
tasklist /m /fi “pid eq <Insert Process ID here w/out the brackets>”
```
```bash
Get-CimInstance -ClassName Win32_Process | Select-Object CreationDate, ProcessName, ProcessID, CommandLine, ParetProcessId | where ProcessID -eq xxxx
```
```bash
Get-Process | Select-Object StartTime, ProcessName, ID, Path | Where Id -eq xxxx
```

#####list schedule task, services
```bash
schtasks /query /fo list /v > schtasks.txt
```

#####list services
```bash
Get-CimInstance –ClassName Win32_Service | Select-Object Name, DisplayName, StartMode, State, PathName, StartName, ServiceType
```
```bash
Get-Service | Select-Object Name, DisplayName, Status, StartType
```

#####various wevtutil
```bash
wevtutil qe Security /f:text > seclogs.txt
wevtutil el | Measure-Object
```

#####system information
```bash
systeminfo
```

#####osbuild, servicepack, buildnumber, csname, lastboot
```bash
Get-CimInstance Win32_OperatingSystem | Select-Object Caption, Version, servicepackmajorversion, BuildNumber, CSName, LastBootUpTime
```

#####various wmic
```bash
wmic /node:<remote-ip> /user:<username> startup list full | more
wmic /node:<remote-ip> /user:<username> service list full | more
wmic /node:<remote-ip> /user:<username> ComputerSystem Get UserName
wmic /node:<remote-ip> /user:<username> useraccount list full
wmic /node:<remote-ip> /user:<username> process get description,processid,parentprocessid,commandline /format:csv
wmic /node:<remote-ip> /user:<username> bios get serialnumber
wmic /node:<remote-ip> /user:<username> diskdrive get model,serialNumber,size,mediaType
```

#####ipconfig
```bash
ipconfig /displaydns
```

#####network related
network activity
```bash
Get-NetTCPConnection -RemoteAddress xxx.xxx.xxx.xxx -RemotePort xx | Select-Object CreationTime, LocalAddress, LocalPort, RemoteAddress, RemotePort, OwningProcess, Stat
```
netstat with PID
```bash
netstat -bona
```

#####see firewall state
```bash
netsh firewall show state
```

#####melihat dan setting execution policy
```bash
Get-ExecutionPolicy
Set-ExecutionPolicy
```

#####check PS version
```bash
Get-Host | Select-Object Version
```

#####hashing
```bash
Get-FileHash .\file.txt -Algorithm MD5
```

#####Alternate data stream (ADS)
```bash
Get-Item .\file.txt -Stream *
```

#####raw file access, hex
```bash
Get-Content .\file.txt –Encoding Byte | Format-Hex
```

#### penutup

bagusnya sih dirangkum dalam sebuah script yang dapat dengan mudah langsung dijalankan.
post ini tentu saja akan saya update d masa mendatang, dan semoga suatu saat script yang menyatukannya akan saya kerjakan :D

#### referensi

1. [arts.ubc.ca](https://isit.arts.ubc.ca/how-to-locate-serial-number-of-computer/)
2. [sans](https://www.sans.org/blog/wmic-for-incident-response/)
3. [jordanpotti](https://jordanpotti.com/2017/01/20/basics-of-windows-incident-response/)
4. [cybernote](http://www.cybernote.net/index.php/2020/05/02/practical-incident-response-commands-wmic/)
5. [Troy Wojewoda](https://www.giac.org/paper/gsec/23549/hunting-gathering-powershell/121279)
