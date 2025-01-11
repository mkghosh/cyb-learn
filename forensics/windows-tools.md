# Windows tools to use for forensics

|Tools|Install Location|Purpose|
|---|---|---|
|Sysmon|C:\Windows\Sysmon64.exe|Provides detailed event logging and detection.|
|Eric Zimmerman Tools|C:\Tools\EZ-Tools|Forensic utilities for analyzing digital evidence, such as registry hives and event logs.|
|Yara|C:\Tools\yara\yara.exe|Signature-based file scanning tool.|
|Chainsaw|C:\Tools\Sigma\chainsaw\chainsaw_x86_64-pc-windows-msvc.exe|Command-line tool for parsing and hunting through Windows Event Logs.|
|Sigma|C:\Program Files\Python312\Scripts\sigma.exe|Generic signature format for SIEM rule creation.|
|Zircolite|C:\Tools\Sigma\zircolite\zircolite_win_x64_2.20.0.exe|Sigma-based EVTX log analysis.|
|Osquery|C:\Program Files\osquery\osqueryi.exe|Endpoint monitoring using SQL-like queries.|
|Velociraptor|C:\Program Files\Velociraptor (https://<Target_IP>:8889)|Endpoint monitoring, collection, and response.|
|Wireshark|C:\Program Files\Wireshark|Packet capture tool for network traffic analysis.|
|[DumpIt](https://www.toolwar.com/2014/01/dumpit-memory-dump-tools.html)|C:\Tools\Memory-Dump\DumpIt.exe|Memory dumping utility for memory forensics.|
|[WinPmem](https://github.com/Velocidex/WinPmem)|C:\Tools\Memory-Dump\winpmem_mini_x64_rc2.exe|Memory dumping utility for memory forensics.|
|[Volatility v2](https://github.com/volatilityfoundation/volatility)|C:\Tools\Volatility2|Memory forensics tool for analyzing memory dumps.|
|[Volatility v3](https://github.com/volatilityfoundation/volatility)|C:\Tools\volatility3|Memory forensics tool for analyzing memory dumps.|
|SilkETW|C:\Tools\SilkETW\SilkETW\SilkETW.exe|C# wrappers for ETW.|
|[SealighterTI](https://github.com/pathtofile/SealighterTI)|C:\Tools\SealighterTI.exe|Running Microsoft-Windows-Threat-Intelligence without a driver.|
|AMSI-Monitoring-Script|C:\Tools\AMSIScript\AMSIScriptContentRetrieval.ps1|Extracting script contents using the AMSI ETW provider.|
|[JonMon](https://github.com/jsecurity101/JonMon)|C:\Tools\JonMon|Collection of open-source telemetry sensors.|
|CFF-Explorer|C:\Tools\CFF-Explorer\CFF Explorer.exe|Tool designed for examining and editing Portable Executable (PE) files.|
|Ghidra|C:\Tools\Ghidra\ghidraRun.bat|Software reverse engineering (SRE) framework.|
|x64dbg|C:\Tools\x64dbg|Open-source x64/x32 debugger for windows.|
|SpeakEasy|C:\Tools\speakeasy|Modular, binary emulator designed to emulate Windows kernel and user mode malware.|
|SysInternalsSuite|C:\Tools\SysinternalsSuite|Sysinternals Troubleshooting Utilities.|
|Get-InjectedThread|C:\Tools\Get-InjectedThread.ps1|Looks for threads that were created as a result of code injection.|
|Hollows_Hunter|C:\Tools\hollows_hunter64.exe|Scans all running processes. Recognizes and dumps a variety of potentially malicious implants.|
|Moneta|C:\Tools\Moneta64.exe|Live usermode memory analysis tool for Windows with the capability to detect malware IOCs.|
|PE-Sieve|C:\Tools\pe-sieve64.exe|Detects malware running on the system, as well as collects the potentially malicious material for further analysis.|
|API-Monitor|C:\Tools\API Monitor|Monitors and controls API calls made by applications and services.|
|PE-Bear|C:\Tools\PE-bear|Multiplatform reversing tool for PE files.|
|ProcessHacker|C:\Tools\ProcessHacker|Monitors system resources, debugs software and detects malware.|
|ProcMonX|C:\Tools\ProcMonX.exe|Extended Process Monitor-like tool based on Event Tracing for Windows.|
||||
