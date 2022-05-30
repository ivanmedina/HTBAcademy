## HTB-ACADEMY (FFUF)
## Ivan Medina
---

### Discovery

##### Discovery page

```
ffuf -w /opt/useful/SecLists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://SERVER_IP:PORT/FUZZ
```

##### Discovery extention

```
ffuf -w /opt/useful/SecLists/Discovery/Web-Content/web-extensions.txt:FUZZ -u http://SERVER_IP:PORT/blog/indexFUZZ
```

##### Discovery page with determinated extention 

```
ffuf -w /opt/useful/SecLists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://SERVER_IP:PORT/blog/FUZZ.php
```

##### Recursive Scanning 

```
ffuf -w /opt/useful/SecLists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://SERVER_IP:PORT/FUZZ -recursion -recursion-depth 1 -e .php -v
```

### Domain fuzzing

##### Add domian to host file 

```
sudo sh -c 'echo "SERVER_IP  academy.htb" >> /etc/hosts'
```

##### Subdomain fuzzing

```
ffuf -w /opt/useful/SecLists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u https://FUZZ.hackthebox.eu/
```

##### VHST Fuzzing

"The key difference between VHosts and sub-domains is that a VHost is basically a 'sub-domain' served on the same server and has the same IP, such that a single IP could be serving two or more different websites".

```
ffuf -w /opt/useful/SecLists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u http://academy.htb:PORT/ -H 'Host: FUZZ.academy.htb'
```

### Filtering options

-  ```-mc```: Match HTTP status codes, or "all" for everything. (default: 200,204,301,302,307,401,403)
-  ```-ml```: Match amount of lines in response
-  ```-mr```: Match regexp
-  ```-ms```: Match HTTP response size
-  ```-mw```: Match amount of words in response

FILTER OPTIONS:
-  ```  -fc ```: Filter HTTP status codes from response. Comma separated list of codes and ranges
-  ```  -fl ```: Filter by amount of lines in response. Comma separated list of line counts and ranges
-  ```  -fr ```: Filter regexp
-  ```  -fs ```: Filter HTTP response size. Comma separated list of sizes and ranges
-  ```  -fw ```: Filter by amount of words in response. Comma separated list of word counts and ranges

### Paramter fuzzing GET

```
ffuf -w wordlist.txt:FUZZ -u http://admin.academy.htb:PORT/admin/admin.php?FUZZ=key -fs xxx
```

### Paramter fuzzing POST

```
ffuf -w wordlist.txt:FUZZ -u http://admin.academy.htb:PORT/admin/admin.php -X POST -d 'FUZZ=key' -H 'Content-Type: application/x-www-form-urlencoded' -fs xxx
```

### Value fuzzing

```
ffuf -w ids.txt:FUZZ -u http://admin.academy.htb:PORT/admin/admin.php -X POST -d 'id=FUZZ' -H 'Content-Type: application/x-www-form-urlencoded' -fs xxx
```

### Wordlists

- /opt/useful/SecLists/Discovery/Web-Content/directory-list-2.3-small.txt	Directory/Page Wordlist
- /opt/useful/SecLists/Discovery/Web-Content/web-extensions.txt	Extensions Wordlist
- /opt/useful/SecLists/Discovery/DNS/subdomains-top1million-5000.txt	Domain Wordlist
- /opt/useful/SecLists/Discovery/Web-Content/burp-parameter-names.txt	Parameters Wordlist

