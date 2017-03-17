---
layout: page
title: Internally Network Devices Enclave
category: pages
---

***ORNL Internal Use Only***

-----------


* [IND Enclave Overview](#ind-enclave-overview)
* [Authentication in IND](#authentication-in-ind)
    * [Windows](#windows)
    * [Linux](#linux)
    * [Other Systems](#other-systems)
    * [Applications](#applications)
* [IND Network Considerations](#ind-network-considerations)
	* [IP Addressing](#ip-addressing)
	* [Flows](#flows)
* [IND Zones](#ind-zones)
    * [Printers](#indprinters)
    * [VOIP](#voip)
    * [Devices](#inddevices)
    * [Type 4](#indtype4)
    * [Related zones in Other Enclaves](#related-zones-in-other-enclaves)


# IND Enclave Overview

Internally Networked Devices (IND) is primarily intended for devices which need to be on the ORNL network, but which cannot be fully managed by typical ORNL management tools.  Devices in IND do not have Internet access (see [Related Zones](#related-zones-in-other-enclaves) for a link to OpenResearch.Devices for a network location for unmanaged devices which need Internet access).  Also, devices in IND should generally not be able to initiate traffic to other ORNL network devices.  Outbound firewall exceptions from IND are Unusual, except for those essential for base operation and configuration (e.g. NTP, DNS, Radius, certificate revocation lists).  

The IND enclave is managed at M/L/L for confidentiality, integrity, and availability.  Moderate confidentiality is necessary so that, for example, printers may be used to print Sensitive (but not classified) information and phones may be used for sensitive (but not classified) discussions.  While the overall printing system is Moderate Availability, any given printer is Low Availability.  Likewise the phone system overall is Moderate Availability, but any given phone is Low Availability.  This is why the printers and phones themselves are in IND, but the servers which control the overall system will be in other enclaves (typically Servers) and the supporting network hardware is in Core Infrastructure.

With the exception of [IND.Type4](#indtype4), systems in IND do not require non-standard device exceptions to be on the ORNL network.  This enclave is specifically constructed to handle devices that are non-standard and cannot be in other enclaves.  However, the ability to register a device in IND is restricted.  The details of this restriction need to be worked out.  As a starting point, it should be restricted to those with a Networking role.  

# Authentication in IND

In general, IND does not make use of ORNL *CAMS systems.  In some cases, devices may be able to make use of LDAP for authentication or for attribute lookup.  Where that is the case, accounts used for interactive login in IND must not be used in any other enclave.  For example, a Linux-based system may be able to use LDAP for password authentication or for attribute lookup.

The key guiding principles are that accounts used within IND must be local to IND to minimize the risks that a compromised IND account can be used for lateral movement back to other ORNL enclaves.  Accounts should only be used on the IND systems needed and should only have the necessary permissions (least user privilege), in order to minimize risks of lateral movement across IND.

Password (Level 2) authentication is generally permitted within IND, though higher level forms of authentication, where practial, are encouraged.  In particular, MultiFactor Authentication (MFA) tools used elsewhere in ORNL **may** be used within IND.  Specifically, SecurID (via RADIUS) may be used, and certificates (including PIV, LSSO, and eToken) may be mapped to IND-local accounts.  

Level 4 MFA is required for administrative accounts on ITSD-managed systems in IND which provide basic infrastructure services to this enclave.  In some cases, the best option is to provide these services using systems in other enclaves.  In that case, the rules for the device enclave apply.  However, to avoid issues during network partition events, it is often desirable to have basic services (e.g. ntp, dhcp, dns, intrusion prevention, and intrusion detection) provided from within the enclave.  In such cases, administrative accounts used for interactive login to such systems are considered Privileged Accounts under the MFA 

Because there is a logical separation and no trust relationship to other ORNL enclaves, accounts with system administration rights on systems other than ITSD-managed infrastructure systems are not considered Privileged Users under the MFA requirements.  MFA is particularly encouraged, where practical, for administrative accounts, but it is not required.

As with all other ORNL environments, credentials should be assigned to a specific person.  Usernames and passwords should not be shared.  However, sharing is often unavoidable for some of the kinds of systems in IND, such as accounts used on laboratory equipment involved with long-running experiments.  Where shared accounts are necessary, these should be documented in the [Device Exception System](https://home.ornl.gov/~dex/prod) using an *Operational UserID Subscription* exception.

## Windows

Windows systems in IND will not be joined to any ORNL domain.  They may use local system authentication or workgroup authentication (which means accounts will generally be unmanaged).  IND is a valid location for obsolte versions of Windows (such as XP), particularly in the IND.Type4 zone. 

Where it is necessary for a Windows system to be used to provide core infrastructure services to IND, it is strongly preferred that such a system be located in another enclave (where it can be domain-joined) and firewall rules put in place to permit access to that system from the IND enclave.  Note that the print servers, which are often running Windows, should be located in the Servers enclave (to serve Workstations and Servers) or DMZ (for Visitor Wireless services) and then reach into IND to the IND.Printers zone.

Windows systems may be configured to use smart card authentication to local or workgroup accounts. PIV, LSSO, eToken, and other forms of ORNL certificate-based authentication are specifically permitted.  However, Windows natively only supports smart card authentiation for domain accounts.  It is untested at ORNL, but [EIDAuthenticate](https://www.mysmartlogon.com/eidauthenticate/) is a third party product which supports smart card authentication for local accounts and may be appropriate for IND applications where smart card authentication provides advantages. 

Where IND systems need credentialed access to systems in other enclaves, such as laboratory equipment in IND.Type4 accessing a file share in Servers (to move data out to where researchers can more easilya access it), service accounts used specifically for this purpose should be used.  In the above example, an ORNL staff member responsible for the laboratory would request a UCAMS project account.  That username and password (or a certificate bound to that UCAMS account) would be used as the credential for the laboratory computer to access the file share.  The project account may not be used on systems outside of IND, but may be used on multiple IND systems for which that staff member is responsible.  

## Linux

Linux systems may **not** use the workstation password server.  They may use direct LDAP binding for password authentication, but only if the accounts involved are not used in any other ORNL Enclave.  Linux systems may also use SecurID via RADIUS for authentication, potentially with LDAP attribute lookup.  While there is no lateral migration risk for a SecurID-authentication account in IND, the account must still only be used in the IND enclave, as a matter of documentation and clarity of account segmentation.

ssh-key authentication (including ssh keys derived from PIV, LSSO, eToken, and other ORNL credentials) to local accounts is the preferred form of authentication when using ssh within IND and when coming from other ORNL enclaves.  

## Other Systems

Other systems should follow the same pattern.  Accounts should be locally managed, though use of LDAP and Radius authentication is permitted for IND-specific accounts.  As a specific example, some printers are capable of using LDAP for authentication for their administrative credentials.  So long as the accounts used are IND-specific (e.g. not used in any other enclave), access to LDAPU for this type of authentication is permitted, particularly since managed accounts are preferred over unmanaged accounts.  

## Applications

Applications should not be hosted in IND.  However, there are cases where systems in IND may need to reach back into other ORNL zones for authentication.  For example, Prox card readers can be in an IND zone.  In this case, there are servers that push configurations to the card readers (pushing into IND is preferred over IND pulling from other zones).  There is interest in using badges as a means to relase jobs from printers.  In such cases, it may be necessary for the printer to reach back to a web service to determine if the badge is valid and retrieve the attributes to know what job(s) to release.  

# IND Network Considerations

Generally, devices in IND will be on wired networks, and wireless is an exception.  Note that the F&O barcode readers and the display kiosks are considered managed devices and can be in Workstations.  If there are IND devices that need campus-wide wireless access, the present design does not accommodate that need and this will need to be discussed with Networking and Cybersecurity.  Where wireless access is needed in a well-defined area, then it may be appropriate for Networking to set up a few dedicated WiFi access points with password protection using one or more wired ports assigned to an IND VLAN.  

## IP Addressing

IND should use RFC 1918 IPv4 address space and should be blocked at the NAT services from Internet access.

## Flows

In general, firewall exceptions into IND from Workstations and Servers are considered Normal (c.f. [Firewall Rule Classification](firewall-rule-classification)).  Firewall exceptions from other ORNL network zones are Unusual.  

Some inbound firewall exceptions for specific zones are birthright, as documented in the discussions for those zones.  

Interactive login (e.g. rdp and ssh) for IND.Type4 does not have to go through a jump server.  Interactive login for all other zones requires a jump server or a cybersecurity plan.

The following types of outbound firewall exceptions from IND are Normal: 

* ldaps access to LDAP servers (but this does not include access to Active Directory), 
* radius access to the RADIUS servers, 
* radius access to the securID servers, 
* SecurID protocol access to the securID servers, 
* http(s) access to the repoman repository, 
* git access (ssh or https) to ORNL gitlab servers,
* http(s) access to ORNL certificate revocation list (CRL) and Online Certificate Status Protocol (OCSP) severs
* access to basic network services (e.g. ntp, dhcp, dns) where it has been determined that providing those services from outside the enclave is best.

All other firewall exceptions out of IND to another ORNL network zone is Unusual.

Any firewall exception involving Internet access is a CISO Exception.

# IND Zones

## IND.Printers

IND.Printers hosts printers and similar devices.  To the extent possible, peer-to-peer traffic within this network should be blocked. As with the rest of IND, printers should be on hardwired connections using RFC 1918 addresses.  

Inbound firewall exceptions from Workstations are Unusual.  Workstations should use print servers (such as ORNLPRINT), rather than printing directly to a networked printer.  

Inbound firewall exceptions from print servers are Normal (including print servers in Servers, Open Research, and the DMZ (for Visitor Wireless)).  Inbound firewall exceptions from other types of servers are Unusual and require clear justification for why a print server presents problems for the particular application.   

## IND.VOIP

The specific characteristics and location of this zone are subject to the VOIP project design, and this zone should be considered as preliminary/proposed.  The intent is that the VOIP phones themselves should be in this zone, isolated from other devices and traffic.  

## IND.Devices

IND.Devices is intended for equipment which is 

1. required for a mission or mission support need, 
2. needs some form of access to or from the ORNL internal network, and
3. cannot be managed through core IT management tools (e.g. mobile device management, Active Directory/SCCM, or CFEngine).  

This equipment may have some form of management or configuration control specific to the class of the devices, and there may be subzones used to group or manage classes of devices.  For example networked Prox card readers are managed and are configuration controlled, but not with core IT systems.  These devices may be in IND.Devices itself or in a subzone.

IND.Devices is closely related to the IND.Type4 zone.  The key differences are:

1. IND.Devices is a more restrictive network environment, with no birthright access to or from other ORNL networks.  
1. Systems in IND.Type4 must have an approved exception in DES (either non-supported system or obsolete operating system).  They are often running operating systems which have been permitted at one point at ORNL, so there is at least some level of experience working with the OS and there may be some residual management tools available.  
1. In return for that formal exception approval, IND.Type4 systems have birthright access from selected ORNL zones and to selected ORNL services.
1. All firewall exceptions involving IND.Devices are at least Unusual, while there are some Normal firewall exceptions involving IND.Type4


## IND.Type4

The IND.Type4 zone is a very specific configuration of the broader concept of a Type 4 system -- one which is completely end-user managed and behind an ITSD-managed firewall.  The majority of systems in IND.Type4 are running an obsolete operating system, though completely non-standard operating systems can also be permitted.  

Where IND.Type4 systems are used for Moderate Confidentiality, some form of physical access control is required, such as ensuring that the equipment is in a locked room with need-to-know and audited physical access controls.  

As noted in [IND.Devices](#inddevices), systems in IND.Type4 must have an approved device exception.  In return for that formal exception approval process, systems in IND.Type4 are granted a baseline of network access to and from other ORNL internal network enclaves, noted below:

* Inbound rdp and ssh from the Workstations enclave (note that an IND.Type4 device owner must specifically enable RDP or ssh and grant specific accounts access)
* Linux package repository (repoman, anonymous access)
* ORNLAccess (Level 3 or better required)
* ORNLPrint (anonymous print is permitted)
* ORNLData (credentialed, preferably by certificate, but Level 2 is permitted) 
* Backup services. 

In addition to the Normal exceptions discussed in [Flows](#flows), the following exceptions for IND.Type4 are also Normal:

* Access to a Windows fileshare in the Servers or Open Research Enclaves (for purpose of data movement)
* Access to ORNL-internal git services (for purposes of data movement or configuration management)
* Access to a NFS mount in Servers or Open Research Enclaves (for purposes of data movement)
* ssh access to a server in Servers or Open Research if using ssh keys, for the purposes of data movement
* ssh access from a server in Servers or Open Research if using ssh keys, for the purposes of data movement, system administration, or configuration management (note that ssh from Workstations is allowed as birthright)
 

## Related Zones in Other Enclaves

In earlier versions of this documentation, the ILOM (integrated lights-out-management) zone was shown in IND.  However, this zone was moved to Servers, in part because some of these systems are able to use LDAP and UCAMS credentials.  In addition, IND is primarily a L,L,L zone.  Access to the lights out is Moderate for integrity and potentially moderate for confidentiality. Therefore, putting ILOM into Servers (which is M,M,M) is more appropriate for controls.  See [Servers.ILOM](netseg/servers-enclave#serversilom) for additional information.

For unmanaged devices, there are two network areas that are most relevant.  The key tradeoff is ORNL network access and Internet access.  Devices that need to ORNL network access or need to be accessed by other ORNL devices should be in an IND zone.  Devices that need to access the Internet need to be in [OR.Devices](netseg/open-research-enclave#ordevices), but they will not have ORNL internal network access.  Note that past practice was to put unmanaged devices needing Internet access onto Visitor Wireless, and there may be technical reasons where that's necessary, but OR.Devices is preferred.  