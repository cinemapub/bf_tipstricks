# DUAL HOMED QNAP NETWORK CONFIG

## Problem

* I use a dual-homed 4-bay [QNAP](http://www.qnap.com/) as file server and it syncs a number of its local folders with Amazon S3, using a custom version of [s3cmd](https://github.com/cinemapub/s3cmd) (modified by me to support dual homed machines)
* each network card connects to a separate WAN uplink, and to use some kind of load balancing, I bind half of the transfers to `eth0` and the other half to `eth1`. It lets me upload at double speed. Since we have DSL upload speeds of 200-300 KB/s, this matters.
* after a QNAP system upgrade, I could no longer bind the `s3cmd` client app to `eth1`, I always got "`([Errno 111] Connection refused)`". 
* Binding on `eth0` worked fine. I tested and I had the same problem with `wget --bind-address=<ip1>` and `curl --interface <ip1>`. So I deduced it was a generic network (routing) problem, not a bug in my client software. 
* I saw in the QNAP interface that my eth1 interface did not get its own gateway, even if I specified it in the network config page.
* Both LAN connections are completely separate, and the default gateway `gw0` is not visible from `eth1`. So I looked for a way to let any connection from `eth1` alwaysuse its own LAN gateway.


### Solution 

After some research, I ran the following command:

	route add default gw <gw1> eth1

Before:

	Kernel IP routing table
	Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
	(...)
	127.0.0.0       *               255.0.0.0       U     0      0        0 lo
	<ip0>           *               255.255.255.255 UH    0      0        0 eth0
	<ip1>           *               255.255.255.255 UH    0      0        0 eth1
	default         <gw0>           0.0.0.0         UG    1      0        0 eth0
	

After:

	Kernel IP routing table
	Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
	(...)
	127.0.0.0       *               255.0.0.0       U     0      0        0 lo
	<ip0>           *               255.255.255.255 UH    0      0        0 eth0
	<ip1>           *               255.255.255.255 UH    0      0        0 eth1
	default         <gw1>           0.0.0.0         UG    0      0        0 eth1
	default         <gw0>           0.0.0.0         UG    1      0        0 eth0


### Legend

- `ip0`	is something like 10.11.111.14
- `gw0`	is something like 10.11.114.255
- `ip1`	is something like 192.168.1.14
- `gw1`	is something like 192.168.1.1


### Tested on

* QNAP TS-419P / QNAP TS-419P+