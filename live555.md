# Everything about live555

[2016-05-19 - 10:47:15]
In linux, order of link library for live555 is -lliveMedia -lgroupsock -lBasicUsageEnvironment -lUsageEnvironment

[2016-07-26 - 11:02:16]
In redhat liked linux, if you face problem "Unable to determine our source address: This computer has an invalid IP address: 0.0.0.0"
it is caused of firewall or iptables disable multicast. So to fix this error, disable firewall or enable multicast in iptables, or edit
loopback IP in /etc/hosts