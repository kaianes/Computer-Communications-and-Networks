# Network topologies

#### a) star
All devices conect through a central device (switch). Easy to manage, fault tolerance (cabes) but still depends on the center device.

#### b) bus
All devices share a commun cabe (backbone). If the cabe fails, the whole network is down.

#### c) ring
Each device connects to the next making a circle. Data flows in only one way. Low scalability. 

#### d) mesh
Each device connects to every other. Very reliable, high fault tolerance but less scalable.

| Topology | Scalability                                            | Fault Tolerance                                                                    | Simplicity                          | Cost                                              | Reliability                                   |
| -------- | ------------------------------------------------------ | ---------------------------------------------------------------------------------- | ----------------------------------- | ------------------------------------------------- | --------------------------------------------- |
| **Bus**  | Low – performance drops as more devices are added      | Very Low – failure of main cable brings down entire network                        | High – very easy to set up          | Low – minimal cabling                             | Low – high chance of collisions and failures  |
| **Star** | Medium to High – easy to add new devices               | Medium – failure of one node doesn’t affect others, but central device is critical | High – simple to install and manage | Medium – requires more cabling and central device | High – stable and widely used                 |
| **Ring** | Low to Medium – adding/removing nodes disrupts network | Low – failure of one node can affect entire network                                | Medium – more complex than bus/star | Medium – requires structured connections          | Medium – no collisions, but fragile structure |
| **Mesh** | High – supports growth well (especially partial mesh)  | Very High – multiple paths ensure redundancy                                       | Low – complex to design and manage  | High – lots of connections and infrastructure     | Very High – extremely robust and resilient    |

# How to measure a network

### - Amount of data;

**Data** - bit (b): 0 or 1 or byte (B): 8 bits
- KB (kilobyte) = 1.000 bytes
- MB (megabyte) = 1.000.000 bytes
- GB (gigabyte) = 1.000.000.000 bytes

ex: 4MB - 4 milion bytes

### - Speed of data transmition;
k/M/G bps - kilobits/megabits/gigabits per second
k/M/G Bps - kilobytes/megabytes/gigabites per second

ex: 100 kbps - 100.000 bits per second

## throughput 

> Rate at witch data is successfully delivered? the final transmition's velocity is limited by the slowliest link of the path. (bottleneck)

## Circuit switching 

A way of communication between hosts. In the circuit switching the network creates a path that is reserved and dedicated end-to-end before the data is sent.

Even when not in use, that path is reserved.

The resource allocation can be done by Frequency Division or Time Division.

## Packet Switching

Another way of communication between hosts. Data is slipt into packets, each packets travels independently through the network. No reserved path. Each host only uses the network while transmiting.

The packet switching way only needs to have the bandwidth available. If the bandwidth required doesn't match the amount available, the packets will queue up.

#  OSI Model 

### Layer 2 - Data Link

#### Parity Checking
**even parity** - the number of 1s in the data should be even. If it's odd, it means that there was an error during the transmition.
**odd parity** - the number of 1s in the data should be odd. If it's even, it means that there was an error during the transmition.

Ex: two-dimensional parity scheme

[Lines]
1110 = 3 ones - odd parity - add 1 to make it even: 11101
0110 = 2 ones - even parity - add 0 to make it even: 01100
1001 = 2 ones - even parity - add 0 to make it even: 10010
1101 = 3 ones - odd parity - add 1 to make it even: 11011

-- Lines with parity bits: 1 0 0 1

[Columns]
1110
0110
1001
1101

1. First column: 3 ones - odd parity - add 1 to make it even: 10111
2. Second column: 3 ones - odd parity - add 1 to make it even: 11011
3. Third column: 2 ones - even parity - add 0 to make it even: 10110
4. Fourth column: 2 ones - even parity - add 0 to make it even: 00110

-- Columns with parity bits: 1 1 0 0

### Layer 3 - Network

- **Logical addressing** (IP addresses)
- **Routing** (choosing the path across networks)
- **Forwarding** (moving packets between routers)
- **Fragmentation** (IPv4)

#### Data Plane vs Control Plane
Data plane is responsable to *move* the data, local to each router and using a fowarding table (pre-setted rules). Control plane decides *how* the data has to be sent by calculating routes, updating the fowarding table and using protocols.

### About the IP 

