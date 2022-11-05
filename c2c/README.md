# Docker compose for instances of sysrepo-netopeer2 with i2nsf-ipsec support

The example is based on the following scenario:

 
    
                         I2NSF 1                                                     I2NSF 2         
        +-------------------------------------------+         +-------------------------------------------+
        |           +-------------+                 |         |                +-------------+            |
        |           |   Security  |(.100)         172.24.4.0/24          (.200)|   Security  |            |
        |           | Controller 1|<------------------------------------------>| Controller 2|            |
        |           |     (c1)    |                 |Interface|                |     (c2)    |            |
        |           +------+------+                 |         |                +------+------+            |
        |                  ^(.200)                  |         |                       ^(.200)             |
        |                  |                        |         |                       |                   |
        |      +------------+------------+          |         |          +------------+------------+      |
        |      |        10.0.1.0/24      |          |         |          |        10.0.2.0/24      |      |
        |(.102)v                         v (.204)   |         |   (.234) v                         v(.18) |
        |  +---+---+(.254)        (.2)+---+---+   192.168.123.0/24  +---+---+(.2)        (.254)+---+---+  |
        |  |  h1   |------------------|  gw1  |========IPSEC========|  gw2  |------------------|  h2   |  |
        |  +---+---+ 192.168.201.0/24 +---+---+     |         |     +---+---+ 192.168.202.0/24 +---+---+  | 
        +-------------------------------------------+         +-------------------------------------------+ 
          
     
# GW-2-GW SAs ESP tunnel mode - IKE case  
   
   
## Run the testbed:

`\# sudo ./up.sh`

## Check containers are running:

`\# sudo docker-compose ps`


## Connect to Controller 1 (c1):

`\# sudo docker exec -it c2c_c1_1 /bin/bash`
`\# cd controller`
`\# make`
`\# make install`
`\# ./ietf-i2nsf-controller -v 2`

## Connect to Gateway 1 (gw1):

`\# sudo docker exec -it c2c_gw1_1  /bin/bash`

`\# ./ietf-i2nsf-ike -c case1 -v 2`

## Connect to Gateway 2 (gw2):

`\# sudo docker exec -it c2c_gw2_1  /bin/bash`

`\# ./ietf-i2nsf-ike -c case1 -v 2`

## Connect to the Controller 2 (c2):
`\# sudo docker exec -it c2c_c2_1 /bin/bash`

`\# netopeer2-cli`

## Configure IPSEC for gw2
`> connect --host 10.0.2.234 --ssh --login netconf (password: netconf)`
`> subscribe`
`> edit-config --target running --config=/home/netconf/i2nsf-ipsec/client-xmls/g2g-ike-case-tunnel-mode-gw2.xml`
`> get-config --source=running`
`> disconnect`

## Configure IPSEC for gw1 through Controller 1
`> connect --host 172.24.4.100 --ssh --login netconf (password: netconf)`
`> subscribe`
`> edit-config --target running --config=/home/netconf/i2nsf-ipsec/client-xmls/g2g-ike-case-tunnel-mode-gw1.xml`
`> get-config --source=running`
`> disconnect`


## Test

Check SPD state in gw1 and gw2. For example, in gw1:

`\# docker exec c2c_gw1_1 swanctl -L`

From h1, test ping to h2

`\# docker exec c2c_h1_1 ping 192.168.202.254`

Run tcpdump in gw1 or gw2. For example, in gw1:

`\# docker exec c2c_gw2_1 tcpdump -i eth0 esp`

(can take a while to show the ESP packets)


## Stop the testbed:

`\# sudo ./down.sh`
