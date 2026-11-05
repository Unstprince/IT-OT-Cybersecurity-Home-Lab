# Lab 01 - Cisco Network Infrastructure

## Objective

Build and configure the initial network infrastructure for my IT/OT cybersecurity home lab using a physical Cisco Catalyst 3750X switch and Cisco 1900 Series router.

## Equipment

- Cisco Catalyst 3750X Switch
- Cisco 1900 Series Router
- Windows 11 Laptop
- Console cable
- Ethernet cables

## Tasks Completed

- Established console access to the switch and router
- Configured VLANs
- Created a dedicated management VLAN
- Configured access ports
- Configured an 802.1Q trunk
- Configured Router-on-a-Stick
- Configured SSH on the switch
- Configured SSH on the router
- Tested remote SSH connectivity
- Saved the configurations

## Configuration Steps

### Step 1 - Initial Device Access

Connected my laptop to the Cisco devices using a console connection and PuTTY.

### Step 2 -Ssh and Basic Device configuration 

…Commands used for switch and Router
Hostname R1/S1…

…enable algorithm-type sha256 secret #password#
service password-encryption…

…ip domain-name <Domain name>
crypto key generate rsa
ip ssh version 2…

…username <username> privilege 15 secret #password#…

…login block-for 120 attempts 3 within 60s

…login on-success log
login on-failure log…


…line vty 0 4
login local 
transport input ssh
exit…

…line console 0
password <password>
Login local
exit…


### Step 3 - VLAN Configuration

Created VLANs on the Cisco Catalyst 3750X switch to separate network traffic.

Commands used:

…vlan 10
 name IT
vlan 20
 name OT
vlan 30
 name MANAGEMENT 
vlan 99
 name NATIVE
exit …

### Step 4 - Management VLAN,SVI Configuration and Switchport Access Modes

Configured a dedicated management VLAN and assigned an IP address to the switch SVI for remote manageme.Also created
switchport access mode for the management vlan.

Commands used

…interface vlan 30
ip add <ip address and subnet mask>
no shut
exit
ip default gateway<router default gateway >…

interface g1/0/0
switch port mode access
switchport access vlan 30
## After this step we can establish a connection between the switch and the pc so as to create an ssh connection.

# Configuration of Trunk port and Router on a stick.

commands use 
interface g0/0/2
…switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan,
20,30,40
no shutdown…

# On Router…Router on a stick
…interface Go/0/0
no shutdown 
exit…

…interface G0/0/0.10
encapsulation dot1q 1
exit
ip address <default gateway of the interface>…

…interface g0/0/0.20
encapsulation dot1q 20
exit
ip address <default gateway of the interface>…

…interface G0/0/0.30
encapsulation dot1q 30
exit
ip address <default gateway of the interface>…

## step 5 Verify inter vlan connection 

commands used…
…show vlan brief
show interfaces trunk
show ip interface brief
ping <VLAN gateway>
ping <device in another VLAN>…

## Connectivity Verification:
…I verified that the VLANs 
and trunk were operational 
and tested communication 
between VLANs through the 
router-on-a-stick 
configuration…

## Step 6 
i established a remote connection for the router and switche
command used..

…ssh -oKexAlgorithms=+diffie-hellman-group1-sha1 <username>@<IP-address>…


