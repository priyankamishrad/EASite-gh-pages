---
layout: page
title: Open Questions for Network and Credential Segmentation
category: pages
---

***ORNL Internal Use Only***

-----------

**Summary**: This page describes issues and questions that have not yet been resolved in the design for the network and credential segmentation. Issues that have been addressed can be found on the [Questions and Answers](questions-and-answers) and [System Examples](system-examples) pages.  

# Topics
* &&&&& Table of contents to be generated

* How is dev.ornl.gov handled, particularly since there's one AD per zone?

* How should FUSEnet be handled?  This is an example of a moderate collaboration where external parties need OS level access to a set of virtual machines.  What zone does this belong in, since it is a) moderate confidentiality, b) externally accessable, and c) has users who are not ORNL staff.  And giving UCAMS accounts to all of those collaborators is not acceptable.   

* How should AWS authentication be handled, particularly in the context of CADES?  Is this a priviledged userID, and so should use DCAMS?  For the moderate-term future (years), some users will need to log into the AWS console in order to be able make settings changes that are not managed by cloudforms or other user-facing tools.  

* How should Moderate CADES overall be handled?  Again, users will include external collaborators who will need OS authentication.  

* Clearly document the differences between DMZ.ExtServicesLow vs Mod.  What are the access patterns for data in the Data & Relay, since there is only one of those (accreditied for Moderate).

* Clearly document the differences between External Proxy, Data & Relay, and Internal Services for the DMZ.

* Clarify diagram to distinguish between app authentication vs OS authentication.

* From the perspective of a host-based firewall for Windows 10 policy, is there a network range that can be used to identify the systems that need to reach back to an internal Windows workstation. For example, would it be possible to restrict inbound connections to Servers.Monitoring?  Note that this will break RDP back to workstations from (for example) conference room PCs and ORNLAccess.

Clarify how the credentials get segmented within the databases.  Example of this is the proposed new SharePoint POC.  

Who are the owners for the enclaves and for the zones.

IP Address range (how many devices)
Where DNS
?DHCP
How remediation handled
Active Directory Domain
Compliance services sources
Where physically located

Authentication
Owner   

Servers.MEC
Servers.DOE

MEC requires an additional firewall.  How is this handled for workstations.MEC (if this exists)?  


Mark Diodati -- Gartner Analyst for Linux MFA.  

Few orgs using Linux with Smart cards.  Elbow grease to get it to work.  Typically with common system.  Some Orgs use AD bridging products (centrify or beyond trust).  Use AD and fixes Kerberos to make it compatible with Windows.  Can lose SSO with some of the Kerberos implementations.

Rarely see public/private key across bastion host.  Use Kerb for SSH.  AD protocols are often UDP and multiple ports.  No data that this is working from a private cloud infrastructure.  OTP dropping, migration to mobile push.  Recommendation of Duo.    

* Per Vicki Wheeler and Eddie Bishop: Concept of a sandbox enclave:  "Could be used for special projects, development, prototyping, etc. Then once the project is determined to be viable and you know where it goes, it is another instance of an existing enclave.  But you could have multiple sandboxes as well. Just establish a sandbox enclave."

* Separate enclaves for HDSI and CJIS -- both are enhanced controls over a moderate baseline.  


What OU will a server get dropped into based on Server Enclave?  Same for DMZ?  How do the OU's map to the zones?  Who has permissions to put things into those OUs.  This is a barrier for buildouts.  

Document the remediation zones and processes

Who sets up the groups for server administration in the new enclave structure and how are those managed?

credentials (ssh keys) for Tenable and Mid Servers are currently the same for all zones.  How do we want to address this?

Need to handle both the zones and authentication for endpoint virtual machines.  

How do we handle the CADES VM self service admins?  We will have end-users who will be able to self-administer virtual machines inside of CADES.  How will they authenticate and how do we cover Level 4 for these users?  
