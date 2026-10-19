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
