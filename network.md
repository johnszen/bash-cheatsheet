# Network

Display resolver cache - current name to ip list
`ipconfig /displaydns`

`nslookup www.google.com`

- check order at */etc/host.conf* (linux)

  `order host,bind`
- look at 

  *c:\windows\system32\drivers\etc\hosts* (on Windows)

  *look at /etc/hosts* (linux)
- query DNS server specified at
    *network setting / DHCP* (Windows)
  
    */etc/resolv.conf / DHCP* (Linux)
    
    ```
    search tasitii.com
    nameserver 192.1.2.3
    nameserver 192.2.3.4
    ```
  - if it is authoritative return
  - in cache return
  - look for root servers (root-servers.org)
  
    *C:\Windows\System32\Dns\Cache.dns* (Windows)
    
    */var/named/named.root* (Linux)
    
  - look for next level domain name e.g. com (iana.org)
  - look for next level domain name e.g. google (verisign)
  - ...
