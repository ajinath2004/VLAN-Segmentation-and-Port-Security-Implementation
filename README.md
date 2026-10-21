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

- ***config terminal***
  This command takes you into global configuration mode, which allows you to make changes to the switch settings.

- ***interface fastEthernet 0/1 (or int fa0/1)***
  This selects the specific port (fa0/1) on the switch where the PC is physically plugged in.

- ***switchport mode access***
  This sets the port mode to "Access," meaning this port is intended for connecting a single end-user device like a PC or Laptop.

- ***switchport access vlan 2***
  This assigns the selected port to VLAN 2, making it part of the Finance Department network.

- ***switchport port-security***
  This turns on the Port Security feature on this specific port.

- ***switchport port-security maximum 1 (or max 1)***
  This restricts the port to allow only 1 connected device at a time, preventing multiple devices or splitters from connecting.

- **switchport port-security violation shutdown**
  If an unauthorized device or attacker tries to plug into this port, the switch will instantly shut down the port (err-disable state) to protect the network.

- **switchport port-security mac-address sticky**
  This tells the switch to automatically learn the connected PC's MAC address and permanently attach it to the port's running configuration.

- need to follow these exact same steps for every other PC in the network. The only things that change are the port number in interface fastEthernet 0/? (depending on which switch port the   PC is plugged into, like fa0/2, fa0/3, etc.) and the VLAN ID (access vlan 2, vlan 3, etc.) corresponding to that PC's department.

### Verifying MAC Address Table and Port Security Binding
<img width="702" height="680" alt="Screenshot 2026-10-18 214615" src="https://github.com/user-attachments/assets/a44a8432-208b-40a1-863c-a873226c1270" />
- Switch# show mac-address-table

- To confirm that VLAN assignment and sticky port security were applied correctly, I ran the show mac-address-table command on Switch0 via the console terminal
- VLAN 1 Ports (Fa0/3, Fa0/4, Fa0/5): Show as DYNAMIC entries, meaning they dynamically learn connected MAC addresses without port locking.
- VLAN 2 Ports (Fa0/9, Fa0/10, Fa0/11, Fa0/12): Show as STATIC entries. This confirms that the sticky port security feature successfully bound each Finance PC's physical MAC address to its   specific switch port

- **The STATIC binding ensures that if any device with a different MAC address attempts to plug into ports Fa0/9 through Fa0/12, the port security policy will trigger and immediately shut    down the interface to prevent unauthorized network access.**

### Testing Port Security Violation & Port Recovery
  To verify that the Switch Port Security policy effectively blocks unauthorized devices, I simulated a rogue device attack and performed a port recovery procedure on Switch0
  <img width="1060" height="656" alt="Screenshot 2026-10-18 214844" src="https://github.com/user-attachments/assets/925d0f22-f14c-4936-a395-2324f8f6116c" />
- Simulating an Unauthorized Device Connection (Security Violation)Action: An unauthorized rogue pc (PC13) was connected to port FastEthernet 0/9, which was previously allocated to
  Finance PC PC9. 
- As soon as PC13 attempted to send traffic, Switch0 detected an unknown MAC address on port Fa0/9.   
- Result: The switch immediately triggered a Violation, placing port Fa0/9 into an err-disable (shutdown) state. As shown in the topology, both link lights turned Red, blocking all 
  network communication to and from

# Resolving the Security Violation & Recovering the Port
  To bring the port back online after removing the rogue device, follow these steps via the Console CLI:

- Step 1: Disconnect the Unauthorized Device
  Unplug PC13 and reconnect the authorized Finance PC (PC9) back to port Fa0/9
  <img width="732" height="657" alt="Screenshot 2026-10-18 214917" src="https://github.com/user-attachments/assets/e7b0b88b-c64d-4359-a39b-d919f077b02f" />

  Reset the Interface on Switch CLI
 ***Switch# configure terminal***
 ***Switch(config)# interface fastEthernet 0/9***
 ***Switch(config-if)# shutdown***
 ***Switch(config-if)# no shutdown***

 ### Successful Port Recovery and Network Restoration
 <img width="752" height="670" alt="Screenshot 2026-10-18 215056" src="https://github.com/user-attachments/assets/de680848-cadd-4fff-9536-0df2f640b7f1" />
 - Authorized communication for PC9 was fully restored, confirming that sticky MAC port security safely recovers and operates as intended once rogue devices are disconnected.
