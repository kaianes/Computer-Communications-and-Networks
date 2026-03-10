#  Lab 6 – Static Routing and VLAN Configuration


##  Overview

This lab focuses on:

* VLAN configuration
* Router-on-a-Stick (subinterfaces)
* Static routing between routers
* Inter-VLAN communication
* Communication between VLAN and non-VLAN networks

The objective is to ensure seamless communication between all PCs in the network.

---

# Exercise 1 – VLAN + Router-on-a-Stick + Static Routing

## Objective

Create a network topology with:

* 4 VLANs
* Subinterfaces on routers
* Static routes between R0 and R1
* Full connectivity between all VLANs

---

## Network Architecture

![Network Topology](./images/ex1-topology.png)

### VLANs and Networks

| VLAN    | Network         | Gateway      |
| ------- | --------------- | ------------ |
| VLAN 10 | 192.168.10.0/24 | 192.168.10.1 |
| VLAN 20 | 192.168.20.0/24 | 192.168.20.1 |
| VLAN 30 | 192.168.30.0/24 | 192.168.30.1 |
| VLAN 40 | 192.168.40.0/24 | 192.168.40.1 |

### Inter-Router Network

| Connection | Network         |
| ---------- | --------------- |
| R0 ↔ R1    | 192.168.70.0/24 |

---

## Step 1 – VLAN Configuration (Switch)

```bash
enable
conf t

vlan 10
name VLAN10

vlan 20
name VLAN20
```

Assign access ports:

```bash
interface fa0/1
switchport mode access
switchport access vlan 10

interface fa0/2
switchport mode access
switchport access vlan 20
```

Configure trunk port:

```bash
interface fa0/3
switchport mode trunk
switchport trunk allowed vlan 10,20
exit
```

---

## Step 2 – Router-on-a-Stick (R0)

```bash
enable
conf t

interface fa0/0
no shutdown
```

Create subinterfaces:

```bash
interface fa0/0.10
encapsulation dot1Q 10
ip address 192.168.10.1 255.255.255.0

interface fa0/0.20
encapsulation dot1Q 20
ip address 192.168.20.1 255.255.255.0
```

---

## Step 3 – Inter-Router Configuration

### On R0

```bash
interface fa0/1
ip address 192.168.70.1 255.255.255.0
no shutdown
```

### On R1

```bash
interface fa0/1
ip address 192.168.70.2 255.255.255.0
no shutdown
```

![Network router-example](./images/ex1-router-example.png)


---

## Step 4 – Configure R1 VLANs

```bash
interface fa0/0
no shutdown

interface fa0/0.30
encapsulation dot1Q 30
ip address 192.168.30.1 255.255.255.0

interface fa0/0.40
encapsulation dot1Q 40
ip address 192.168.40.1 255.255.255.0
```

---

## Step 5 – Static Routing

### On R0

```bash
ip route 192.168.30.0 255.255.255.0 192.168.70.2
ip route 192.168.40.0 255.255.255.0 192.168.70.2
```

### On R1

```bash
ip route 192.168.10.0 255.255.255.0 192.168.70.1
ip route 192.168.20.0 255.255.255.0 192.168.70.1
```

---

## Verification

Use:

```bash
show ip route
```

Test connectivity:

```bash
ping 192.168.30.2
```
![Network ping-example](./images/ex1-ping-result.png)

Analysing the photo: PC in VLAN 20 pinged PC in VLAN 30 successfully, confirming inter-VLAN and inter-router communication.

All PCs across VLANs should communicate successfully.

---

# Exercise 2 – Adding Non-VLAN Networks

## Objective

1. **VLAN segmentation**
| VLAN    | Network         |
| ------- | --------------- |
| VLAN 10 | 192.168.10.0/24 |
| VLAN 20 | 192.168.20.0/24 |
| VLAN 30 | 192.168.30.0/24 |
| VLAN 40 | 192.168.40.0/24 |

2. **Router-on-a-Stick**(subinterfaces)
Comunication between VLANs using subinterfaces on R0 and R1.
``PC VLAN10 → Router → VLAN20``

3. **Static routing between R0 and R1**
R0 ---- R1
Each router controls two VLANs and they communicate through a static route.

4. New networks outside VLANs
Normal LANs connected to R0 and R1 without VLAN segmentation.
| PC        | Rede            |
| --------- | --------------- |
| PC4 / PC5 | 192.168.50.0/24 |
| PC6 / PC7 | 192.168.60.0/24 |


Extend the topology by:

