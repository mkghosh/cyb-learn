# Gobuster: Introduction

**Introduction:** Gobuster is an open-source offensive tool written in Golang. It enumerates web directories, DNS subdomains, vhosts, Amazon S3 buckets, and Google Cloud Storage by brute force, using specific wordlists and handling the incoming responses. Many security professionals use this tool for penetration testing, bug bounty hunting, and cyber security assessments. Looking at the phases of ethical hacking, we can place Gobuster between the reconnaissance and scanning phases.

We can get help text by entering command ```gobuster --help``` in the terminal.

| Short Flag | Long Flag | Description |
| ---- | ---- | ---- |
| -t | --threads | This flag configures the number of threads to use for the scan. Each of these threads sends out requests with a slight delay. The default number of threads is 10. This number may be slow when using large wordlists. You can increase or decrease the number of threads depending on the available system resources. |
| -w | --wordlist | The flag configures a wordlist to use for iterating. Each wordlist entry is attached to the URL you included in the command. |
| | --delay | This flag defines the amount of time to wait between sending requests. Some web servers include mechanisms to detect enumeration by looking at how many requests are received in a certain period of time. We can increase the delay between subsequent requests to make it look like normal web traffic. |
| | --debug | This flag helps us to troubleshoot when our command gives unexpected errors. |
| -o | --output | This flag writes the enumeration results to a file we choose. |
| | | |

## Example

```bash
gobuster dir -u "http:/www.example.thm/" -w /usr/share/wordlists/dirb/small.txt -t 64
```

- gobuster dir indicates that we will use the directory and file enumeration mode.
- -u "<http://www.example.thm/>" tells Gobuster that the target URL is <http://example.thm/>
- -w /usr/share/wordlists/dirb/small.txt directs Gobuster to use the small.txt wordlist to brute force the web directories. Gobuster will use each entry in the wordlist to form a new URL and send a GET request to that URL. If the first entry or the wordlist were images, Gobuster would send a GET request to <http://example.thm/images/>
- -t 64 sets the number of threads Gobuster will use to 64. This improves performance drastically.

## Use Case: Directory and File Enumeration

Gobuster has a dir mode, allowing users to enumerate website directories and their files. This mode is useful when you are performing a penetration test and would like to see what the directory structure of a website is and what files it contains. Often, directory structures of websites and web apps follow a particular convention, making them susceptible to Brute Force using wordlists. Gobuster is powerful because it allows you to scan the website and return the status codes. These staus codes immediately tell you if you, as an outside user, can request that directory or not.

### Dir-Help

If you want a complete overview of what the Gobuster dir command can offer, you can look at the help page. Seeing the extensive help page for the dir command can somewhat be intimidating. So, we will focus on the most essential flags. Type the following command to display the help: ```gobuster dir --help```

| Flag | Long Flag | Description |
| -c | --cookies | This flag configures a cookie to pass along each request, such as a session ID. |
| -x | --extensions | This flag specifies which file extensions you want to scan for. E.g., php, .js |
| -H | --headers | This flag configures an entire header to pass along with each request. |
| -k | --no-tls-validation | This flag  skips the process that checks the certificate when https is used. It often happens for CTF events or test rooms like the ones on THM a self-signed certificate is used. This causes an error during the TLS check. |
| -n | --no-status | You can set this flag when you don’t want to see status codes of each response received. This helps keep the output on the screen clear. |
| -P | password | You can set this flag together with the --username flag to execute authenticated requests. This is handy when you have obtained credentials from a user. |
| -s | --status-codes | With this flag, you can configure which status codes of the received responses you want to display, such as 200, or a range like 300-400. |
| -b | --status-codes-blacklist | This flag allows you to configure which status codes of the received responses you don’t want to display. Configuring this flag overrides the -s flag. |
| -U | --username | You can set this flag together with the --password flag to execute authenticated requests. This is handy when you have obtained credentials from a user. |
| -r | --followredirect | This flags configures Gobuster to follow the redirect that it received as a response to the sent request. A HTTP redirect status code (e.g., 301 or 302) is used to redirect the client to a different URL. |
| | | |

### How to use dir mode

To run Gobuster in dir mode, use the following command format: ```gobuster dir -u "<http://www.example.thm>" -w /path/to/wordlist```

## Use Case: Subdomain Enumeration

The next mode we’ll focus on is the dns mode. This mode allows Gobuster to brute force subdomains. During a penetration test,  checking the subdomains of your target’s top domain is essential. Just because something is patched in the regular domain, it doesn't mean it is also patched in the subdomain. An opportunity to exploit a vulnerability in one of these subdomains may exist. For example, if TryHackMe owns tryhackme.thm and mobile.tryhackme.thm, there may be a vulnerability in mobile.tryhackme.thm that is not present in tryhackme.thm. That is why it is important to search for subdomains as well!

### DNS-Help

If you want a complete overview of what the Gobuster dns command can offer, you can have a look at the help page. Seeing the extensive help page for the dns command can be intimidating. So, we will focus on the most important flags in this room. Type the following command to display the help: gobuster dns --help

The dns mode offers fewer flags than the dir mode. But these are more than enough to cover most DNS subdomain enumeration scenarios. Let us have a look at some of the commonly used flags:

| Flag | Long Flag | Description |
| --- | --- | --- |
| -c | --show-cname | Show CNAME Records (cannot be used with the -i flag). |
| -i | --show-ips | Including this flag shows IP addresses that the domain and subdomains resolve to. |
| -r | --resolver | This flag configures a custom DNS server to use for resolving. |
| -d | --domain | This flag configures the domain you want to enumerate. |

### How to use DNS mode

To run Gobuster in dns mode, use the following command syntax: ```gobuster dns -d example.thm -w /path/to/wordlist```

## Use Case : Vhost enumeration

The last and final mode we’ll focus on is the vhost mode. This mode allows Gobuster to brute force virtual hosts. Virtual hosts are different websites on the same machine. Sometimes, they look like subdomains, but don’t be deceived! Virtual hosts are IP-based and are running on the same server. Subdomains are set up in DNS. The  difference between vhost and dns mode is in the way Gobuster scans:

- vhost mode will navigate to the URL created by combining the configured HOSTNAME (-u flag) with an entry of a wordlist.
- dns mode will do a DNS lookup to the FQDN created by combining the configured domain name (-d flag) with an entry of a wordlist.

### vhost-Help

 If you want a complete overview of what the Gobuster vhost command can offer, you can have a look at the help page. Seeing the extensive help page for the vhost command can be intimidating. So, we will focus on the most important flags in this room. Type the  following command to display the help: gobuster vhost --help

The vhost mode offers flags similar to those of the dir mode. Let us have a look at some of the commonly used flags:

| Short Flag | Long Flag | Description |
| -u | --url | Specifies the base URL (target domain) for brute-forcing virtual hostnames. |
| | --append-domain | Appends the base domain to each word in the wordlist (e.g., word.example.com). |
| -m | --method | Specifies the HTTP method to use for the requests (e.g., GET, POST). |
| | --domain | Appends a domain to each wordlist entry to form a valid hostname (useful if not provided explicitly). |
| | --exclude-length | Excludes results based on the length of the response body (useful to filter out unwanted responses). |
| -r | --follow-redirect | Follows HTTP redirects (useful for cases where subdomains may redirect) |
