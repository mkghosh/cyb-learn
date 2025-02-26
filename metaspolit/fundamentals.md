# Metasploit fundmentals

## Main concepts of Metasploit

While using the Metasploit framework, you will primarily interact with the metasploit console. You can launch it from the terminal using msfconsole command. The console will be your main interface to interact with the different modules of the Metasploit Framework. Modules are small components within the Metasploit framework that are built to perform a specific task, such as exploiting a vulnerability, scanning a target, or performing a brute-force attack.

Some concepts related to metasploit framework:

- Exploit: A piece of code that uses a vulnerability present on the target system.
- Vulnerability: A design, coding, or logic flaw affecting the target system. The exploitation of a vulnerability can result in disclosing confidential information or allowing the attacker to execute code on the target system.
- Payload: An exploit will take advantage of a vulnerability. However, if we want the expoit to have the result we want (gaining access to the target system, read confidential information, etc.), we need to use a payload. Payloads are the code that will run on the target system.

## Msfconsole

As previously mentioned, the console will be your main interface to the Metasploit Framewok. you can launch it using the msfconsole comman on your terminal or any system the Metasploit Framework is installed on.

Once launched, you will see the command line changes to msf6 (or msf5 depending on the installed version of Metasploit). The Metasploit console (msfconsole) can be used just like a regular command-line shell.

The help command can be used on its own or for a specific command. Msfconsole is managed by context; this means that unless set as a global variable, all parameter settings will be lost if you change the module you have decided to use.

The module to be used can also be selected with the use command followed by the number at the beginning of the search result line.

```bash
msf6 > use exploit/windows/smb/ms17_010_eternalblue
```

While the prompt has changed, you will notice we can still run the commands previouly mentioned. This means we did not "enter" a folder as you would typically expect in an operating system command line.

The prompt tells us we now have a context set in which we will work. You can see this by typing the ```show options``` command. This will print options related to the exploit we have chosen earlier. The ```show options``` command will have different output depending on the context it is used in. The example above shows that this exploit will require we set variables like RHOSTS and RPORT. On the other hand, a post-exploitation module may only need us to set a SESSION ID. The show command can be used in any context followed by a module type (auxiliary, payload, exploit, etc) to list available modules. ```show payloads``` will show all the payloads available in this context. You can leave the context by typing ```back``` command.

Further information on any module can be obtained by typing the ```info``` command within its context. Alternatively, you can use the ```info``` command followed by the module's path from the msfconsole prompt (e.g. ```info exploit/windows/smb/ms17_010_eternalblue```). Info is not a help menu; it will display detailed information on the module such as its author, relevant sources, etc.

One of the most useful commands in msfconsole is search. This command will search the Metasploit Framework database for modules relevant to the given search parameter. You can conduct searches useing CVE number, exploit names (eternalblue, hearbleed, etc.), or target system.

The output of the search command provides an overview of each returned module. You may notice the name column already gives more information than just the module name. you can see the type of module and the category of the module (scanned, admin, windows, Unix, etc). You can use any module returned in a search result with the command use followed by the number at the beginning of the result line. (e.t. ```use 0``` instead of ```use auxiliary/admin/smb/ms17_010_command```).

Another essential piece of information returned is in the rank column. Exploits are rated based on their reliability. The table below provides their respective descriptions.

|Ranking|Description
|---|---
|ExecentRanking|The exploit will never crash the service. This is the case for SQL Injection, CMD execution, RFI, LFI etc. No typical memory corruption exploits should be given this ranking unless there are extraordinary circumstances.
|GreatRanking|The exploit has a default target AND either auto-detects the appropriate target or uses an application-specific return address AFTER a verison check
|GoodRanking|The exploit has a default target and it is the common case for this type of software (English, Windows7 for a desktop app, 2012 for server, etc)
|NormalRanking|The exploit is otherwise reliable, but depends on a specific version and can't (or doesn't) reliably autodetect.
|AverageRanking|The exploit is generally unreliable or difficult to exploit.
|LowRanking|The exploit is nearly impossible to exploit (or under 50% success rate) for common platforms.
|ManualRanking|The exploit is unstable or difficult to exploit and is basically a DoS. This ranking is also used when the module has no use unless specifically configured by the user (e.g. exploit/unix/webapp/php_eval)

You can direct the search function using keywords such as type and platform.

For example, if we wanted our search results to only include auxiliary modules, we could set the type to auxiliary by using the below command:

```bash
msf6 > search type:auxiliary telnet
```

Do, remember that exploits take advantage of a vulnerability on the target system and may always show unexpected behaviour. A low-ranking exploit may work perfectly, and an excellent ranked exploit may not, or worse, crash the target system.

## Working with modules

As mentioned earlier, show options command will list all available parameters. Some of these parameters require a value for the exploit to work. Some required parameter values will be pre-populated, make sure you check if these should remain the same for you target. For example, a web exploit could have an RPORT (remote port: the port on the target system Metasploit will try to connect to and run the exploit) value preset to 80, but your target web application could be using port 8080.

- RPORT: "Remote Port", the port on the target system the vulnerable application is running on.
- PAYLOAD: The payload you will use with the exploit.
- LHOST: "Localhost", the atacking machine IP address.
- LPORT: "Local Port", the port you will kuse for the reverse shell to connect back to. This is aport on your attacking machine, and you can set it to any port not used by any other application.
- SESSION: Each connection established to the target system using Metasploit will have a session ID. You will use this with post-exploitation modules that will connect to the target system using an existing connection.

You can override any set parameter using the set command again with a different value. You can also clear any parameter value using the unset command or clear all set parameters with the unset all command.

You can use setg command to set values that will be used for all modules. The setg command is used like the set command. The difference is that if you use the set command to set a value using a module and you switch to another module, you will need to set the value again. The setg command allows you to set the value so it can be used by default across different modules. You can clear any value set with setg using unsetg.

Once all module parameters are set, you can launch the module using the exploit command. Metasploit also supports the run command, which is an alias created for the exploit command as the work exploirt did not make sense when using modules that were not exploits (port scanners, vulnerability scanners, etc).

Once a vulnerability has been successfully exploited, a session will be created. This is the communication channel established between the target system and Metasploit. You can use the ```backgroud``` command to background the session prompt and go back to the msfconsole prompt. Alernatively, CTRL+Z can be used to background sessions. The sessions command can be used from the msfconsole prompt or any context to see the existing sesisons. To interact with any session, you can use the sessions -i command followed by the desired session number.

```bash
msf6 > sessions
msf6 > session -i 1
```
