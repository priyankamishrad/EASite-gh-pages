---
layout: page
title: Wireless Networking in Network/Credential Segmentation
category: pages
---

***ORNL Internal Use Only***

***DRAFT/Predecisional****

-----------

**Summary**: This document describes the proposed use of wireless in the networking/credential segmentation model being deployed in the FY2016 and subsequent network segmentation and multifactor authentication (MFA) project.  

# Design Goals 

* If the user has logged into the device using a UCAMS account with MFA, then that device may and should be put into the Workstations.MFA network environment.  It can go into Workstations.Level2, but that is a user-hostile solution unless there's an automatic solution (such as VPN or Direct Access) that gives the user access equivalent to Workstations.MFA.

* ????? Can we assume that Workstations.MEC cannot be reached by a wireless environment?  NO.  Need to address wireless for both DOE and MEC enclave.

* If the user has logged into the device using a UCAMS account with a password, then that device should be put into the Workstations.Level2 network enviromnent.  

* If the user has logged into the device using an XCAMS account (regarless of authentication mechansim), the device should be put into the OR.Workstations network environment.

* ORNL-owned devices that do not use UCAMS or XCAMS either go into the IND.Low environment (no Internet access) or OR.Devices environment (Internet access, but no ORNL internal access)

&&&&& TODO.