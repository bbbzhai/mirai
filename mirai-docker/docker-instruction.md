# Instruction for Mirai Docker

## step 1: build docker
```
docker build -t miraidocker .
```

## step 2: run docker 
```
docker run -i -t miraidocker /bin/bash
```

## step 3: set up database
I can't quite figure out how to configure database in the dockerfile so my solution right now is to get into the container and set up manually, the code to run is:
```
1. /etc/init.d/mysql stop.
2. mysqld_safe --skip-grant-tables &
3. mysql -u root.
4. use mysql;
5. update user set password=PASSWORD("root") where User='root';
6. flush privileges;
7. quit.
```

**End of configuration**
***

