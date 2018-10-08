FROM buildpack-deps

RUN apt-get update && apt-get install -y \
		ca-certificates \
		curl

# verify gpg and sha256: http://nodejs.org/dist/v0.10.31/SHASUMS256.txt.asc
# gpg: aka "Timothy J Fontaine (Work) <tj.fontaine@joyent.com>"
#RUN gpg --keyserver pool.sks-keyservers.net --recv-keys 7937DFD2AB06298B2293C3187D33FF9D0246406D

ENV NODE_VERSION 0.10.25
ENV NPM_VERSION 1.3.24

RUN curl -SLO "http://nodejs.org/dist/v$NODE_VERSION/node-v$NODE_VERSION-linux-x64.tar.gz" \
#	&& curl -SLO "http://nodejs.org/dist/v$NODE_VERSION/SHASUMS256.txt.asc" \
#	&& gpg --verify SHASUMS256.txt.asc \
#	&& grep " node-v$NODE_VERSION-linux-x64.tar.gz\$" SHASUMS256.txt.asc | sha256sum -c - \
	&& tar -xzf "node-v$NODE_VERSION-linux-x64.tar.gz" -C /usr/local --strip-components=1 \
	&& rm "node-v$NODE_VERSION-linux-x64.tar.gz" \
#	&& rm SHASUMS256.txt.asc \
	&& npm install -g npm@"$NPM_VERSION" \
	&& npm cache clear

RUN cd /opt && \
    mkdir stratum-server && cd stratum-server && \
    git clone https://github.com/zone117x/node-stratum-pool && \
    cp node-stratum-pool/package.json package.json && \
    npm install && \
    mv node-stratum-pool node_modules/node-stratum-pool

#RUN npm install -g node-inspector

ADD stratum-server /opt/stratum-server
#ADD rootfs /
#COPY config.json /usr/src/app/config.json

EXPOSE 3333 3334 9229

WORKDIR /opt/stratum-server
#CMD [ "bash" ]
#CMD [ "node", "--inspect=0.0.0.0:9229","server.js" ]
CMD [ "node", "server.js" ]
