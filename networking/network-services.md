# Understanding different types of networking services and their configurations

## Server Message Block (SMB) protocol

The server message block protocol is a client-server communication protocol used for sharing access to files, printers, serial ports and other resources on a network. It can also carry transaction protocols for interprocess communication. Over the years, SMB has beed used primarily to connect windows computer, although most other system **such as Linux and macOS** also include client components for connecting to SMB resources.

## SMB dialects

- SMB 1.0 (1984) SMB 1.0 was created by IBP for file sharing in DOS. It introduced opportunistic locking (OpLock) as a client-side caching mechanism designed to reduce network traffice. Microsoft would later include the SMB protocol in its LAMN Manger product.
- CIFS (1996) CIFS is a Microsoft-developed SMB dialect that debuted in Windows 95. Short for Common Internet File System, CIFS added support for larger file sizes, direct transport over TCP/IP, and symbolic links and hard links.
- SMB 2.0 (2006) SMB 2.0 was released with Windows Vista and Windows Server 2008. It reduced chattiness to improve persormance, enhanced scalability and resiliency, and added support for wide area network (WAN) acceleration.
- SMB 2.1 (2010) SMB 2.1 was introduced with Windows Server 2008 R2 and Windows 7. The client OpLock leasing model replaced OpLock to enhance caching and improve performance. Other updates included large maximum transmission unit support and improved energy efficiency, which enabled clinets with open files from an SMB server to enter sleep mode.
- SMB 3.0 (2012) SMB 3.0 deguted in Windows 8 and Windows Sever 2012. It added several significant upgrades to improve availability, performance, backup, security and management. Noteworthy new features included SMB Multichannel, SMB Direct, transparent failover of client access, Remote Volume Shadow Copy Service support, SMB Encryption and more.
- SMB 3.02 (2014) SMB 3.02 was introduced in Windows 8.1 and Windows Server 2012 R2. It included performance updates and the ability to disable CIFS/SMB 1.0 support, including removal of the related binaries.
- SMB 3.1.1 (2015) SMB 3.1.1 was released with Windows 10 and Windows server 2016. It added support for advanced encryption, pre-authentication integrity to prevent man-in-the-middle (MitM) attacks and cluster dialect fencing, among other updates

## Enumerating SMB

This process is essential for an attacke to be successful, as wasting time with exploits that either don't work or can crash the system can be a wate of energy. Enumeration can be used to gather usernames, passwords, network information, hostnames, application data, services, or any other information that may be valuable to an attacker.

Typically, there are SMB share drives on a server that can be connected to and used to biew or transfer files. SMB can often be a great starting point for an attacker looking to discover sensitive information you'd be surprised what is sometimes included on these shares.

## Port scanning

The first step of enumeration is to conduct a port scan, to find out as much information as you can about the services, applications, structure and operating system of the target machine.

## [Enum4Linux](https://github.com/CiscoCXSecurity/enum4linux)

Enum4Linux is a tool used to enumerate SMB shares on both Windows and Linux systems. It is basically a wrapper around the tools in the Samba package and makes it easy to quickly extract information from the target pertaining to SMB.