INTERNET PROTOCOL HAS FOUR PARTS
EX: 202.3.14.25 (each number before "." has 1 byte / 8 bits)

Converting decimals do binary: You transform the number into a sum of powers of 2.
Always use: 128 64 32 16 8 4 2 1

Get the biggest number that is smaller than the number you want to convert, then subtract it from the original number and repeat the process with the remainder until you get to 0. The binary representation is formed by placing a 1 in the positions corresponding to the powers of 2 used and 0 in the others.

ex: Convert 202 to binary:
- 202 - 128 = 74 (1 in the 128 position)
- 74 - 64 = 10 (1 in the 64 position)
- 10 - 32 = -22 (0 in the 32 position)
- 10 - 16 = -6 (0 in the 16 position)
- 10 - 8 = 2 (1 in the 8 position)
- 2 - 4 = -2 (0 in the 4 position)
- 2 - 2 = 0 (1 in the 2 position)

So, 202 in binary is: 11001010

## Subnetting

Making a 4 subnet of the network 10.2.22.0/23.

IPs from 10.2.22.0 to 10.2.23.255

subnet 1: 200 hosts
subnet 2: 61 hosts
subnet 3: 55 hosts
subnet 4: 41 hosts

1. See how many IPs are in the original block.
2. See how many hosts each department needs. (Remember that the number of hosts is 2^n - 2, where n is the number of bits available for hosts. The "-2" accounts for the network and broadcast addresses, which cannot be assigned to hosts.)
3. Choose, for each department, the smallest subnet that can accommodate that number of hosts.
4. Distribute from largest to smallest.

Every IP has 32 bits and from the "/23", its possible to know that there is 9 bits available for hosts (32 - 23 = 9). So, the original block has 2^9 = 512 IPs.

1. The original block has 512 IPs.

- network: lowest number
- broadcast: highest number
- hosts: all the numbers in between

**subnet mask:** 11111111.11111111.1111111{0.00000000}

**ip address:** 00001010.00000010.0001011{0.00000000}

the last 9 bits are free!

**highest number:** 00001010.00000010.0001011{1.11111111} = 10.2.23.255

Its possible to know that /23 has 510 avaiable IPs. (512 total - 1 network - 1 broadcast).

3. The number of hosts needed for each subnet is:
a) subnet 1: 200 hosts - needs 256 IPs (2^8 = 256) - subnet mask: /24 
b) subnet 2: 61 hosts - needs 64 IPs (2^6 = 64) - subnet mask: /26
c) subnet 3: 55 hosts - needs 64 IPs (2^6 = 64) - subnet mask: /26
d) subnet 4: 41 hosts - needs 64 IPs (2^6 = 64) - subnet mask: /26

Now, its necessary to verify if the original block has enough IPs to accommodate all the subnets. The total number of IPs needed is 256 + 64 + 64 + 64 = 448, which is less than the 510 available in the original block, so it's possible to create these subnets.

4. Distributing from largest to smallest: forget the binary in this step.
- subnet 1:
  - network: 10.2.22.0
  - broadcast: 10.2.22.255
- subnet 2:
  - network: 10.2.23.0
  - broadcast: 10.2.23.63
- subnet 3:
  - network: 10.2.23.64
  - broadcast: 10.2.23.127
- subnet 4:
  - network: 10.2.23.128
  - broadcast: 10.2.23.191


### Layer 4 - Transport

Communication protocols:
- TCP (Browser - HTTP): reliable and controlled. Guarantees data's delivery, correct packet's order, reesends lost packets, controols traffic congestion.

- UDP (Streaming): Does not guarantees deliver, order not retransmition. But is really quick and low latency.

# Wireless 

A wireless channel is the medium used to transmit data through the air using electromagnetic waves. Instead of cables (like Ethernet), devices communicate over the air.

1. A device (phone, laptop) sends data
2. The data is converted into radio waves
3. These waves travel through the air
4. Another device receives and decodes them

#### Wireless impairments

**a) Path loss**
Reduction in signal power over distance. Signal looses its path of delivery. Signal gets weaker over distance.

**b) Multipath propagation**
A signal reaches the receiver through multiple paths due to reflection, diffraction, and scattering. Signals bounce off objects and arrive at different times.

**c) Interference**
Caused by external signals from other devices or networks using the same or nearby frequencies. Signals can overlap or be disrupted - ex: many WIFI networks in one place.


