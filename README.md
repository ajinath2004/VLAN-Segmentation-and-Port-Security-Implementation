# VLAN-Segmentation-and-Port-Security-Implementation
Implementation of multi-department VLAN segmentation (Technical, Admin, Finance) with Switch Port Security to protect sensitive department ports in Cisco Packet Tracer.




### Network Topology

<img width="808" height="667" alt="Screenshot 2026-10-18 202425" src="https://github.com/user-attachments/assets/e52d71d2-7d17-4487-9031-ac4edbbb86e3" />

# Central Networking Device
- Switch  A Cisco 2960-24TT Layer-2 switch serves as the central interconnection point for the entire network. All departmental PCs and the management console are connected to this switch    via wired Ethernet cables.

# Console / Management Setup

- Console PC (PC12): Located at the top, PC12 is a dedicated Management/Admin PC. It is connected to the switch using an RS-232 Console cable (light blue line), enabling out-of-band            administration to configure VLANs, Switch Port Security, and manage network settings via the Command Line Interface (CLI).


# Departmental Segmentation

- Technical Department (Left Zone):

  Devices: PC0, PC1, PC2, PC3.

  Role: Workstations allocated for technical personnel, grouped into a dedicated VLAN for isolated broadcast management.

- Admin Department (Bottom Zone):

  Devices: PC4, PC5, PC6, PC7.

  Role: Endpoints designated for administrative operations and general office workflows.

- Finance Department (Right Zone):

  Devices: PC8, PC9, PC10, PC11.

  Role: High-security domain handling sensitive financial data, where Switch Port Security will be actively enforced to prevent unauthorized rogue device connections.

### Phase 1: VLAN Creation & Port Security Implementation

- To isolate the Finance Department from other network traffic and protect its switch ports from unauthorized access, the following configuration steps were executed on Switch0 via the Console interface (PC12).
- Creating VLAN 2 for the Finance Department
- First, a dedicated Virtual LAN (VLAN 2) was created and named FINANCE to form a isolated broadcast domain.
# create vlan 2
Switch> enable
Switch# configure terminal
Switch(config)# vlan 2


### Step-by-Step CLI Configuration & Command Breakdown
Here is the exact step-by-step CLI execution shown in the terminal for configuring individual interface security and VLAN assignmen

<img width="692" height="668" alt="Screenshot 2026-10-18 203740" src="https://github.com/user-attachments/assets/512de7e7-c0cf-4ee5-9fe5-f747108110b8" />

- Switch# config terminal
- Switch(config)# interface fastEthernet 0/9
- Switch(config-if)# switchport mode access
- Switch(config-if)# switchport access vlan 2
- Switch(config-if)# switchport port-security
- Switch(config-if)# switchport port-security maximum 1
- Switch(config-if)# switchport port-security violation shutdown
- Switch(config-if)# switchport port-security mac-address sticky

# Command Breakdown & Explanation

- **config terminal**
This command takes you into global configuration mode, which allows you to make changes to the switch settings.

- **interface fastEthernet 0/1 (or int fa0/1)**
This selects the specific port (fa0/1) on the switch where the PC is physically plugged in.

- **switchport mode access**
This sets the port mode to "Access," meaning this port is intended for connecting a single end-user device like a PC or Laptop.

- **switchport access vlan 2**
This assigns the selected port to VLAN 2, making it part of the Finance Department network.

- **switchport port-security**
This turns on the Port Security feature on this specific port.

- **switchport port-security maximum 1 (or max 1)**
This restricts the port to allow only 1 connected device at a time, preventing multiple devices or splitters from connecting.

- **switchport port-security violation shutdown**
If an unauthorized device or attacker tries to plug into this port, the switch will instantly shut down the port (err-disable state) to protect the network.

- **switchport port-security mac-address sticky**
This tells the switch to automatically learn the connected PC's MAC address and permanently attach it to the port's running configuration.
