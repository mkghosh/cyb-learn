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

To learn more about en4ln go [here](https://labs.portcullis.co.uk/tools/enum4linux/)

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

