---
layout: page
title: Network and Credential Segmentation Architecture
category: pages
---

***ORNL Internal Use Only***

-----------

**Summary**: This wiki provides architecture information on the overall network and credential segmentation architecture for ORNL.  These two concepts are closely tied and must (generally) be considered together.  

**Purpose**: The primary objective of network and credential segmentation is to provide layers of defense against lateral movement on ORNL's network by an adversary and improve our ability to detect attempts at such movement.  It also provides tools to improve operational intelligence around the proper functioning of ORNL's IT systems.  Having a clear understanding and data on what is normal behavior increases our ability to detect anomalies, whether those are caused by adversarial actions, system malfunction, or human error.  

See the [Repository File List](https://code-int.ornl.gov/ea/architecture/tree/master/netseg) for the Visio files and PDF versions of the diagrams.

# Topics 
* See [Open Questions](open-questions.html) for a list of issues yet to be resolved.
* [Definitions and Key Concepts](#definitions-and-key-concepts)
    * [Network Environment](#network-environment)
    * [Enclave](#enclave)
    * [Zone](#zone) 
    * [SubZone](#subzone)
    * [Credential](#credential)
    * [Firewall Exception Types](#firewall-exception-types)
        * [Birthright Firewall Exception](#birthright-firewall-exception)
        * [Normal Firewall Exception](#normal-firewall-exception)
        * [Unusual Firewall Exception](#unusual-firewall-exception)
        * [CISO Firewall Exception](#ciso-firewall-exception)
    * [RFC 1918 addresses and NAT](#rfc-1918-addresses-and-nat)
    * [Credential Segmentation Concepts](#credential-segmentation)
* [To Be Enclaves and Zones](to-be-network.html)
* [Current Enclaves and Migration Plan](current-network.html)
* [Wireless Networking](wireless.html)
* [System Examples](system-examples.html)
* [Questions and Answers](questions-and-answers.html)

------------

------------
  
# Definitions and Key Concepts

## Network Environment

A **Network Environment** (aka a Network Segment) is a means to manage both the logical layout of the ORNL network and the accreditation of the ORNL computing infrastructure under FISMA.  As described below, network environments are organized in a hierarchy, starting with Enclave at the top level, one or more Zones in an Enclave, and zero or more SubZones in a Zone.  

Network Environments have the following attributes:
    * Name: Names are laid out in dot walking notation, in the form Enclave.Zone.SubZone
    * Subnets: A network environment has zero or more IP address blocks assigned to it.  An environment may have zero subnets if it is outside of ORNL's network (such as a Cloud zone) or if it represents an off-network environment.
    * Owner: Each network environment has an ORNL Staff member who is the primary person responsible for that environment, including approving firewall exceptions going in or out of that environment and what users have permissions to register devices (or device interfaces) in that envioronment.  
    * CIA level: The Confidentiality, Availability, and Integrity level for the environment.  Note that a network environment may not contain an environment with a higher CIA level (recognizing that Moderate with Enhanced Controls (M/EC) is still Moderate).
    * Minimum Authentication Level: The minimum credential level (as defined in NIST 800-63 rev2) for operating system authentication permitted in that environment. 
    * Allowed device types: The types of networking devices permitted in that environment.
    * Authorized users: The users (typically based on roles within NetReg) who have permissions in that environment, such as see registered devices, change device registrations, and add/remove devices or device interfaces.
    
A device (computer, switch, virtual machine, ...) is registered to a specific environment.  Generally, devices will be registered to Zones or SubZones, but some types of devices  (particularly networking equipment) will be registered in an Enclave.  The device registration determines the controls that apply to the device, such as the types of accounts (UCAMS, DCAMS, XCAMS, etc) which may be used on the device and the CIA levels for which that device is approved.  

A device will have one or more network interfaces, which will each be registered in a network segment.  Normally, all network interfaces for a device will be registered in the same segment as the device, and only a relatively small number of people (primarily those in IT Networking) will have the privileges to register a network interface outside of the network segment to which the device itself is registered.  

## Enclave

The highest-level Network Environment is the **Enclave**.  For accrediation, an Enclave is a FISMA system, and site office approval is needed for each new Enclave.  For logical network construction, Enclaves are mostly containers for Zones.  However, some networking devices and interfaces can and do get registered in Enclaves.  Enclaves are also useful for certain types of firewall rules, such as permitting access to a service from all endpoints in the Workstations Enclave (rather than having to do firewall rules for each Zone separately and then having to add new rules as Zones are created).  Enclaves can also be viewed as representing a slice or section of either the core router (most enclaves) or the border router (DMZ).

Enclaves are credential boundaries – a given user credential (such as an Active Directory Account) may only be used for interactive OS authentication within a single Enclave.  Note that user credentials (particularly standard user credentials) may be used with applications in multiple enclaves.  For example, assume ORNL\xyz is a primary userID in UCAMS. That account can be used for interactive OS authentication only within the workstations Enclave, but it may also be used for authentication to applications hosted in Servers, applications hosted in the DMZ, applications in the cloud, and applications hosted in Open Research.  

As noted above, a network environment cannot contain another environment with a higher CIA level.  For example, the Workstations enclave has a CIA level of {M,L,L}.  It cannot, therefore, contain a Zone or SubZone with a CIA level of {M,M,M}, but it could contain a {L,L,L} zone.  Where a Zone  has a lower CIA level than the Enclave, then some of the controls may be overridden to a less stringent level (tailoring down).  In some cases, however, a Zone can tailor up to have enhanced controls within a given CIA level where that is appropriate for the type of information involved.  An example of tailoring up is the MEC Zones within Workstations and Servers Enclaves, used for certain types of particularly sensitive information. Similarly, the Servers.KLAN zone may be viewed as having enhanced controls for both confidentiality and integrity.  

## Zone
To enable more fine-grained management controls and network-level protections, each Enclave consists of zero or more **Zones**.  Zones are identified based on the Enclave in which the system is located, e.g Workstations.Level2, DMZ.IntServices, or IND.Printers.  Nearly all devices and network interfaces will be registered in a Zone (or a SubZone).  

From an accreditation perspective, a Zone inherits all of the controls from the parent Enclave.  The Zone can then specify additional or tailored controls for that collection of systems within the Enclave.  

From a networking perspective, each zone is separated from all other zones, typically using a layer 3 firewall with a default deny policy.  For each zone, there are certain types of firewall exceptions that are expected and typical behavior, with other types of firewall exceptions requiring additional approvals.  

Generally, all of the network interfaces and virtual machines for a device will be in the Zone to which that device is registered.  Exceptions include: 

* Network devices managed by ITSD
* Hypervisor, storage, and converged infrastructure systems managed by ITSD
* Management interfaces (e.g. LOM cards) for servers will be in a LOM zone
* Other exceptions as described in a cybersecurity plan 

Ideally (the mechanism to do this does not exist), the owners of all of the Zones involved should be aware of and approve devices which have interfaces that span multiple zones, in the same way that those Zone owners would be involved in the approval of firewall exceptions between zones.  

## SubZone
To provide further isolation and risk management, a Zone may be further divided into **SubZones** (formerly called a Context).    

From an accreditation perspective, a SubZone inherits all of the controls of the parent Zone, and may override some of these to tailor the controls to the requirements of the applications and systems hosted in that subzone.  

From a networking perspective, a SubZone is at least isolated from all other systems in the zone by some type of network restriction, typically a layer 3 firewall (physical or software defined).  Servers.Moderate.SAP is an example of a set of physical systems segmented into a SubZone by a physical firewall.  DMZ.Tenant.SharePoint is an example of a set of virtual systems isolated using NSX software defined networking.  

## Credential
A **Credential** is the combination of a given user identifier (username) and authentication method (such as a password, SecurID token, or smart card).  Where single sign-on (SSO) methods (such as a Windows hash) enable multiple authentication methods to result in the same SSO identifier, those credentials must be considered as a single credential. As an example, it is possible to authenticate to a Windows domain with a password and with a smart card.  That is a single credential for segmentation purposes.  In most cases, it is appropriate to equate a credential with specific userID or computer account.  

However, UCAMS primary accounts that are replicated into XCAMS are considered to be separate credentials for segmentation purposes. While they do have a common password, they have different User Principal Names (UPNs): xyz@ornl.gov versus xyz@extranet.ornl.gov.

## Firewall Exception Types

Firewall exception types are largely governed by the types of actions that a protocol is used for (client read, server read, client write, and server write).  For example, the ldaps protocol is generally used for client read -- the client is asking whether a password is valid or retrieving attribute information about a person.  The http protocol is often a client read, but may be used for client write (and there are aspects of server read (e.g. cookies and http headers) and server write (e.g. cookies).  Some protocols, such as those used for interactive login, are extremely powerful and are often used for all four types of actions.

Where this matters is in terms of Confidentiality, Integrity, and Availability.  For Confidentialty (which is often the primary focus for Moderate controls), information flow from higher to lower requires compensating controls (client in a lower zone reading from a higher zone or a server in a higher zone writing to a lower zone).  However, information flow from the lower zone to the higher zone is generally permitted (client in a lower zone writing to a higher zone or server in a higher zone reading from a lower zone).  For Integrity and Availability, however, the directions of concern are reversed: a higher zone reading from a lower zone or a lower zone writing to a higher zone require compensating controls.   

It is generally not possible to determine the types of actions involved in a firewall request, based simply on the port and protocol.  However, there are predominant uses and ORNL's implementations of many protocols often provide the necessary compensating controls.  For example, the DMZ enclave is designed to provide services to the Internet (which is completely untrusted and below L/L/L in terms of its CIA assessment).  However, externally facing services in the DMZ must have compensating controls to ensure that only authorized users are allowed to read information (to protect Confidentiality) and that there is both appropriate authorization and input validation for information written from the Internet (to protect Integrity and Availability).

## Birthright Firewall Exception
These exceptions are set up at the network zone level and are typically from one zone to another zone.  They do not need to be requested for individual systems, such as ports 80 and 443 from Workstations.MFA to the Internet.  In some cases, these exceptions may be implemented through proxies or NAT services, but are effectively still birthright firewall exceptions.  Where possible, these exceptions get documented as Normal Firewall exceptions in the firewall management infrastructure.  Where manual firewall exceptions must be used, a Risk Assessment should be completed as documentation.  

### Normal Firewall Exception
The firewalls between zones have a default deny policy, blocking traffic to all other zones. For each zone, however, certain types of inbound and outbound network traffic are normal, anticipated behavior and are considered **Normal Firewall Exceptions**.  These exceptions either involve normally allowed flows, as describe above (e.g. lower integrity system reading from a higher integrity system or writes between two moderate integrity systems), or where the controls for the zone are designed to provide the necessary compensating controls (e.g. strict input validation when writing from lower integrity to higher).

As described above, the External Services zones in the DMZ enclave (DMZ.ExtServices) are designed for servers providing Internet-facing services.  Exceptions into DMZ.ExtServices from the world are part of the normal pattern of behavior for this zone and are processed through the firewall exception management system.

Normal firewall exceptions are submitted through SSR and should be approved unless a reason exists to disallow.

### Unusual Firewall Exception

In some cases, additional documentation is required for a firewall exception to demonstrate that appropriate controls are in place and to justify an unusual traffic pattern.  These are termed **Unusual Firewall Exceptions**.  An example is traffic from Workstations.Low into Servers.Moderate.  In this case, the firewall request needs to document how the application using this exception either does not expose Moderate data or enforces MFA.  Similarly, a system in DMZ.InternalServices can be approved to reach out to the Internet, but the request should document the kind of information being pulled back and how that information is processed.  

Unusual firewall exceptions are submitted through SSR and should be approved if the documentation describes appropriate information protection.  

### CISO Firewall Exception
When a firewall exception is needed that is not part of the typical patterns documented for a zone, the exception must be approved by the ORNL CISO through the risk acceptance process.  The need for the firewall exception should be documented, along with the mitigating controls through a risk acceptance form.

CISO approval and a risk acceptance form is also required for any firewall exceptions which must be manually entered, rather than managed through the firewall management system.  

## RFC 1918 Addresses and NAT

Internal networks should use RFC 1918 ("non-routable") addresses to the extent possible.  In many cases, even Internet accessible servers can use RFC 1918 addresses, with the routable IP address attached to a load balancer or static NAT route on a firewall. For RFC 1918 systems that need to access the Internet (outbound traffic), Network Address Translation (NAT) will be done at the border router.

In some cases, it may be desirable to use NAT within ORNL, though the cybersecurity systems are not presently capable of handling this.  One of the potential structures possible with NSX is for multiple SubZones to use the same IP addresses internally, with subzone-internal DNS.  This has the potential to allow for simpler set-up of Prod, QA, Dev, and Test systems, with identical internal names and IP addresses.  Only the load balancer endpoints and static NAT routes in the parent Zone need have different DNS names and IP addresses.  This also has significant potential for upgrades of production systems.

Even where SubZones do not use overlapping IP address ranges, it may still be desirable to use NAT within ORNL to ensure that no route exits from outside the SubZone into systems which do not need to be reachable.  

# Credential Segmentation 

## Rationale and Limitations

Credential segmentation serves to minimize risks for lateral movement by an adversary, particularly between enclaves, and to increase the probability of detection of compromised credentials. Credential segmentation is particularly important as a preventative measure in Windows environments, due to pass the hash attacks, and in other places where single sign-on (SSO) technologies are used.  

However, credential segmentation creates usability challenges for users and administrators.  It can be difficult to keep track of which credentials are to be used for which system, leading to operational inefficiency and wasted time.  

In the absence of Kerberos or other SSO methods, credential segmentation provides less direct benefits as a preventative control and is primarily useful as a means for detection. An adversary may attempt to use a credential outside of the environment in which it is supposed to be used, leading to increased chances for intrusion detection.  

## Implementation

An Enclave may use no more than one *CAMS system and associated Active Directory domains for computer and operating system user authentication. Where a domain is used in multiple enclaves (such as ornl.gov for Workstations and Servers), a given credential may not be used for OS authentication in more than one enclave.  A privileged user credential must not be usable for OS authentication in multiple enclaves, and may be further restricted based on policies established by application owners.  
 
It is generally acceptable for a credential to be used interactively on multiple systems across an enclave, including across zones.  For example, a systems administrator may use a single credential for work across multiple zones in the Servers enclave.  But that credential may not be used in the DMZ enclave. Likewise, a UCAMS primary account can be used on multiple workstations across the Workstations enclave.  But that primary account may not be used for interactive login in the Servers enclave.

However, primary accounts may be used for application authentication across multiple enclaves.  A UCAMS primary account can be used to authenticate to applications hosted in Servers and in the DMZ.  

SBMS Procedures, IT Operating Procedures, or system owners may require more restrictive credential segmentation policies.  For example, the accounts used for administration of the Ping Fed servers may only be used within the Ping Fed application.  That is a policy defined by the Ping Fed application owner.  Likewise, the high level Active Directory domain administration accounts may only be used within the specific zone used for the domain controllers.  Where an account is restricted to a particular set of systems, it is best if those systems are contained in a single Zone or SubZone.

There are cases where a credential must be used across enclaves, such as to push data from Servers to DMZ.  This should be done using a service account (not used for interactive login) which is specific to that particular application system. The credential use should also consider ways to mitigate misuse, both in terms of prevention and detection.  Setting up IP address restriction and using certificate-based authentication are two possible approaches to risk mitigation when credentials must cross enclave boundaries.Network Segmentation
