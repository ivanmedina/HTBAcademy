## HTB-ACADEMY (Linux Fundamentals/ Files)
## Ivan Medina
---

### Philosophy

- Everthing is a file.

### File System

| Path   |      Description      |
|----------|:-------------:|
| /bin |  Esentials Command binaries. |
| /boot |    Bootloader, kernel and required files for boot.   |
| /dev | Devices folder in system. |
| /lib | Shared Library files required by system. |
| /media | External devices mounted. |
| /mnt | Temporary mounted file systems. |
| /opt | Optional folders for some programs. |
| /root | Root User home folder. |
| /sbin | Sysadmin Executables.  |
| /usr | Executables, libraries, manuals, etc. |
| /var | Los files, email, web apps, cron tasks, etc. |
| /tmp | Temporary folder visible to everyone. |
| /etc | Local configuration files. |
| /home | Users folders. |

### File Descriptors

| File Descriptor   |      Description      |
|----------|:-------------:|
| 0 |  STDIN |
| 1 |    STDOUT   |
| 2 | STDERR |

### Permissions

``` - rwx rw- r-- 1 root root 12 some_date / file.txt  ```
| File | Owner | Group | Other | Links | User | Group | Size |  Date | Filename |
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| - |  rwx | rw |  r-- | 1 |  root | root |  12 | some_date |  file.txt |

### Octal representation.

| Number | Equivalent |
|:-:|:-:|
| 0 |  --- |
| 1 |  --x |
| 2 |  -w- |
| 3 |  -wx |
| 4 |  r-- |
| 5 |  r-x |
| 6 |  rw- |
| 7 |  rwx |

### Change permissions.

``` chmod <octal representation> <file> ```

### Change user/group owner.

``` chown <user>:<group> <file> ```

### Find Files and dirs.

- ``` which <file> ```
- ``` find <location> <options> ```
- ``` locate <pattern> ```
- ``` updatedb ```

### Filter Outputs

- ``` head ```
- ``` sort ```
- ``` grep ```
- ``` grep -v  #exculde match ```
- ``` cut -d <"delimiter"> -f<position> ```
- ``` tr <"to_remove"> <"remplace"> $column -t ```
- ``` awk '{print $n $NF}' ```
- ``` sed 's/bin/HTB/g' ```
- ``` wc -l ```

### tr

##### Convert Lower-upper

- ``` tr 'a-z' 'A-Z' <file> ```

##### Find Replace

- ``` tr <pattern_find> <pattern_replace> <file> ```

##### Cesar Cipher

- ``` tr 'a-z' 'e-zabcd' ```
- ``` tr 'e-zabcd' 'a-z'  ```

##### Truncate normal

When are same size works, when not use padding last char.

``` tr -t 'abcdef' 'ABC' -S '' ```

- ``` -S ```: avoid repeating
- ``` -d 'x' ```: delete spec chars

### wc

- ``` wc -t <file> ```: count lines.
- ``` wc -w <file> ```: count words.
- ``` wc -c <file> ```: count bcytes.
- ``` wc -m <file> ```: count chars.
- ``` wc -L <file> ```: longest line.

### cut

- ``` cut <opc> <file> ```: replace.
- ``` -b --bytes=[] ```
- ``` -c, --chars...=LIST ```
- ``` -d, --delim...=DELIM ```
- ``` -f, --fields...=LIST ```
- ``` -b 1-[2-5] ```
- ``` -b -4 ```
- ``` -c -2,4,6 ```

### awk

- ``` awk <opt> 'criteria {action} <inputfile> ```
- ``` awk {print} <inputfile> ```
- ``` awk '/pattern/' {print} <inputfile> ```
- ``` awk '{prin$1,$4}' ```
- ``` awk '{prin$1,$4}' ```

### fwd

- ``` -type f ```
- ``` -name *.conf ```
- ``` -user root ```
- ``` -size +20k ```
- ``` -newermt 2020-0303 ```