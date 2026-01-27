# Lab 01 - Introduction to Cisco Packet Tracer

## Task 01 & Task 02

### Switch



## Task 03
![Task 03](./images/Task3.png)
- Try pinging one PC from another or sending a message between two PCs. What do you observe?

- Can all the PCs communicate with each other successfully? If not, can you identify the possible reasons for the communication failure?

- If you have identified the reason(s), address the issues to ensure all PCs can successfully communicate with each other.


Even though the PCs are connected to the same switch, and they all have IP addresses, they cannot communicate with each other because they are in different networks.

![Task 03- ping error](./images/Task3-error-ping.png)

Before:
- PC0: IP: 192.168.1.1
- PC1: IP:  192.168.2.3
- PC2: IP: 192.168.1.3
- PC3: IP: 192.168.2.1

After changing the IP addresses to be in the same network:
- PC0: IP: 192.168.1.1
- PC1: IP: 192.168.1.2
- PC2: IP: 192.168.1.3
- PC3: IP: 192.168.1.4

![Task 03- ping error](./images/Task3-ping-success.png)

## Task 04
 
 ### Router