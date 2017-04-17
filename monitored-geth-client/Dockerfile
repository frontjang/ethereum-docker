FROM ethereum/client-go

RUN apt-get update &&\
    apt-get install -y curl git-core &&\
    curl -sL https://deb.nodesource.com/setup_4.x | bash - &&\
    apt-get update &&\
    apt-get install -y nodejs

RUN cd /root &&\
    git clone https://github.com/cubedro/eth-net-intelligence-api &&\
    cd eth-net-intelligence-api &&\
    npm install &&\
    npm install -g pm2

ADD geth.patch /root/geth.patch
RUN cd / && git clone --depth 1 --branch release/1.5 https://github.com/ethereum/go-ethereum && \
	cd /go-ethereum && patch -p1 -l < /root/geth.patch

RUN apt-get install -y --no-install-recommends golang git make gcc libc-dev ca-certificates && \
	cd /go-ethereum && make geth && cp build/bin/geth /geth

ADD start.sh /root/start.sh
ADD app.json /root/eth-net-intelligence-api/app.json
RUN chmod +x /root/start.sh

ENTRYPOINT /root/start.sh