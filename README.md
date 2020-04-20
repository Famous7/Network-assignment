# PA_1 readme

## Required packages
- networkx==2.4

## 1. Shortest path routing
Please type 'pingall' in a mininet console, as soon as you execute the shortest.py

1. Launch command
 
 ```ryu-manager shortest.py --observe-links
 mininet> pingall
 mininet> h1 ping -c1 h5```

2. Print out the shortest path between two host in the network topology
```
shortest path = host[00:00:00:00:00:02] -> sw[2] -> sw[1] -> sw[4] -> host[00:00:00:00:00:04]
```

## 2. Network Statistics Collection

1. Launch command

 ```ryu-manager netstats.py --observe-links```
 
 ```mininet> pingall```

2. Print out the information about port_stats_reply and flow_stats_reply every 5 seconds
```
------------------------------ [PORT_STATES] ------------------------------
datapath                port           duration_nsec    rx-pkts    tx-pkts
----------------        ----------     --------------   --------   --------
0000000000000003                 1          970000000        175        175
0000000000000003                 2          970000000        169        166
0000000000000003                 3          968000000         25        205
0000000000000003        4294967294          973000000          0          8

--------------------------------------------------- [FLOW_STATES] ---------------------------------------------------
datapath              packet_count       byte_count         duration_nsec        ether_src          ether_dst
----------------      -------------      -------------      ---------------      -----------------  -----------------
0000000000000004                232              37278            644000000      *****************  *****************
0000000000000004                  3                238             54000000      *****************  00:00:00:00:00:02
0000000000000004                  2                140            982000000      *****************  00:00:00:00:00:05
0000000000000004                  2                140             10000000      *****************  00:00:00:00:00:04
0000000000000004                  3                238            103000000      *****************  00:00:00:00:00:01
0000000000000004                  9                658             98000000      *****************  00:00:00:00:00:04
0000000000000004                  3                238             15000000      *****************  00:00:00:00:00:03
0000000000000004                 40              13380            644000000      *****************  ff:ff:ff:ff:ff:ff
0000000000000004                 74               4440            638000000      *****************  01:80:c2:00:00:0e
```

## 3. Network Tapping
Please type 'pingall' in a mininet console, as soon as you execute the nettap.py

1. Launch command

 ```python nettap_Topo.py```
 
 ```ryu-manager netstats.py --observe-links```
 
 ```mininet> pingall```
 
 ```mininet> xterm h1 h2 h4```
 
 ```h1> tcpdump -i h1-eth0 -xx tcp```
 
 ```h2> tcpdump -i h2-eth0 -xx tcp```
 
 ```h4> python -m SimpleHTTPServer 80```
 
 ```mininet> h2 curl http://10.0.0.4:80```

2. The results for tcpdump in h1 and h2 are same
```
22:00:19.642890 IP 10.0.0.4.http > 10.0.0.2.46736: Flags [.], ack 74, win 57, options [nop,nop,TS val 72987097 ecr 72987094], length 0
	0x0000:  0000 0000 0002 0000 0000 0004 0800 4500
	0x0010:  0034 40d4 4000 4006 e5ea 0a00 0004 0a00
	0x0020:  0002 0050 b690 b174 daea 4755 6c26 8010
	0x0030:  0039 ff60 0000 0101 080a 0459 b1d9 0459
	0x0040:  b1d6

11 packets captured
11 packets received by filter
0 packets dropped by kernel
```

## 4. L3 load balancing
Please type 'pingall' in a mininet console, as soon as you execute the LB.py

1. Launch command

 ```python LB_Topo.py```
 
 ```ryu-manager LB.py --observe-links```
 
 ```mininet> pingall```
 
 ```mininet> xterm h1 h2 h4```
 
 ```h1> python -m SimpleHTTPServer 80```
 
 ```h2> python -m SimpleHTTPServer 80```
 
 ```h3> curl http://10.0.0.100:80```

 2. The controller selects new serving server between h1 and h2 according to bps for both port associated h1 and h2.
 ```
------------------------------ [Current_port : 0, Current_server : 1] ------------------------------
Server switch 1
server 00:00:00:00:00:01, port 4
Server switch 1
server 00:00:00:00:00:02, port 5
Insert barrier rule to 1
Port 4, delta 180
Port 5, delta 180
------------------------------ [Current_port : 0, Current_server : 2] ------------------------------
Server switch 1
server 00:00:00:00:00:01, port 4
Server switch 1
server 00:00:00:00:00:02, port 5
Insert barrier rule to 1
Port 4, delta 6271
Port 5, delta 849
```
