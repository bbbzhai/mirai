# Mirai System Requirements

The environment requires a minimum of 3 VMs (1 for CNC, 1 for Scan Listen/Loading + DNS, and 1 for Victim).

For our environment we used:

### CNC (IP: 10.10.10.2)

#### OS: Ubuntu Desktop 14.04 LTS (64-bit)

#### Name: cnc

#### Programs

* MySQL Server (For CNC database)

#### Binaries run

* ./mirai.dbg
* ./cnc


### Scan Listen (IP: 10.10.10.5)

#### OS: Ubuntu Desktop 16.04 LTS (64-bit)

#### Name: ns1

#### Programs

* Bind (For DNS Server)
* Apache Server (To load binaries)

#### Binaries run

* ./scanListen
* ./loader.dbg

### Victim (IP: 10.10.10.7)

#### OS: Ubuntu Server 14.04 LTS (64-bit)

#### Name: victim3

#### Programs

* Busybox (Mirai specifically targets IoT devices with busybox installed)

#### Binaries run

* ./mirai.dbg