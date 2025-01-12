---
title:  "PowerNetScan"
---
## Introduction

There are so many TCP scanners available on the web. So why did I write another one? During Red Teaming operations or certification exams like OSEP or OSED I made a lot of network scans. Of course, the first choice is Nmap. The king is only one.

But when I am inside Active Directory Domain and I want to scan inside the network, I have two options: tunneling or using Windows tools. Tunneling network traffic for scanning is not reliable enough (my private opinion only).

So I have to use some Windows tools. To install additional software, it is required to gain Administrator privileges. Using portable Nmap is often marked as malicious behavior. The last chance to do it easily is to use PowerShell.

There are so many open-source PowerShell port scanners. But anyone meet my requirements. Different command syntax (reminder - Nmap is the king), slow scanning, and truncated output are the usual problems. 

So... I had to create my own port scanner, which works well for me.

## Usage
Adding the command to a PowerShell session is so simple.

```
(New-Object System.Net.WebClient).DownloadString('https://raw.githubusercontent.com/r2alter/PowerNetScan/main/PowerNetScan.ps1') | IEX
```

And just execute **Invoke-TCPPortScan** command. Targets can be defined using parameters *Hosts* **OR** *HostsFile*.

```
Invoke-TCPPortScan -Hosts google.com
```

I wanted to create similar command syntax to Nmap basic invoking. So you can use the following parameters:
- Hosts (Object) - provide IP addresses, IP address ranges (like Nmap) and hostnames separate it by comma. If used **FileHosts** parameters cannot be used.
```
Invoke-TCPPortScan -Hosts google.com,172.16.209.128-130,172.16.10.2
```
- HostsFile (Object) - provide a path to the file where targets are written line by line. If used **Hosts** parameters cannot be used.
- Ports (Object) - like always, TCP ports to scan, also ranges, and single ports separated by commas. If not provided, the top 250 ports from Nmap will be scanned.
```
Invoke-TCPPortScan -Hosts google.com -Ports 80,82,441-446
```
- Timeout (float) - How many seconds wait for a response from each port.
- Threads (int) - How many threads will be used for scanning. Nice to improving performance.
- ShowScope (switch) - If you want to see the whole scanning scope after input formatting and resolving DNS. Useless without **Verbose** switch.
- ConsoleOutput (switch) - everything is written to the console. Default output type.
- ObjectOutput (switch) - If choosen results of the scan are returned as object.
- FileOutput (String) - Save results of scan to file.

and common parameters. Important is the **Verbose** parameter which shows what is happening in background and allows checking live results.

## Valuable features

Script performs DNS resolving during format input stage. If the hostname has more than one IP address, all addresses will be added to scope.
![DNS feature](/assets/images/DNS_feature.png)

Possibility to track live results and also have results saved in variable.
![live results feature](/assets/images/live_results_feature.png)

## Conclusions

After two years I decided to share this script because I realized it works fine. LOL. I hope it will help someone at hacking activities. Enjoy.

Github link: https://github.com/r2alter/PowerNetScan/tree/main