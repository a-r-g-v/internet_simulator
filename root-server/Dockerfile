FROM debian:wheezy

RUN apt-get update && apt-get -y upgrade && apt-get -y install chkconfig
RUN apt-get install -y net-tools adduser chkconfig vim debhelper

### nsd4 build-dep
RUN apt-get install -y make gcc g++ build-essential wget autoconf bison flex libssl-dev
RUN echo 'deb http://cdn.debian.net/debian/ wheezy main contrib non-free\ndeb-src http://cdn.debian.net/debian/ wheezy main contrib non-free' > /etc/apt/sources.list && \
    apt-get update && apt-get build-dep nsd3
RUN apt-get install -y libevent-dev

### nsd4 build
RUN mkdir -p /tmp/nsd
WORKDIR /tmp/nsd
RUN wget https://nlnetlabs.nl/downloads/nsd/nsd-4.1.2.tar.gz --no-check-certificate && \
    tar zxvf nsd-4.1.2.tar.gz && \
    cd nsd-4.1.2 && \
    ./configure --enable-root-server && \
    make && \
    make install

### nsd4 init-script
ADD init-script /etc/init.d/nsd

RUN mkdir /problem
ADD . /problem
ADD /nsd /etc/nsd

    

WORKDIR /problem
RUN mkdir -p /var/run/nsd && chown root /var/run/nsd && /usr/local/sbin/nsd-control-setup

RUN /usr/local/sbin/nsd-control start && /usr/local/sbin/nsd-control reload && /usr/local/sbin/nsd-control stop

RUN chkconfig nsd on  


CMD ["/sbin/init"]
