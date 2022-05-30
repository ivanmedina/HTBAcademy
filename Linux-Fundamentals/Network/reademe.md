## HTB-ACADEMY (Linux Fundamentals/ Network)
## Ivan Medina
---

### IP network

##### Assign ip/netmask

- ``` ifconfig eth0 <addr> netmask <addr> ```
- ``` if up/ if down eth0 ```
- ``` ip a show eth0 ```

##### Hops to network resource

- ``` tracerout -n <ip> ```

##### Hops to network resource

- ``` tracerout -T <ip> ```

##### netstat

- ``` netstat -p  ```: programs
- ``` netstat -s  ```: ports
- ``` netstat -r  ```: routing

##### ss

- ``` ss -t4 state closed  ```
- ``` ss -t4 state established  ```
- ``` ss dst <ip>  ```

##### dig

``` dig <option> <domain>   ```
- A -> IPV4
- AAAA -> IPV6
- CNAME
- HINFO
- MX
- NS
- PTR
- PTRSOA
- TXT

 ``` dig axfr <domain> @ <ip>  ``` 

##### tcpdump

- ``` -D  ```:  Display interfaces.
- ``` -i  ```: select interface.
- ``` -n  ```: do not resolve hostname.
- ``` -nn  ```: well known ports.
- ``` -e  ```: get headers.
- ``` -x  ```: hex & ascii.
- ``` -xx  ```: hex & ascii with headers.
- ``` -v,vv,vvv  ```: verbose.
- ``` -c  ```: limit number of packs to intercept.


##### tcp filters

- host
- src/dest
- net
- proto
- port
- portrange
- less/greater
- AND / && / or / NOT


##### curl

- ``` -d, --data  'user=a&password=123' ``` : Data for post request.
- ``` -I, --include  ```: Include headers.
- ``` -o, --output  ```
- ``` -silent, --silent  ```
- ``` -u, --user <user:password> ```
- ``` -A, --user-Agent <useragent> ```
- ``` -K, -skip ssl ```
- ``` -H  ```: headers
- ``` -i  ```: headers and response
- ``` -X  ```: POST
- ``` -b <cookie>  ```

#### headers
[Headers reference.](https://www.ibm.com/docs/en/app-connect/11.0.0?topic=messages-http-headers)
