---
layout: page
title: Change Log
category: pages
---

***ORNL Internal Use Only***

-----------

* 2016-05-04: Draft of core CI Infrastructure enclave.  Minor tweaks to some other pages.  Stubbed out Workstation Enclave zones.  

* 2016-04-13: Rename DMZ.ExternalProxy to DMZ.Proxy (external is redundant in the context of the DMZ).

* 2016-04-13: Dropped Border as an enclave.  The border router itself will be in the IT Core Infrastructure enclave.  The network segments currently hanging off the border firewall (ARM and IPv6Test can either be standalone Enclaves (if needed) or a Zone or SubZone in another enclave).

* 2016-04-13: Renamed Network Segment to Network Environment.  

* 2014-04-07: Stubbed out for IT Core Infrastructure and Border enclaves, per discussion with Carly Logan.

* 2014-04-07: Stubbed out the definition for DMZ.Tenant in the [DMZ Enclave](netseg/dmz-enclave) descriptions.

* 2016-04-07: Added definition of Network Segment to the [NetSeg Home](netseg/home) page.  Also on that page changed Context to SubZone.  Some text in the Enclave, Zone, and SubZone segments were moved into the Network Segment section, as Enclaves, Zones, and SubZones are types of Network Segments.  Clarified that devices (particularly networking gear) may be registered in an Enclave (most devices, and all endpoints, will get registered in a Zone or a SubZone).  

* 2016-04-06: Fixed bug in DMZ flow diagrams, where some of the internal flows were erroneously labeled as "Internet ports". These should have said "non-shell Ports".  In the DMZ flow diagrams, the Internet is always to the right, internal systems to the left, and other DMZ systems are down.  The key point is that there are differences in flows for ports where shell access is permitted (e.g. RDP, PowerShell, and ssh) and ports where shell access is typically not involved (e.g. https, http, database access, and other application access protocols).

* 2016-04-06: Renamed Workstations.Moderate to Workstations.MFA, renamed Workstatations.Low to Workstations.Level2, changed CIA level for Workstations.Level2 to {M,L,L}.  We needed to allow for systems (like Macs) that can't directly MFA for interactive login but which may still hold moderate confidentiality information. Workstations.MEC and Workstations.MFA are perhaps too close in name, but I don't have a better idea yet.  As part of this, clarified that Workstations.MFA is L3+ (enhanced L3).  Also renamed the two Citrix farms to MFA Citrix and Level2 Citrix.

* 2016-04-06: Changed L4/3 to L4 for servers.  There are effectively no L3 exceptions for privileged users, so server enclave interactive logins will leed to be L4.  This affects all zones in Servers and DMZ.  It also affects OR.Servers

* 2016-04-01: Supercomputing enclave is {M,M,L} (Low for availability).  Updated netseg diagrams and text to reflect this.

