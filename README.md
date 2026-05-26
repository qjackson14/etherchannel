<p align="center">




<h1>Video Demonstration</h1>

- ### [YouTube: Day 23 Etherchannel](https://www.youtube.com/watch?v=AfomCntS3Sg)

<h2>Environments and Technologies Used</h2>

- Jeremy's IT Lab Youtube Channel
- Cisco Packet Tracer
  

  
<h2>Operating Systems Used </h2>

- Cisco IOS


<h2>Step-by-Step</h2>
- 🟢 <b>Step 1: Configure LACP EtherChannel (ASW1 ↔ DSW1)</b>
On ASW1

Enter privileged mode:

enable

Enter global configuration:

conf t

Verify spanning tree:

show spanning-tree

Configure interfaces for EtherChannel:

interface range g0/1 - 2
channel-group 1 mode active

Configure trunk on port-channel:

interface po1
switchport mode trunk

Verify:

show etherchannel summary
show running-config
On DSW1

Enter configuration mode:

enable
conf t

Configure interfaces:

interface range g1/0/3 - 4
channel-group 1 mode active

Set trunk encapsulation and mode:

interface po1
switchport trunk encapsulation dot1q
switchport mode trunk

Verify:

show etherchannel summary
show interfaces trunk
- 🟢 <b>Step 2: Configure PAgP EtherChannel (ASW2 ↔ DSW2)</b>
On ASW2

Enter config mode:

enable
conf t

Configure interfaces:

interface range g0/1 - 2
channel-group 1 mode desirable

Configure trunk:

interface po1
switchport mode trunk
On DSW2

Configure interfaces:

interface range g1/0/3 - 4
channel-group 1 mode desirable

Configure trunk:

interface po1
switchport trunk encapsulation dot1q
switchport mode trunk

Verify:

show etherchannel summary
- 🟢 <b>Step 3: Configure Layer 3 EtherChannel (DSW1 ↔ DSW2)</b>
On DSW2

Convert interfaces to routed ports:

interface range g1/0/1 - 2
no switchport

Create EtherChannel:

channel-group 2 mode on

Assign IP:

interface po2
ip address 10.0.0.2 255.255.255.252
On DSW1

Configure interfaces:

interface range g1/0/1 - 2
no switchport
channel-group 2 mode on

Assign IP:

interface po2
ip address 10.0.0.1 255.255.255.252

Verify:

show etherchannel summary
ping 10.0.0.2
- 🟢 <b>Step 4: Configure Static Routing</b>
On DSW1

Enable routing:

ip routing

Add route to server network:

ip route 172.16.2.0 255.255.255.0 10.0.0.2

Verify:

show ip route
On DSW2

Enable routing:

ip routing

Add route to PC network:

ip route 172.16.1.0 255.255.255.0 10.0.0.1

Verify:

show ip route
Test Connectivity

From PC:

ping 172.16.2.1

✅ Expect initial timeouts (ARP), then success

- 🟢 <b>Step 5: Check Load Balancing Method</b>

On any switch:

show etherchannel load-balance

Default:

Source MAC address
- 🟢 <b>Step 6: Change Load Balancing Method</b>
On ALL switches (ASW1, ASW2, DSW1, DSW2)

Configure:

port-channel load-balance src-dst-ip

Verify:

show etherchannel load-balance
