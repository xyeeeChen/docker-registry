FROM alpine
LABEL maintainer="xyChen <god455108@gmail.com>"
RUN apk --update add gcc build-base cmake && \
    rm -rf /var/lib/apt/lists/* && \
    rm /var/cache/apk/*
RUN apk add --virtual .build-deps git autoconf libtool automake && \
    git clone https://github.com/redis/hiredis.git && \
    cd hiredis && make && make install && cd .. && \
    wget http://dist.schmorp.de/libev/libev-4.24.tar.gz && \
    tar -xvf libev-4.24.tar.gz && cd libev-4.24 && ./configure && \
    make && make install && cd .. && \
    git clone https://github.com/maxmind/geoip-api-c.git && \
    cd geoip-api-c && ./bootstrap && ./configure && make && make check && make install && \
    apk del .build-deps && \
    rm -rf /var/lib/apt/lists/* && \
    rm /var/cache/apk/*
