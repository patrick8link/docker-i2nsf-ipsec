FROM sysrepo/sysrepo-netopeer2:legacy

RUN cd /opt/dev && \
    cd libyang && git checkout v1.0.184 && \
    cd build && \ 
    cmake -DCMAKE_INSTALL_PREFIX=/usr -DCMAKE_BUILD_TYPE="Debug" -DENABLE_BUILD_TESTS=OFF .. && \
    make -j2 && make install

ENV SRC_PATH /home/netconf
WORKDIR $SRC_PATH

RUN apt-get update
RUN apt-get install -y iproute2 tcpdump iputils-ping curl libpam0g-dev autotools-dev autoconf
RUN apt-get install -y strongswan strongswan-swanctl

#Install strongswan
ARG STRONGSWAN_VERSION=5.5.0
ENV STRONGSWAN_VERSION_BUILD=$STRONGSWAN_VERSION

RUN set -e -x; \
      curl -O https://download.strongswan.org/strongswan-$STRONGSWAN_VERSION_BUILD.tar.bz2; \
      tar xvf strongswan* strongswan-5.5.0/src/libcharon/plugins/vici/libvici.h; 

RUN set -e -x; cp /home/netconf/strongswan-$STRONGSWAN_VERSION_BUILD/src/libcharon/plugins/vici/libvici.h /usr/local/include/;

ENV LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/lib/ipsec:/usr/lib/ipsec/

#Install cfgipec2
RUN set -e -x; \
	git clone https://github.com/patrick8link/i2nsf-ipsec.git; \
	cd i2nsf-ipsec; \
    git checkout wldyd; \
    git pull;

ENV SRC_PATH /home/netconf/i2nsf-ipsec
WORKDIR $SRC_PATH


COPY misc/supervisord.conf /etc/supervisord.conf
CMD ["/usr/bin/supervisord", "-c", "/etc/supervisord.conf"]

#Debug
EXPOSE 830

#SSH
RUN apt-get update && apt-get install -y openssh-server
RUN mkdir /var/run/sshd
RUN sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config
EXPOSE 22