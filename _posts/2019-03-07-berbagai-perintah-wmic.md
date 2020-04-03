---
layout: post
title: berbagai perintah wmic
date: 2019-03-07 08:00
author: admin
comments: true
categories: [DFIR, forensic, forensic, triage]
---
Beberapa perintah wmic yang berguna saat melakukan <em>triage</em>, yang <strong>rencananya</strong> akan terus di update<!--more-->

nama, model, ukuran dan serial number hard drive

<code>wmic diskdrive get name,model,size,serialnumber</code>

versi windows dan uuid mesin

<code>wmic csproduct get name,version,uuid</code>

user account dan security identifier:

<code>wmic useraccount get name,sid</code>

&nbsp;

<span style="font-size: 10pt;"><em>*updated 07/03/2019</em></span>
