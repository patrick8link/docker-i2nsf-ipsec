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


#Install cfgipec2, not compilation required, only for xml configuration examples
RUN set -e -x; \
	git clone https://github.com/patrick8link/i2nsf-ipsec.git; \
    cd i2nsf-ipsec; 

ENV SRC_PATH /home/netconf/i2nsf-ipsec
WORKDIR $SRC_PATH


RUN apt-get update && apt-get install -y openssh-server
RUN mkdir /var/run/sshd
RUN sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config
EXPOSE 22

