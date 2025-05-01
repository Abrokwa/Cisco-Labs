# Cisco-Labs


# 1) VLAN and Inter-VLAN Routing Lab (Router-on-a-Stick) - Cisco Packet Tracer

This repository documents a basic lab setup for VLAN configuration and inter-VLAN routing using a Cisco 2960 switch and a Cisco 2911 router in Cisco Packet Tracer.

## Lab Objective
To simulate a small office network with two departments (HR and IT) using VLANs, and to configure inter-VLAN communication using a single router interface (Router-on-a-Stick method).

## Topology Overview
```
        [Router: G0/0]
             |
             | (Trunk)
        [Switch 2960]
         /    |    \
     PC0   PC1   PC2   PC3

VLAN 10: PC0, PC1 (HR Department)
VLAN 20: PC2, PC3 (IT Department)
```

## Devices Used
- 1 x Cisco 2911 Router
- 1 x Cisco 2960 Switch
- 4 x PCs

## IP Address Plan
| Device           | IP Address     | VLAN  | Role                |
|------------------|----------------|-------|---------------------|
| PC0              | 192.168.10.2   | 10    | HR                  |
| PC1              | 192.168.10.3   | 10    | HR                  |
| PC2              | 192.168.20.2   | 20    | IT                  |
| PC3              | 192.168.20.3   | 20    | IT                  |
| Router G0/0.10   | 192.168.10.1   | 10    | Gateway for VLAN 10 |
| Router G0/0.20   | 192.168.20.1   | 20    | Gateway for VLAN 20 |

## Step-by-Step Configuration

### Step 1: Configure VLANs on the Switch
```bash
Switch> enable
Switch# configure terminal
Switch(config)# vlan 10
Switch(config-vlan)# name HR
Switch(config-vlan)# exit
Switch(config)# vlan 20
Switch(config-vlan)# name IT
Switch(config-vlan)# exit
```

### Step 2: Assign Ports to VLANs
```bash
Switch(config)# interface range fa0/2 - 3
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 10
Switch(config-if-range)# exit

Switch(config)# interface range fa0/4 - 5
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 20
Switch(config-if-range)# exit
```

### Step 3: Set Trunk Port (Switch Port Connected to Router)
```bash
Switch(config)# interface fa0/1
Switch(config-if)# switchport mode trunk
Switch(config-if)# exit
```

### Step 4: Configure Subinterfaces on Router (Router-on-a-Stick)
```bash
Router> enable
Router# configure terminal
Router(config)# interface gigabitEthernet 0/0.10
Router(config-subif)# encapsulation dot1Q 10
Router(config-subif)# ip address 192.168.10.1 255.255.255.0
Router(config-subif)# exit

Router(config)# interface gigabitEthernet 0/0.20
Router(config-subif)# encapsulation dot1Q 20
Router(config-subif)# ip address 192.168.20.1 255.255.255.0
Router(config-subif)# exit

Router(config)# interface gigabitEthernet 0/0
Router(config-if)# no shutdown
Router(config-if)# exit
```

### Step 5: Configure PCs with IP Addresses

#### For PC0 & PC1 (HR VLAN 10):
- IP: 192.168.10.2 / 192.168.10.3
- Subnet Mask: 255.255.255.0
- Default Gateway: 192.168.10.1

#### For PC2 & PC3 (IT VLAN 20):
- IP: 192.168.20.2 / 192.168.20.3
- Subnet Mask: 255.255.255.0
- Default Gateway: 192.168.20.1

### Step 6: Test Connectivity

Open the command prompt on any PC and run:
```bash
ping 192.168.10.3   # From PC0 to PC1
ping 192.168.20.2   # From PC0 to PC2
```

Both pings should work — showing that VLANs can now communicate through the router.

---

## Summary (Simple Explanation)
- VLANs are like **separate rooms** in a building.
- A **trunk port** carries traffic from all rooms (VLANs) to the router.
- The router uses **subinterfaces** (virtual mini-doors) to connect with each VLAN.
- This setup is called **Router-on-a-Stick**.

Each VLAN has its own IP range and gateway, but the router lets them **communicate safely and efficiently.**

---

## License
MIT

## Author
Your Name (replace with your GitHub username)
