# Day 02 — Network Hardening and Secure Management

## Objective

Harden the Cisco switch and router, secure management access, and troubleshoot management connectivity.

## Lab Environment

- Cisco Catalyst 3750X switch
- Cisco 1900 router
- Management VLAN: VLAN 30
  

## 1. Port Security

Configured port security on the management access port.

```
interface gi1/0/1
switchport mode access
switchport access vlan 30
switchport port-security
switchport port-security maximum 1
switchport port-security mac-address sticky
switchport port-security violation restrict
```

Verification:

```
show port-security interface gi1/0/1
show port-security address
```

## 2. PortFast and BPDU Guard

Configured Layer 2 protection on the management access port.

```
interface gi1/0/1
spanning-tree portfast
spanning-tree bpduguard enable
```

Verification:

```
show spanning-tree interface gi1/0/1 detail
show running-config interface gi1/0/1
```

## 3. Unused Port Hardening

Unused switch interfaces were administratively disabled.

Example:

```
interface range gi1/0/4 - 24
shutdown
```

This reduces the number of active physical access points into the network.

## 4. SSH Management

SSH was used for encrypted remote administration of both the switch and router.

Example VTY configuration:

```
line vty 0 15
login local
transport input ssh
```

Verification:

```
show ip ssh
show running-config | section line vty
```

## 5. SSH Management ACL

A named standard ACL was used to restrict SSH management access to the administration workstation.

```
ip access-list standard SSH_CONNECTION
10 permit host #ip address                 
20 deny any
```

Applied to the VTY lines:

```
line vty 0 15
access-class SSH_CONNECTION in
```

Verification:

```
show access-lists SSH_CONNECTION
show running-config | include access-class
```

## 6. Router SSH Restriction

The router was also configured to allow SSH management only from the administration workstation.

```cisco
ip access-list standard SSH_CONNECTION
10 permit host #ip address 
20 deny any
```

Applied to the router VTY lines:

```cisco
line vty 0 4
access-class SSH_CONNECTION in
transport input ssh
```

## 7. Troubleshooting

During the lab, connectivity to the switch management IP stopped working after interface changes.

Commands used during troubleshooting includes:       

```
show ip interface brief
show vlan brief
show interfaces status
show interfaces gi1/0/1 switchport
show mac address-table
show mac address-table interface gi1/0/1
show arp
show port-security interface gi1/0/1
show interfaces gi1/0/1 counters
show ip ssh
show running-config interface gi1/0/1
show running-config | section line vty
```

Windows troubleshooting commands included:

```
ipconfig
route print
arp -a
ping 192.168.30.2
tracert 192.168.30.2
getmac /v
```

The switch showed that the physical Ethernet link was active, but no incoming frames were initially being received from the laptop.

After restarting the Windows laptop, Ethernet communication was restored and ping/SSH connectivity returned.

## Key Lessons Learned

- An interface showing `up/up` does not guarantee traffic is actually flowing.
- MAC address tables help confirm whether a switch is receiving Layer 2 traffic.
- ARP verifies IP-to-MAC resolution.
- Interface counters help determine whether packets are entering or leaving a port.
- Port security limits which devices can use an access port.
- BPDU Guard protects endpoint ports from unexpected switching devices.
- SSH encrypts management traffic.
- VTY ACLs restrict which source IP addresses are allowed to initiate management sessions.
- Troubleshooting is most effective when moving systematically through Layers 1, 2, and 3.

## Next Steps

- Windows Server DHCP
- DHCP Snooping
- Dynamic ARP Inspection
- IP Source Guard                            
