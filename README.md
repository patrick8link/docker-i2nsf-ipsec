# Acknowledgment
This repository updates the work of https://gitlab.atica.um.es/gabilm.um.es/cfgipsec2/tree/master to follow current release of RFC9061.

Original work by:
### Professors:

- Rafael Marín López (rafa at um dot es, University of Murcia)
- Fernando Pereñiguez García (fernando dot pereniguez at cud dot upct dot es, University Defense Center, CUD)
- Gabriel López Millán (gabilm at um dot es, University of Murcia)

### Collaborators:

- Alejandro Pérez Méndez

### Students:

- Felipe Roca Blaya
- Ignacio Martínez Alpiste
- Rubén Ricard Sánchez
- Adrián Miralles Palazón
- Javier Pastor Galindo
- Valentin Kivachuck

Current release contributors:
### Students:
- Patrick Lingga
- Uhm Jiyong

# Docker compose for instances of sysrepo-netopeer2 with cfgipsec2 support


This docker compose feeds from [Dockerfile for sysrepo-netopeer2](https://github.com/sysrepo-archive/docker-sysrepo-netopeer2) and from [cfgipsec: IPsec SAs configuration](https://github.com/patrick8link/i2nsf-ipsec).

## Scenarios:

- Gw-2-Gw SA with ESP in tunnel mode (case 1 and case 2). Set up a basic gateway to gateway scenario with manual controller. 

- Host-2-Host SA with ESP in transport mode (case 1 and case 2). Set up a basic host to host scenario with manual controller. 

- N Host SA with ESP in transport mode: Set up a NxN host to host scenario dynamically managed by a python-based controller. 


## Try it:

To test the different scenarios:

`# git clone https://github.com/patrick8link/i2nsf-ipsec.git `

`# cd i2nsf-ipsec`

`# cd scenario_name`

Follow the README.md instructions.






 




















