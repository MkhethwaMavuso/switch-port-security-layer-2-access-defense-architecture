# switch-port-security-layer-2-access-defense-architecture
Layer 2 Enterprise Access Security Network featuring a central Layer 3 distribution/DHCP core switch (SW3 / 3650-24PS), two departmental edge switches (SW1 for VLAN 10 and SW2 for VLAN 20), legitimate end-user workstations, and an unauthorized intrusion node (Laptop-PT Attacker).

🌐 Quick Overview & Network Specs
🟢 Architecture & Topology: Layer 2 Enterprise Access Security Network featuring a central Layer 3 distribution/DHCP core switch (SW3 / 3650-24PS), two departmental edge switches (SW1 for VLAN 10 and SW2 for VLAN 20), legitimate end-user workstations, and an unauthorized intrusion node (Laptop-PT Attacker).

🔵 Designated Node MAC Address Allocations:

PC1 (VLAN 10): MAC Address 0001.1111.1111 (Connected via SW1)

PC2 (VLAN 10): MAC Address 0002.2222.2222 (Connected via SW1)

PC3 (VLAN 20): MAC Address 0003.3333.3333 (Connected via SW2)

PC4 (VLAN 20): MAC Address 0004.4444.4444 (Connected via SW2)

Laptop-PT Attacker: MAC Address 0005.5555.5555 (Unauthorized rogue device attempting interface access on SW2)

🟣 Core Security Objectives: Enforce strict Layer 2 access control to mitigate MAC address flooding attacks, rogue DHCP server spoofing, unauthorized physical plug-ins, and unauthorized device mobility across campus access ports.

💡 Key Technical Insights & What This Lab Demonstrates
🔍 Port Security Enforcement: Proves full mastery of Cisco Catalyst switch port security configurations by limiting the maximum number of allowable MAC addresses per access port.

⚡ Violation Modes (Protect, Restrict, Shutdown): Demonstrates how switches handle incoming frames from unknown/unauthorized sources (such as Laptop-PT Attacker with MAC 0005.5555.5555):

Protect: Drops frames from unauthorized MAC addresses silently without generating notifications or incrementing security violation counters.

Restrict: Drops unauthorized frames, logs syslog messages, and increments port security violation counters.

Shutdown (Default): Disables the physical interface immediately (err-disabled), drops traffic, and logs an alert.

🛠️ Sticky Learning vs. Static Configuration: Explores how sticky port security automatically learns connected source MAC addresses and appends them to the running configuration, reducing manual administrative overhead.

🛡️ Layer 2 Defense-in-Depth: Highlights hardening techniques for access ports (disabling dynamic trunking protocol/DTP, setting ports explicitly to access mode, and implementing storm control).

🛠️ Essential Cisco CLI Configuration Snippets
Enforcing Port Security on Edge Access Ports (SW2)
Cisco CLI
interface FastEthernet0/1
 description Access Port for PC3
 switchport mode access
 switchport access vlan 20
 switchport port-security
 switchport port-security maximum 1
 switchport port-security violation shutdown
 switchport port-security mac-address sticky
!
interface FastEthernet0/2
 description Access Port for PC4 / Attacker Vulnerability Point
 switchport mode access
 switchport access vlan 20
 switchport port-security
 switchport port-security maximum 1
 switchport port-security violation restrict
 switchport port-security mac-address 0004.4444.4444
🧪 Verification & Audit Commands
Bash
# Verify port security status, violation mode, maximum addresses, and current sticky entries
show port-security interface fastEthernet 0/2

# Display summary of all secure ports across the switch
show port-security summary

# Audit interfaces currently placed in err-disabled state due to security violations
show interfaces status err-disabled
