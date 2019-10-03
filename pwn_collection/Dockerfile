FROM ubuntu:16.04

RUN sed -i "s/http:\/\/archive.ubuntu.com/http:\/\/mirrors.tuna.tsinghua.edu.cn/g" /etc/apt/sources.list && \
    apt-get update && apt-get -y dist-upgrade && \
    apt-get install -y lib32z1 xinetd ssh

RUN groupadd ciscn && \
	useradd -g ciscn ciscn -m && \
	password=$(openssl passwd -1 -salt 'abcdefg' '123456') && \
	sed -i 's/^ciscn:!/ciscn:'$password'/g' /etc/shadow


WORKDIR /home/ciscn

RUN cp -R /lib* /home/ciscn && \
    cp -R /usr/lib* /home/ciscn

RUN mkdir /home/ciscn/dev && \
    mknod /home/ciscn/dev/null c 1 3 && \
    mknod /home/ciscn/dev/zero c 1 5 && \
    mknod /home/ciscn/dev/random c 1 8 && \
    mknod /home/ciscn/dev/urandom c 1 9 && \
    chmod 666 /home/ciscn/dev/*

RUN mkdir /home/ciscn/bin && \
    cp /bin/sh /home/ciscn/bin && \
    cp /bin/ls /home/ciscn/bin && \
    cp /bin/cat /home/ciscn/bin

COPY ./ciscn.xinetd /etc/xinetd.d/ciscn
COPY ./start.sh /start.sh
RUN echo "Blocked by ciscn_xinetd" > /etc/banner_fail

RUN chmod +x /start.sh

COPY ./bin/ /home/ciscn/
RUN chown -R ciscn /home/ciscn && \
    chmod -R 777 /home/ciscn && \
    chmod 777 /home/ciscn/flag


CMD ["/start.sh"]

EXPOSE 9999
