# two-site-network-segmentation
Two-site enterprise-style network demonstrating user, server, and management segmentation with inter-site routing and access control.

# Project Overview

This project models a two-site enterprise-style network with separate user, server, and management VLANs at each location. The design focuses on secure segmentation, inter-site routing, and controlled administrative access. User VLANs are permitted to access server resources, while management VLANs are isolated from user endpoints using ACLs. Routing between sites is implemented using a combination of static routing and OSPF. DHCP and DNS services are provided locally at each site.

# Network Zones

 Location 1 
 * Users: VLAN10, VLAN20, VLAN30
 * Servers: VLAN15
 * Management: VLAN99

 Location 2 
 * Users: VLAN110, VLAN120, VLAN130
 * Servers: VLAN115
 * Management: VLAN199



# Expected Connectivity Matrix

 * Location 1 Users (10/20/30) → Location 1 Servers (15): Allowed
 * Location 1 Users (10/20/30) → Location 2 Servers (115): Allowed
 * Location 2 Users (110/120/130) → Location 2 Servers (115): Allowed
 * Location 2 Users (110/120/130) → Location 1 Servers (15): Allowed
 * Location 1 Management (99) → Infrastructure (routers/switch SVIs, SSH): Allowed (VTY restricted to Management)
 * Location 2 Management (199) → Infrastructure (routers/switch SVIs, SSH): Allowed (VTY restricted to Management)
 * Location 1 Management (99) ↔ Location 2 Management (199): Allowed
 * Location 1 Management (99) → Location 1 Users (10/20/30): Blocked (by user SVI ACLs)
 * Location 1 Management (99) → Location 2 Users (110/120/130): Blocked (by user SVI ACLs on Location 2)
 * Location 2 Management (199) → Location 2 Users (110/120/130): Blocked (by user SVI ACLs)
 * Location 2 Management (199) → Location 1 Users (10/20/30): Blocked (by user SVI ACLs on Location 1)
 * Users (any user VLAN) → Management PCs (99.x / 199.x): Blocked
    * Stateless ACL blocks the reply path from Management back to users, so pings/initiations fail as designed


# Validation

* Screenshots included in this repository demonstrate:
* Successful user-to-server connectivity within and across sites
* Successful management access to network infrastructure via SSH
* Enforced isolation between management and user VLANs
* Functional DHCP address assignment and DNS name resolution across sites