* Adding one Ethernet interface to R0 and R1 (Module WIC-1ENET)
* Connecting additional PCs outside VLANs
* Ensuring full communication between all networks

![Extended Network Topology](./images/ex2-topology.png)

---

## 🗺 New Networks

| Router | Network         | Gateway      |
| ------ | --------------- | ------------ |
| R0     | 192.168.50.0/24 | 192.168.50.1 |
| R1     | 192.168.60.0/24 | 192.168.60.1 |

---

## Configure R0

```bash
interface eth0/0
ip address 192.168.50.1 255.255.255.0
no shutdown
```

Add static route:

```bash
ip route 192.168.60.0 255.255.255.0 192.168.70.2
```

![Network extended-router-example](./images/ex2-router0-config.png)

---

## Configure R1

```bash
interface eth0/0
ip address 192.168.60.1 255.255.255.0
no shutdown
```

Add static route:

```bash
ip route 192.168.50.0 255.255.255.0 192.168.70.1
```

---

![Network extended-router-example](./images/ex2-ip-route-router0.png)


## Final Expected Result

All PCs must be able to communicate:

* VLAN ↔ VLAN
* VLAN ↔ Non-VLAN
* Non-VLAN ↔ Non-VLAN

![Network final-result](./images/ex2-ping-result.png)

---

# Key Concepts Learned

* VLAN segmentation
* Inter-VLAN routing
* 802.1Q encapsulation
* Static routing configuration
* Multi-network communication design

---

# Common Issues

* Forgetting `no shutdown`
* Incorrect gateway on PCs
* Trunk port not configured
* Missing static routes


## Summary

#### 1. Create VLANs on the switch

First, create the VLANs on the switch.

Example:

```
enable
configure terminal

vlan 10
vlan 20
vlan 30
vlan 40
```

These VLANs represent **different networks**.

---

#### 2. Assign switch ports to VLANs

Each PC port must belong to the correct VLAN.

Example:

PC in VLAN 10:

```
interface fa0/2
switchport mode access
switchport access vlan 10
```

PC in VLAN 20:

```
interface fa0/3
switchport mode access
switchport access vlan 20
```

---

#### 3. Configure the switch port connected to the router as a trunk

The port connected to the router must carry **multiple VLANs**.

```
interface fa0/1
switchport mode trunk
```

This allows VLAN traffic to go to the router.

---

#### 4. Configure router subinterfaces (Router-on-a-Stick)

The router uses **subinterfaces** to communicate with each VLAN.

Example:

```
interface fa0/1.10
encapsulation dot1Q 10
ip address 192.168.10.1 255.255.255.0
```

```
interface fa0/1.20
encapsulation dot1Q 20
ip address 192.168.20.1 255.255.255.0
```

These IP addresses are the **default gateways** for the VLANs.

---

#### 5. Configure networks that are not VLANs

For normal LAN networks, configure a router interface directly.

Example:

```
interface eth0/1/0
ip address 192.168.50.1 255.255.255.0
no shutdown
```

---

#### 6. Connect the routers

Each router has an interface connecting them.

Router0:

```
interface fa0/0
ip address 192.168.70.1 255.255.255.0
no shutdown
```

Router1:

```
interface fa0/0
ip address 192.168.70.2 255.255.255.0
no shutdown
```

---

#### 7. Configure static routes

Each router must know how to reach the networks on the other router.

Router0:

```
ip route 192.168.30.0 255.255.255.0 192.168.70.2
ip route 192.168.40.0 255.255.255.0 192.168.70.2
ip route 192.168.60.0 255.255.255.0 192.168.70.2
```

Router1:

```
ip route 192.168.10.0 255.255.255.0 192.168.70.1
ip route 192.168.20.0 255.255.255.0 192.168.70.1
ip route 192.168.50.0 255.255.255.0 192.168.70.1
```

---

#### 8. Configure the PCs

Example:

PC1:

```
IP: 192.168.10.2
Mask: 255.255.255.0
Gateway: 192.168.10.1
```

PC6:

```
IP: 192.168.60.2
Mask: 255.255.255.0
Gateway: 192.168.60.1
```

---

#### 9. Test the network

First test the gateway:

```
ping 192.168.10.1
```

Then test communication between networks:

```
ping 192.168.60.2
```

If it works, the routing is correct.

---

#### Main idea of the exercise

The goal of the lab is to understand:

1. **VLANs separate networks in a switch**
2. **Router-on-a-Stick allows the router to communicate with multiple VLANs**
3. **Routers need routing tables (static routes) to reach other networks**
