# Identifying the hashing method
We can use has-identifier to guess the hashing method or we can also use online tools like [hash-analyzer](https://www.tunnelsup.com/hash-analyzer/) or [hash-identification](https://www.onlinehashcrack.com/hash-identification.php) 

## To install hash-identifier in debian based system (kali or parrot) 
sudo apt install hash-identifier

## Usage
step 1: In terminal enter the command hash-identifier
step 2: insert the hash in the prompt and press enter it will show the possible hashing method (i.e MD5, SHA1, etc)


# Cracking Hash
We can use hashcat to crack hash to install hashcat we can use udo apt install hashcat in debian systems. Or we can use online tools like [crackstation](https://crackstation.net/)

Now cracing the hashs in [rack the hash](https://tryhackme.com/r/room/crackthehash) room of tryhackme. 

# Hash Types
        0 = MD5
       10 = md5($pass.$salt)
       20 = md5($salt.$pass)
       30 = md5(unicode($pass).$salt)
       40 = md5($salt.unicode($pass))
       50 = HMAC-MD5 (key = $pass)
       60 = HMAC-MD5 (key = $salt)
       100 = SHA1
       110 = sha1($pass.$salt)
       120 = sha1($salt.$pass)
       130 = sha1(unicode($pass).$salt)
       140 = sha1($salt.unicode($pass))
       150 = HMAC-SHA1 (key = $pass)
       160 = HMAC-SHA1 (key = $salt)
       200 = MySQL323
       300 = MySQL4.1/MySQL5
       400 = phpass, MD5(Wordpress), MD5(phpBB3), MD5(Joomla)
       500 = md5crypt, MD5(Unix), FreeBSD MD5, Cisco-IOS MD5
       900 = MD4
       1000 = NTLM
       1100 = Domain Cached Credentials (DCC), MS Cache
       1400 = SHA256
       1410 = sha256($pass.$salt)
       1420 = sha256($salt.$pass)
       1430 = sha256(unicode($pass).$salt)
       1431 = base64(sha256(unicode($pass)))
       1440 = sha256($salt.unicode($pass))
       1450 = HMAC-SHA256 (key = $pass)
       1460 = HMAC-SHA256 (key = $salt)
       1600 = md5apr1, MD5(APR), Apache MD5
       1700 = SHA512
       1710 = sha512($pass.$salt)
       1710 = sha512($pass.$salt)
       1720 = sha512($salt.$pass)
       1730 = sha512(unicode($pass).$salt)
       1740 = sha512($salt.unicode($pass))
       1750 = HMAC-SHA512 (key = $pass)
       1760 = HMAC-SHA512 (key = $salt)
       1800 = SHA-512(Unix)
       2400 = Cisco-PIX MD5
       2410 = Cisco-ASA MD5
       2500 = WPA/WPA2
       2600 = Double MD5
       3200 = bcrypt, Blowfish(OpenBSD)
       3300 = MD5(Sun)
       3500 = md5(md5(md5($pass)))
       3610 = md5(md5($salt).$pass)
       3710 = md5($salt.md5($pass))
       3720 = md5($pass.md5($salt))
       3800 = md5($salt.$pass.$salt)
       3910 = md5(md5($pass).md5($salt))
       4010 = md5($salt.md5($salt.$pass))
       4110 = md5($salt.md5($pass.$salt))
       4210 = md5($username.0.$pass)
       4300 = md5(strtoupper(md5($pass)))
       4400 = md5(sha1($pass))
       4500 = Double SHA1
       4600 = sha1(sha1(sha1($pass)))
       4700 = sha1(md5($pass))
       4800 = MD5(Chap), iSCSI CHAP authentication
       4900 = sha1($salt.$pass.$salt)
       5000 = SHA-3(Keccak)
       5100 = Half MD5
       5200 = Password Safe SHA-256
       5300 = IKE-PSK MD5
       5400 = IKE-PSK SHA1
       5500 = NetNTLMv1-VANILLA / NetNTLMv1-ESS
       5600 = NetNTLMv2
       5700 = Cisco-IOS SHA256
       5800 = Android PIN
       6300 = AIX {smd5}
       6400 = AIX {ssha256}
       6500 = AIX {ssha512}
       6700 = AIX {ssha1}
       6900 = GOST, GOST R 34.11-94
       7000 = Fortigate (FortiOS)
       7100 = OS X v10.8+
       7200 = GRUB 2
       7300 = IPMI2 RAKP HMAC-SHA1
       7400 = sha256crypt, SHA256(Unix)
       7900 = Drupal7
       8400 = WBB3, Woltlab Burning Board 3
       8900 = scrypt
       9200 = Cisco $8$
       9300 = Cisco $9$
       9800 = Radmin2
       10000 = Django (PBKDF2-SHA256)
       10200 = Cram MD5
       10300 = SAP CODVN H (PWDSALTEDHASH) iSSHA-1
       11000 = PrestaShop
       11100 = PostgreSQL Challenge-Response Authentication (MD5)
       11200 = MySQL Challenge-Response Authentication (SHA1)
       11400 = SIP digest authentication (MD5)
       99999 = Plaintext

Level 1:
# Hash-1: 48bb6e862e54f2a795ffc4e541caed4d
MD5

Hash analyzer says this is MD5 hash so we will use hashcat with the md5 [hash mode](https://hashcat.net/wiki/doku.php?id=example_hashes)  

$hashcat -m 0 48bb6e862e54f2a795ffc4e541caed4d /usr/share/wordlist/rockyou.txt
48bb6e862e54f2a795ffc4e541caed4d:easy

# Hash-2: CBFDAC6008F9CAB4083784CBD1874F76618D2A97
SHA-1 according to hash-identifier

Command: 
$hashcat -m 100 CBFDAC6008F9CAB4083784CBD1874F76618D2A97 /usr/share/wordlists/rockyou.txt
cbfdac6008f9cab4083784cbd1874f76618d2a97:password123

# Hash-3: 1C8BFE8F801D79745C4631D09FFF36C82AA37FC4CCE4FC946683D7B336B63032

SHA256 or HAVAL-256
Command :
$hashcat -m 1400 1C8BFE8F801D79745C4631D09FFF36C82AA37FC4CCE4FC946683D7B336B63032 /usr/share/wordlists/rockyou.txt
1c8bfe8f801d79745c4631d09fff36c82aa37fc4cce4fc946683d7b336b63032:letmein 

# Hash-4: $2y$12$Dwt1BZj6pcyc3Dy1FWZ5ieeUznr71EeNkJkUlypTsgbX1H68wsRom
Not Found in hash-identifier
bcrypt according to hash analyzer

For this type of hash the pass could not be put directly in the command we need to save the hash in a file first 

vi bc.hash
$2y$12$Dwt1BZj6pcyc3Dy1FWZ5ieeUznr71EeNkJkUlypTsgbX1H68wsRom

$hashcat -m 3200 bc.hash /usr/share/wordlist/rockyou.txt

