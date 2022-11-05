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
RUN apt install -y software-properties-common
RUN apt-add-repository ppa:deadsnakes/ppa
RUN apt install -y python3.8
RUN curl "https://bootstrap.pypa.io/pip/3.6/get-pip.py" -o "get-pip.py"
RUN python3.8 get-pip.py
RUN pip install ncclient
# RUN apt-get install -y python3-dev gcc python3-cffi


#Install cfgipec2, not compilation required, only for xml configuration examples
RUN set -e -x; \
	git clone https://github.com/patrick8link/i2nsf-ipsec.git; \
    cd i2nsf-ipsec; 

ENV SRC_PATH /home/netconf/i2nsf-ipsec
WORKDIR $SRC_PATH



COPY misc/routes.sh .
#RUN chmod +x routes.sh
#ENTRYPOINT []
#CMD ["./routes.sh"]

COPY misc/supervisord.conf /etc/supervisord.conf
CMD ["/usr/bin/supervisord", "-c", "/etc/supervisord.conf"]

RUN apt-get update && apt-get install -y openssh-server
RUN mkdir /var/run/sshd
RUN sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config
EXPOSE 22





