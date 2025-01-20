We can do ip scan without nmap via bash using below for loop.
for i in {1..255}; do (ping -c 1 192.168.1.${i} | grep "bytes from" &); done
It is also called ping sweep.

To check for open ports without useing nmap we can do the following
for i in {1..65535}; do (echo > /dev/tcp/192.168.1.1/$i) >/dev/null 2>&1 && echo $i is open; done