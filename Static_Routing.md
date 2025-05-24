# Static Routing and DHCP Setup Between Two Networks

## Objective
Configure static routes on two routers to enable communication between two separate networks and set up DHCP servers on both networks to automatically assign IP addresses to clients.

---

## Network Topology

- **Network A:** 192.168.1.0/24  
- **Router1 LAN interface:** GigabitEthernet0/1, IP: 192.168.1.1  
- **Network B:** 192.168.2.0/24  
- **Router2 LAN interface:** GigabitEthernet0/1, IP: 192.168.2.1  
- **Router1 to Router2 link:** GigabitEthernet0/0 on both routers, subnet 10.0.0.0/30  
  - Router1 Gig0/0: 10.0.0.1  
  - Router2 Gig0/0: 10.0.0.2

Network A (192.168.1.0/24) <---> Router1 (Gig0/1) <--Gig0/0--> Router2 (Gig0/0) <---> Network B (192.168.2.0/24) (Gig0/1)


## Configuration Steps

### Step 1: Configure Router1

enable
configure terminal

! Configure router-to-router interface
interface GigabitEthernet0/0
 ip address 10.0.0.1 255.255.255.252
 no shutdown

! Configure LAN interface
interface GigabitEthernet0/1
 ip address 192.168.1.1 255.255.255.0
 no shutdown

! Configure static route to Network B via router-to-router link
ip route 192.168.2.0 255.255.255.0 10.0.0.2

! DHCP configuration for Network A
ip dhcp pool NETPOOL-A
 network 192.168.1.0 255.255.255.0
 default-router 192.168.1.1
 dns-server 8.8.8.8

! Exclude router IP from DHCP pool
ip dhcp excluded-address 192.168.1.1

exit
write memory


### Step 2: Configure Router1

enable
configure terminal

! Configure router-to-router interface
interface GigabitEthernet0/0
 ip address 10.0.0.2 255.255.255.252
 no shutdown

! Configure LAN interface
interface GigabitEthernet0/1
 ip address 192.168.2.1 255.255.255.0
 no shutdown

! Configure static route to Network A via router-to-router link
ip route 192.168.1.0 255.255.255.0 10.0.0.1

! DHCP configuration for Network B
ip dhcp pool NETPOOL-B
 network 192.168.2.0 255.255.255.0
 default-router 192.168.2.1
 dns-server 8.8.4.4
 

! Exclude router IP from DHCP pool
ip dhcp excluded-address 192.168.2.1

exit
write memory



###AUTHOR
NANA KOFI ABROKWA

