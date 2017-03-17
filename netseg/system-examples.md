---
layout: page
title: Network Segmentation System Examples
category: pages
---

***ORNL Internal Use Only***

-----------

**Summary**: This document provides examples (or links to examples) of how systems are constructed based on the credential and network segmentation being put into place at ORNL.  

# Topics

* [DMZ Examples](#dmz-examples)
    * [DMZ Splunk Forwarder](#dmz-splunk-forwarder)
    * [Ironports](#ironports)
* [Other Remote Access](#other-remote-access)
    * [Login Servers](#login-servers)


# DMZ Examples

## DMZ Splunk Forwarder

The DMZ Splunk forwarder needs to be reachable from every other DMZ zone and it needs to be able to push information to the internal splunk systems (in the Servers enclave).  This means that the Splunk forwarder needs to be able to initiate traffic from the DMZ to ORNL internal.  It does not neet Internet access. 

This follows the pattern for the [Data & Relay](dmz-enclave#data-relay) enclave

## Ironports

The Ironports send and receive traffic from the Internet (world) for inbound and outbound email. They also need to receive traffic from the Servers (Ops) enclave for outbound email.  Under normal circumstances mail inbound to ORNL goes from the Ironports to FireEye and then DEX, and from those systems to internal email server.  The inbound exception from the Ironports to internal email servers is allowed, in case it becomes necessary.  The Ironports do not need to talk to other DMZ systems apart from those that are needed for authentication, client health, and basic network services.  For example, the Ironports need to talk to the LDAPD system for account authentication, splunk/syslog forwarders, DNS, and NTP.

This follows the pattern for the [External Proxy](dmz-enclaves#external-proxy).

# Other Remote Access

## Login Servers

The login servers are Internet-facing bastion hosts used to provide ssh access to ORNL-internal systems.  Many would expect to find thes systems in something like DMZ.ExternalServicesModerate, but the correct location is actually Workstations.Moderate (currently, they are in RAN).

The intent of the login servers is ORNL staff log in using UCAMS-managed credentials (with SecurID as the authenticator).  Once logged in, the intent is that users have access to ORNL systems as if they were on a (Linux) workstation physically located on site.  Given that the user has authenticated with MFA, access to Moderate confidentiality information is permitted.  

There are two basic design patterns to use to meet this requirement:

1. Put the system in the DMZ.  This violates the principles a) that there are no systems in the DMZ where standard users log in interactively and b) that DMZ systems use DCAMS for authentication.  If the system has a single network interface, then there have to be special firewall exceptions to allow access from the DMZ to the Servers.* systems to which workstations have access.  If the system has multiple network interfaces, then we have a bridge between enclaves that presents some special challenges from a logging and management perspective.

2. Put the system in the Workstations enclave.  This violates the principle that there aren't systems in Workstations that have direct inbound firewall exceptions. However, that is addressed via a risk acceptance process, which documents the special precautions that are taken with the login servers to mitigate the risks of a system in Workstations (RAN) with a direct inbound firewall exception from the world.  No special management is needed 