To learn more about en4ln go [en4ln](https://labs.portcullis.co.uk/tools/enum4linux/)

## Usage

enum4linux [options] ip

|OPTION|FUNCTION|
|---|---|
|-U|get userlist|
|-M|get machine list|
|-N|get namelist dump (different from -U and -M|
|-S|get sharelist|
|-P|get password policy information|
|-G|get group and member list|
|-a|all of the above (full basic enumeration)|
|-V|verbose mode|
|||

## What is NFS

NFS stands for "Network File System" and allows a system to share directories and files with others over a network. By using NFS, users and programs can access files on remote systems almost as if they were local files. It does this by mounting all, or a portion of a file system on a server. The portion of the file system that is mounted can be accessed by clients with whatever privileges are assigned to each file.

To know details about NFS follow the [NFS in Detail](https://docs.oracle.com/cd/E19683-01/816-4882/6mb2ipq7l/index.html).

## Enumerating NFS

Enumeration is defined as "a process which establishes an active connection to the target hosts to discover potential attack vectors in the system, and the same can be used for further exploitation of the system." - Infosec Institute. It is a critical phase when considering how to enumerate and exploit a remote machine - as the information you will use to inform your attacks will come from this stage.

### NFS-Common

It is important to have this package installed on any machine that uses NFS, either as client or server. It includes programs such as: lockd, statd, showmount, nfsstat, gssd, idmapd and mount.nfs. Primarily, we are concerned with "showmount" and "mount.nfs" as these are going to be most useful to us when it comes to extracting information from the NFS share. If you'd like more information about this package, feel free to read: [NFS Common](https://packages.ubuntu.com/jammy/nfs-common)

### Port Scanning

The first step of enumeration is to conduct a port scan, to find out as much information as you can about the services, open ports and operating system of the target machine. You can go as in-depth as you like on this, however, I suggest using nmap with the -A and -p- tags.

### Mounting NFS shares

Your client’s system needs a directory where all the content shared by the host server in the export folder can be accessed. You can create
this folder anywhere on your system. Once you've created this mount point, you can use the "mount" command to connect the NFS share to the mount point on your machine like so:

``` bash
sudo mount -t nfs IP:share /tmp/mount/ -nolock
```

### What is root_squash?

By default, on NFS shares- Root Squashing is enabled, and prevents anyone connecting to the NFS share from having root access to the NFS volume. Remote root users are assigned a user “nfsnobody” when connected, which has the least local privileges. Not what we want. However, if this is turned off, it can allow the creation of SUID bit files, allowing a remote user root access to the connected system.

### SUID

So, what are files with the SUID bit set? Essentially, this means that the file or files can be run with the permissions of the file(s) owner/group. In this case, as the super-user. We can leverage this to get a shell with these privileges!

### Method

This sounds complicated, but really- provided you're familiar with how SUID files work, it's fairly easy to understand. We're able to upload files to the NFS share, and control the permissions of these files. We can set the permissions of whatever we upload, in this case a bash shell executable. We can then log in through SSH, as we did in the previous task- and execute this executable to gain a root shell!

### The Executable

Due to compatibility reasons,  we will obtain the bash executable directly from the target machine.
With the key obtained in the previous task, we can use SCP with the command ```scp -i key_name username@10.48.188.88:/bin/bash ~/Downloads/bash``` to download it onto our attacking machine.

Another method to overcome compatibility issues is to obtain a standard Ubuntu Server 18.04 bash executable, the same as the server's- as we know from our nmap scan. You can download it here. If you want to download it via the command line, be careful not to download the github page instead of the raw script. You can use wget <https://github.com/polo-sec/writing/raw/master/Security%20Challenge%20Walkthroughs/Networks%202/bash>. Note that this method requires an internet connection, so you won't be able to download it when using a free AttackBox.

### Mapped Out Pathway

If this is still hard to follow, here's a step by step of the actions we're taking, and how they all tie together to allow us to gain a root shell:

``` sh
    NFS Access ->
        Gain Low Privilege Shell ->

            Upload Bash Executable to the NFS share ->

                Set SUID Permissions Through NFS Due To Misconfigured Root Squash ->

                    Login through SSH ->

                        Execute SUID Bit Bash Executable ->

                            ROOT ACCESS
```

## SMTP

SMTP stands for "Simple Mail Transfer Protocol". It is utilised to handle the sending of emails. In order to support email services, a protocol pair is required, comprising of SMTP and POP/IMAP. Together they allow the user to send outgoing mail and retrieve incoming mail, respectively.

The SMTP server performs three basic functions:
It verifies who is sending emails through the SMTP server.
It sends the outgoing mail
If the outgoing mail can't be delivered it sends the message back to the sender

Most people will have encountered SMTP when configuring a new email address on some third-party email clients, such as Thunderbird; as when you configure a new email client, you will need to configure the SMTP server configuration in order to send outgoing emails.

### POP and IMAP

POP, or "Post Office Protocol" and IMAP, "Internet Message Access Protocol" are both email protocols who are responsible for the transfer of email between a client and a mail server. The main differences is in POP's more simplistic approach of downloading the inbox from the mail server, to the client. Where IMAP will synchronise the current inbox, with new mail on the server, downloading anything new. This means that changes to the inbox made on one computer, over IMAP, will persist if you then synchronise the inbox from another computer. The POP/IMAP server is responsible for fulfiling this process.

### How does SMTP work?

Email delivery functions much the same as the physical mail delivery system. The user will supply the email (a letter) and a service (the postal delivery service), and through a series of steps- will deliver it to the recipients inbox (postbox). The role of the SMTP server in this service, is to act as the sorting office, the email (letter) is picked up and sent to this server, which then directs it to the recipient.

We can map the journey of an email from your computer to the recipient’s like this:

1. The mail user agent, which is either your email client or an external program. connects to the SMTP server of your domain, e.g. smtp.google.com. This initiates the SMTP handshake. This connection works over the SMTP port- which is usually 25. Once these connections have been made and validated, the SMTP session starts.

2. The process of sending mail can now begin. The client first submits the sender, and recipient's email address- the body of the email and any attachments, to the server.

3. The SMTP server then checks whether the domain name of the recipient and the sender is the same.

4. The SMTP server of the sender will make a connection to the recipient's SMTP server before relaying the email. If the recipient's server can't be accessed, or is not available- the Email gets put into an SMTP queue.

5. Then, the recipient's SMTP server will verify the incoming email. It does this by checking if the domain and user name have been recognised. The server will then forward the email to the POP or IMAP server, as shown in the diagram above.

6. The E-Mail will then show up in the recipient's inbox.

### Enumerating Server Details

Poorly configured or vulnerable mail servers can often provide an initial foothold into a network, but prior to launching an attack, we want to fingerprint the server to make our targeting as precise as possible. We're going to use the "smtp_version" module in MetaSploit to do this. As its name implies, it will scan a range of IP addresses and determine the version of any mail servers it encounters.

### Enumerating Users from SMTP

The SMTP service has two internal commands that allow the enumeration of users: VRFY (confirming the names of valid users) and EXPN (which reveals the actual address of user’s aliases and lists of e-mail (mailing lists). Using these SMTP commands, we can reveal a list of valid users

We can do this manually, over a telnet connection- however Metasploit comes to the rescue again, providing a handy module appropriately called "smtp_enum" that will do the legwork for us! Using the module is a simple matter of feeding it a host or range of hosts to scan and a wordlist containing usernames to enumerate.

### Requirements

As we're going to be using Metasploit for this, it's important that you have Metasploit installed. It is by default on both Kali Linux and Parrot OS; however, it's always worth doing a quick update to make sure that you're on the latest version before launching any attacks. You can do this with a simple "sudo apt update", and accompanying upgrade- if any are required.

### Alternatives

It's worth noting that this enumeration technique will work for the majority of SMTP configurations; however there are other, non-metasploit tools such as smtp-user-enum that work even better for enumerating OS-level user accounts on Solaris via the SMTP service. Enumeration is performed by inspecting the responses to VRFY, EXPN, and RCPT TO commands.

This technique could be adapted in future to work against other vulnerable SMTP daemons, but this hasn’t been done as of the time of writing. It's an alternative that's worth keeping in mind if you're trying to distance yourself from using Metasploit e.g. in preparation for OSCP.

Now we've covered the theory. Let's get going!
