# PAgP EtherChannel Configuration on Cisco Switches

## Overview
This repository contains configurations and documentation for setting up **Port Aggregation Protocol (PAgP) EtherChannel** on Cisco switches. The setup involves VLAN creation, trunking, and EtherChannel configuration using the **desirable** mode.

## Network Setup
### VLAN Configuration:
- **VLAN 10** → Name: `AA`
- **VLAN 20** → Name: `BB`
- Assign interfaces `Fa0/3-4` to VLAN 10 (Access Mode)
- Assign interfaces `Fa0/5-6` to VLAN 20 (Access Mode)

### EtherChannel Configuration:
- Use **PAgP (desirable mode)** for interfaces `Fa0/1-2`
- Configure **Port-Channel 1** as a trunk

## Configuration Steps
### **Step 1: VLAN Setup**
```bash
Switch(config)# vlan 10
Switch(config-vlan)# name AA
Switch(config-vlan)# exit

Switch(config)# vlan 20
Switch(config-vlan)# name BB
Switch(config-vlan)# exit
```

### **Step 2: Assign Ports to VLANs**
```bash
Switch(config)# interface range fa0/3-4
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 10
Switch(config-if-range)# exit

Switch(config)# interface range fa0/5-6
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 20
Switch(config-if-range)# exit
```

### **Step 3: Configure EtherChannel (PAgP)**
```bash
Switch(config)# interface range fa0/1-2
Switch(config-if-range)# channel-group 1 mode desirable
Switch(config-if-range)# switchport mode trunk
Switch(config-if-range)# exit
```

### **Step 4: Verify Configuration**
To check if EtherChannel is successfully created:
```bash
Switch# show etherchannel summary
```
Expected Output:
```
Group  Port-channel  Protocol  Ports
------+-------------+---------+-----------------------------------
1      Po1(SU)      PAgP      Fa0/1(P) Fa0/2(P)
```

### **Step 5: Save Configuration**
```bash
Switch# wr
Building configuration...
[OK]
```

## Troubleshooting
- If you see an error related to **DTP mode mismatch**, ensure both interfaces have matching trunk/access configurations.
- If **Port-channel is down**, verify both interfaces are active and in the correct mode.
- Use `show running-config` to check for misconfigurations.

## Files Included
- **config.txt** → Contains full switch configuration
- **README.md** → Documentation & setup guide

## Author
Kaushalya Jayasekara
