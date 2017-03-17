---
layout: page
title: Industrial Control Systems Enclave
category: pages
---

***ORNL Internal Use Only***

-----------

* [ICS Enclave Overview](#ics-enclave-overview)
* [Authentication in ICS](#authentication-in-ind)
    * [Windows](#windows)
    * [Linux](#linux)
    * [Other Systems](#other-systems)
    * [Applications](#applications)
* [ICS Network Considerations](#ics-network-considerations)
* [ICS Zones](#ics-zones)
	* [ICS.SNO](#icssno)
	* [ICS.SNO.Citrix](#icssnocitrix)

# ICS Enclave Overview

The Industrial Control System (ICS) enclave houses systems and devices used to control physical equipment and processes at ORNL.  The Confidentialy of data in this enclave is low, and the focus is on protecting system Integrity and Availability.

At present, the only zone in ICS is SNO, which is used for control and management of &&&&&.  

# Authenication in ICS

The SNO zone has its own credential and management system (SNOCAMS), along with its own Active Directory infrastructure.  If and when additional zones are added to ICS, a decision will be needed whether to expand SNO to encompass those other zones or keep them separate.  

Standard User accounts within ICS are excluded from scope for Multifactor Authentication (MFA) requirements, per the DOE MFA implementation plan, as are service accounts.  MFA should be used where practical, but it is not required for standard user accounts.  Standard user accounts may have administrative privileges on one or more systems within ICS, consistent with the practice within Workstations that local administrative access by a system owner does not inherently make that account a Privileged User within the definitions of the DOE MFA implementation plan.  

However, MFA should be considered and used where practical, even for standard user accounts, as a matter of best practice.  ORNL Level 3 and Level 4 MFA credentials (e.g. PIV, LSSO, eToken, and SecurID) may be bound to ICS enclave credentials and used as cryptographic or One Time Password token generators.  System owners within ICS may choose more restrictive controls and require token generators that are restricted to the ICS enclave, one or more zones within ICS, or even specific systems within ICS.  

Accounts which have administrative rights within any ICS Active Directory domain or on enabling infrastructure (such as DHCP, DNS, time, Citrix, or LDAP services) are considered Privileged Accounts and Level 4 authentication is required for interactive login.  This may be achieved through use of LOA 4 jump servers to control remote network access and physical access controls for local (console) access.  


## Windows

Windows systems in ICS must be joined to the SNO domain and make use of SNO domain credentials.  

## Linux

Linux systems in ICS should use credentials managed via SNOCAMS.  At present, there is no OpenLDAP server for ICS (e.g no LDAPICS or LDAPSNO).  Linux systems may use LDAP bindings to the SNO Active Directory for attribute lookup.

## Other Systems

The ICS Enclave will include devices which cannot use ORNL credential management and configuration management tools but which are integral parts of industrial control systems.  Where practical, such devices should make use of isolated networks within ICS.  The use of local credentials should be documented in appropriate cybersecurity and business continuity plans.  

## Applications

Applications hosted in ICS are only for use within this enclave.  Application authentication may make use of existing ORNL MFA tools, Level 2 credentials managed by SNOCAMS, and local credentials (as documented in cybersecurity and business continuity plans).

# ICS Network Considerations

Systems in ICS should use RFC 1918 addresses.  

All firewall exceptions into and out of ICS are at least Unusual.  Any firewall exception involving Internet access and ICS is a CISO Exception.

Given the focus on system availability and integrity for ICS, strong preference should be given to providing core cyberinfrastructure services from within the enclave. 

Wireless networking is not used within SNO.  &&&&& Is this true?

# ICS Zones

## IND.SNO

ICS.SNO hosts all of the devices which are part of the overall SNO system.  

### IND.SNO.Citrix