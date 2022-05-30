## HTB-ACADEMY (Nmap)
## Ivan Medina
---

### Scan options

- ```-sn```:	Disables port scanning.
- ```-Pn```:	Disables ICMP Echo Requests
- ```-n```:	Disables DNS Resolution.
- ```-PE```:	Performs the ping scan by using ICMP Echo Requests against the target.
- ```--packet-trace```:	Shows all packets sent and received.
- ```--reason```:	Displays the reason for a specific result.
- ```--disable-arp-ping```:	Disables ARP Ping Requests.
- ```--top-ports=<num>```:	Scans the specified top ports that have been defined as most frequent.
- ```-p-```:	Scan all ports.
- ```-p22-110```:	Scan all ports between 22 and 110.
- ```-p22,25```:	Scans only the specified ports 22 and 25.
- ```-F```:	Scans top 100 ports.
- ```-sS```:	Performs an TCP SYN-Scan.
- ```-sA```:	Performs an TCP ACK-Scan.
- ```-sU```:	Performs an UDP Scan.
- ```-sV```:	Scans the discovered services for their versions.
- ```-sC```:	Perform a Script Scan with scripts that are categorized as "default".

### Output options

- ```oA filename```:	Stores the results in all available formats starting with the name of "filename".
- ```oN filename```:	Stores the results in normal format with the name "filename".
- ```oG filename```:	Stores the results in "grepable" format with the name of "filename".
- ```oX filename```:	Stores the results in XML format with the name of "filename".

### Performance options

- ```--max-retries <num>```	Sets the number of retries for scans of specific ports.
- ```--stats-every=5s```	Displays scan's status every 5 seconds.
- ```-v/-vv```	Displays verbose output during the scan.
- ```--initial-rtt-timeout 50ms```	Sets the specified time value as initial RTT timeout.
- ```--max-rtt-timeout 100ms```	Sets the specified time value as maximum RTT timeout.
- ```--min-rate 300```	Sets the number of packets that will be sent simultaneously.
- ```-T <0-5>```	Specifies the specific timing template.

### Host discovery

- Scan network range

```
sudo nmap 10.129.2.0/24 -sn -oA tnet | grep for | cut -d" " -f5
```

- Scan multiples ips

```
sudo nmap -sn -oA tnet 10.129.2.18 10.129.2.19 10.129.2.20| grep for | cut -d" " -f5
```

```
sudo nmap -sn -oA tnet 10.129.2.18-20| grep for | cut -d" " -f5
```

- Scan if ip is alive

```
sudo nmap 10.129.2.18 -sn -oA host
```

```
sudo nmap 10.129.2.18 -sn -oA host -PE --reason
```

```
sudo nmap 10.129.2.18 -sn -oA host -PE --packet-trace --disable-arp-ping
```

### Host and Port scanning

- Scan top n ports

```
sudo nmap 10.129.2.28 --top-ports=10
```

- Trace packets

```
sudo nmap 10.129.2.28 -p 21 --packet-trace -Pn -n --disable-arp-ping
```

- Trace packets

```
sudo nmap 10.129.2.28 -p 21 --packet-trace -Pn -n --disable-arp-ping
```

- UDP ports

```
sudo nmap 10.129.2.28 -F -sU
```

```
sudo nmap 10.129.2.28 -sU -Pn -n --disable-arp-ping --packet-trace -p 137 --reason
```

- Service Enumeration

```
sudo nmap 10.129.2.28 -p- -sV --stats-every=5s -v
```