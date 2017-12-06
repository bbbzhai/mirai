FROM ubuntu:xenial
MAINTAINER Bob Zhai <yzhai8@gatech.edu>

#set the working directory and copy over all contents
WORKDIR /app
ADD . /app

ARG DEBIAN_FRONTEND=noninteractive

#Install mysql-server
RUN echo "#!/bin/sh\nexit 0" > /usr/sbin/policy-rc.d &&\
	apt-get -y update && apt-get install -y --no-install-recommends apt-utils &&\
	apt-get install -y debconf-utils &&\
    echo mysql-server mysql-server/root_password password root | debconf-set-selections &&\
    echo mysql-server mysql-server/root_password_again password root | debconf-set-selections &&\
    apt-get install -y mysql-server -o pkg::Options::="--force-confdef" -o pkg::Options::="--force-confold" --fix-missing &&\
    apt-get install mysql-client &&\
    mysql -u root -p root < /app/db.sql


EXPOSE 23

CMD service mysql start &&\
	/app/cnc
