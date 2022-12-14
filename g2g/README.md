# Docker compose for instances of sysrepo-netopeer2 with i2nsf-ipsec support

The example is based on the following scenario:



                                         controller 
                                        netopeer-cli>   
                                            (.200)  
                                              |               
                  /---------------------(10.0.1.0/24)-------------------\
                 /                      /           \                    \
                /                      /             \                    \
               /                      /               \                    \
          (.102)                   (.204)            (.234)               (.18)
            h1                       gw1               gw2                  h2
          (.254)------------------(.2)(.100) ==IPSEC==(.200)(.2)------------(.254)
               (192.168.201.0/24)        (192.168.123.0/24)        (192.168.202.0/24)    



# GW-2-GW SAs ESP tunnel mode - Case 1 (IKE case)


## Run the testbed:

`\# sudo ./up.sh`

## Check containers are running:

`\# docker-compose ps`


## Connect to the Netconf server 1 and launch the i2nsf-ipsec service:

`\# docker exec -it g2g_gw1_1  /bin/bash`

`\# ./ietf-i2nsf-ike -v 2`


## Connect to the Netconf server 2 and launch the cfgipsec2 service:

`\# docker exec -it g2g_gw2_1  /bin/bash`

`\# ./ietf-i2nsf-ike -v 2`


## Connect to the Netconf client:

`\# docker exec -it g2g_c_1 /bin/bash`

`\# netopeer2-cli`


## Configure IPsec ESP host-2-host transport mode between h1 and h2

`>connect --host 10.0.1.204 --ssh --login netconf (password: netconf)`

`>subscribe`

`>edit-config --target running --config=/home/netconf/i2nsf-ipsec/client-xmls/g2g-ike-case-tunnel-mode-gw1.xml`

`>get-config --source=running`

`>disconnect`

`>connect --host 10.0.1.234 --ssh --login netconf (password: netconf)`

`>subscribe`

`>edit-config --target running --config=/home/netconf/i2nsf-ipsec/client-xmls/g2g-ike-case-tunnel-mode-gw2.xml`

`>get-config --source=running`

`>disconnect`

## Test

Check SPD state in gw1 and gw2. For example, in gw1:

`\# docker exec g2g_gw1_1 swanctl -L`

From h1, test ping to h2

`\# docker exec g2g_h1_1 ping 192.168.202.254`

Run tcpdump in gw1 or gw2. For example, in gw1:

`\# docker exec g2g_gw1_1 tcpdump -i eth1 -n esp`

(can take a while to show the ESP packets)


## Stop the testbed:

`\# sudo ./down.sh`
