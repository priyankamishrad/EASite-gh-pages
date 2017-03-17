---
layout: page
title: Firewall Rule Classification
category: pages
---


***ORNL Internal Use Only***

***DRAFT***

-----------

# Table of Contents

* [Overview](#overview)
* [Rule Classification Lists](#rule-classification-lists)
* [DMZ Enclave](#dmz-enclave)
	* [DMZ.ExtServicesLow](#dmzextserviceslow)

# Overview

As noted in [Network Segmentation Key Concepts](netseg/home#firewall-exception-types), there are three types of firewall exceptions: Normal, Unusual, and CISO.  Each network environment defines the types of firewall exceptions which fall into each of these categories.  The various enclave documentation provides a graphical overview of the different rule types.  This document provides additional supporting detail.


# Rule Classification Lists

To determine the firewall exception type, you need to know the source (client) and destination (server) network environments and the protocol.  Where the one environment is outside ORNL's accreditation boundary (i.e. someplace on the Internet), the rules for the other environment apply.  Where both environments are within ORNL's accrediation boundary, then the rules for both source and destination need to be evaluated.  For an example, for an exception permitting traffic from the World (source) to DMZ.ExtServicesLow, only the rules for DMZ.ExtServicesLow need to be evaluated. But for an exception from DMZ.Data/Relay to Workstations.Level2, the rules for both Zones need to be considered.

A Zone inherits all rules from its parent Enclave, and a SubZone inherits all rules from its parent Zone.  

Where multiple rules apply to a proposed firewall exception, the most restrictive rule wins.  

In the tables below "World" as a network environment matches all network environments, whether inside ORNL's accrediation boundary.  For example, with DMZ.ExtServicesLow, inbound firewall exceptions from anywhere are considered Normal.  However, the rules for the DMZ enclave indicate that interactive login protocols (rdp, ssh, and powershell) are considered CISO exceptions when coming from the Internet

# DMZ Enclave

| Source | Destination | Protocol | Exception Type | Notes |
| ------ | ----------- | -------- | -------------- | ----- |
| Internet | DMZ.* | rdp,ssh,powershell | CISO | |
| DMZ.* | Internet | rdp,ssh,powershell | CISO | |
| Other Enclaves | DMZ.* | rdp,ssh,powershell | Unusual | Validate that destination server enforces L4 authentication |
| Internet | DMZ.* | netbios, smb | CISO | | 
| Other Enclaves | DMZ.* | netbios, smb | Unusual | |

## DMZ.ExtServicesLow

| Source | Destination | Protocol | Exception Type | Notes |
| ------ | ----------- | -------- | -------------- | ----- |
| World | DMZ.ExtServicesLow | any | Normal | Ensure that logs from the service are going to Splunk (e.g. web server logs for http and https). |
| DMZ.ExtServicesLow | Internet | any | Unusual | Define reason server needs to reach out and what kind of data it is sending/receiving. |