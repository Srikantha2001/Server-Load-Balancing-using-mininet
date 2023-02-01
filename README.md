# Server Load Balancing using Mininet

This project is a demonstration of server load balancing using Mininet. This involve a custom load balancer that takes into account all the live servers present in the network and send the traffic to server with least traffic. 

## Prerequisites
1. [Mininet](http://mininet.org/)

## Setup

1. Installation of dependencies in requirement.txt
```Shell
pip install -r requirements.txt
```

2. Remove previous residues of mininet (if any) using the command
```Shell
sudo mn -c
```

![Sample output of mn -c](/Resources/mn_minusc.png)


3. Create a topology in mininet using 6 hosts ( 3 for client and 3 for server) using the command 
```Shell
sudo mn --topo=single,6 --controller=remote,port=6633
```

 - *topo*  =  represent the topology of the network
 - *controller*  =  represent the controller used for this network ( since we are using an external controller (POX), argument is set as *remote*)
 - *port* = represent the port number (6633 is default port of POX controller)

 ![Sample output of topo command](/Resources/mn_topo.png)

4. Create terminal for each of the 6 hosts using the command

```Shell
    xterm h1 h2 h3 h4 h5 h6
```

this command will create 6 new terminals. each host have the ip address **10.0.0.x** where **x** is host number (eg: h3 => 10.0.0.3)

5. First 3 terminal is considered as host (h1 h2 h3). In all these terminals run a  HTTP server using the command (in default port 80)

```Shell
    python3 -m http.server 80
```

6. Now open a new terminal and execute following commands

```Shell
cp custom_load_balancer.py pox/pox/misc/

pox/pox.py log.level --DEBUG misc.custom_load_balancer --ip=10.0.1.1 --servers=10.0.0.1,10.0.0.2,10.0.0.3
```

this sets up the pox controller which will use 10.0.1.1 ip exposed to public and all traffic comming to this ip is shared between the servers.

6. In the remaining xterm (h4 h5 h6) make a curl request to the ip address **10.0.1.1** using the command 
```Shell
curl 10.0.1.1
```

the load_balancing effect can be observed

the final output looks something like this
![Sample final output](/Resources/finalOutput.png)


