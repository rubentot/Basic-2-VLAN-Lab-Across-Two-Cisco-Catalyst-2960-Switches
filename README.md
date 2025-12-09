# Basic 2-VLAN Lab Across Two Cisco Catalyst 2960 Switches

Real hardware lab • Cisco IOS • Proper trunking • Layer 2 isolation

![Topology](topology.png)

### Goal
- PC1 ↔ PC3 only (VLAN 10 – 10.0.0.0/24)  
- PC2 ↔ PC4 only (VLAN 20 – 10.0.4.0/24)  
- Complete Layer 2 separation between the two groups

### Equipment used
- 2 × Cisco Catalyst 2960-24TT (real metal, not simulation)  
- 4 × PCs with static IPs as shown

### Key concepts demonstrated
- Creating VLANs  
- Access ports vs trunk ports  
- 802.1Q trunking between switches  
- `spanning-tree portfast` on end devices  
- Restricting VLANs on trunk with `switchport trunk allowed vlan`

### Switch Configurations (copy-paste ready)

**SW1**
```
hostname SW1
vlan 10
 name PC1-PC3
vlan 20
 name PC2-PC4

interface FastEthernet0/1
 description TRUNK-TO-SW2
 switchport mode trunk
 switchport trunk allowed vlan 10,20

interface FastEthernet0/2
 description PC1
 switchport mode access
 switchport access vlan 10
 spanning-tree portfast

interface FastEthernet0/3
 description PC2
 switchport mode access
 switchport access vlan 20
 spanning-tree portfast
```
 
**SW2**
```
 hostname SW2
vlan 10
 name PC1-PC3
vlan 20
 name PC2-PC4

interface FastEthernet0/1
 description TRUNK-TO-SW1
 switchport mode trunk
 switchport trunk allowed vlan 10,20

interface FastEthernet0/2
 description PC3
 switchport mode access
 switchport access vlan 10
 spanning-tree portfast

interface FastEthernet0/3
 description PC4
 switchport mode access
 switchport access vlan 20
 spanning-tree portfast
 ```

PC1 → PC3 : Success
PC1 → PC2 : Timeout (blocked by VLAN)
PC1 → PC4 : Timeout (blocked by VLAN)
PC2 → PC4 : Success
PC3 → PC2 : Timeout (blocked by VLAN)
