---
layout: page
title: Current Network Zones and Migration Plan
category: pages
---

***ORNL Internal Use Only***

-----------


**Summary**: This document describes the current network enclaves and zones, as well as how these will get mapped into the new environment.  This is a working copy and should be reconciled against the cybersecurity program plan and information in NetReg.  

Supercomputing
RAN
Ops
Open Research
DMZ


# Operations (OPS)
The OPS enclave contains those systems (servers, workstations, printers, etc.) that provide business support for the ORNL research mission. These systems are generally funded by the SBMS management systems or within the support organizations of ORNL.

## KLAN (sub protection zone)
The only systems allowed in the KLAN are ORNL.GOV domain controllers, administrative workstations and patching system.

## Infra-Dev (sub protection zone)
The infra-dev sub-enclave was created to separate our development active directory domain and its associated systems from our production system.

## SRV-MGMT (sub protection zone)
The SRV-MGMT zone has been setup for lights out monitoring of servers in DMZ, Operations (OPS) and Research Area Network (RAN).

## OPS.INFRA (sub protection zone)
The OPS.INFRA sub enclave contains those systems that are managed by IT and provide services to a wide range of RAN devices. Examples of systems that will reside in this sub-enclave include systems that will provide the following services: DNS, Patching, Configuration Mangement, SAP, Email, Web, SharePoint.

## Moderate with Enhanced Controls (M/EC) (sub protection zone)
The Moderate with Enhanced Controls (M/EC) PZ contains those systems which process moderate information that ORNL has determined require additional (enhanced) controls to protect the information, including, UCNI, C/FGI-Mod, and NNPI.

## DOE Site Office (sub protection zone)
The DOE PZ contains DOE owned devices only.

## OPS.SERVERS (sub protection zone)
The OPS.SERVERS enclave contains systems that support operations but do not fit into the OPS.INFRA sub-enclave. This may be because the system is not managed by IT or because is supports only a portion of the ORNL community such as a division or directorate server. Systems may be placed in this sub-enclave because it is desired that the services they offer be separated from the OPS.INFRA sub-enclave.

# Research Area Network (RAN)
The RAN protection zone contains systems used to conduct research. This includes most of the general purpose desktop systems, servers, instruments, printers, etc. that are used to perform and document open research within ORNL, and systems used by researchers to conduct proprietary or closed research. The overall RAN system will operate at the Moderate level, which includes all categories of unclassified sensitive information (from Guide to Handling Sensitive Information Exhibit in SBMS) and DOES include ORNL email.

## ARM_CSAT (sub protection zone)
The ARM_CSAT sub-enclave contains devices for ARM and CSAT

## Controlled Research (sub protection zone)
The Controlled Research protection zone is a Research PZ which contains those systems used by researchers to create, store and process proprietary, export controlled, protected CRADA, applied technology or similar information.

## RAN.ICS (sub protection zone)
The RAN.ICS sub-enclave is for the Industrial Controls Systems network and contains the ICS assets within ORNL that exist in industries such as electrical, water, oil and gas.

## RAN.INFRA (sub protection zone)
The RAN.INFRA sub enclave contains those systems that are managed by IT and provide services to a wide range of RAN devices. Examples of systems that will reside in this sub-enclave include systems that will provide the following services:

DNS
Patching
Configuration Management
All services in RAN.INFRA will be a duplication of a service in OPS.INFRA. OPS.INFRA services will be duplicated to RAN.INFRA if the service is deemed necessary for RAN to function independent from OPS.
RAN.SERVERS (sub protection zone)
The RAN.SERVERS sub-enclave will be a general location for systems that provide support to ORNL.s research mission. These servers do not need to be managed by IT and may be managed by individual research projects.

# Supercomputing (NCCS)
The supercomputing (NCCS) protection zone includes those systems that comprise the National Center for Computational Sciences.

# DMZ
The DMZ protection zone includes those systems containing web and ftp servers hosting public information that is accessible via anonymous access for any person or system on the Internet.

## V6Test (sub protection zone)
The V6Test sub-enclave contains devices that are used in the testing and development of ORNL's IPv6 infrastructure.

## ANLFED (sub protection zone)
The ANLFED system contains a backup system for the Argonne National Laboratory Cyber alert Federation system.

## DMZ.WWW (sub protection zone)
The DMZ.WWW sub-enclave contains servers and infrastructure used for WWW.ORNL.GOV and its associated web sites. Devices in this sub-enclave should be IPv6 enabled.

## DMZ.ExtServices (sub protection zone)
The DMZ.ExtServices sub-enclave contains servers whose services need to be accessible from the internet and also need to be accessible from all of Open Research and DMZ. Devices in this sub-enclave should be IPv6 enabled.

## DMZ.InternalServices (sub protection zone)
The DMZ.InternalServices sub-enclave contains servers whose services need to be accessible only from Open Research or DMZ.

## DMZ.ExtDNS (sub protection zone)
The DMZ.ExtDNS sub-enclave contains servers that provide ORNL's External DNS capability. Externally facing devices in this sub-enclave should be IPv6 enabled.

## DMZ.CCP (sub protection zone)
The DMZ.CCP sub-enclave has been setup to allow for segmentation of ORNL machines and systems that process credit card transactions. Devices in this sub-enclave should be IPv6 enabled.

## DMZ.LOM (sub protection zone)
The DMZ.LOM sub-enclave has been setup for lights out monitoring of servers in the DMZ.* sub PZs.

## DMZ.WEB (sub protection zone)
The DMZ.WEB sub-enclave contain servers that provide web services to the internet but are not part of any other DMZ.* zone. Devices in this sub-enclave should be IPv6 enabled.

## DMZ.SAP (sub protection zone)
The DMZ.SAP sub-enclave contains systems that provide SAP connections from the internet and also reach back into the OPS.SAP enclave. Devices in this sub-enclave should be IPv6 enabled.

## DMZ.SharePoint (sub protection zone)
The DMZ.SharePoint sub-enclave contains the servers that provide web connectivity from the internet to our external SharePoint farm. Devices in this sub-enclave should be IPv6 enabled.

## DMZ.DLAN (sub protection zone)
The DMZ.DLAN sub-enclave contains those devices that serve as Windows Active Directory domain controllers for the DMZ Active Directory forests and the management stations used to manage the Active Directory forest. Access to this enclave will be strictly controlled. Accounts used to manage devices in this enclave must use smartcard authentication.

## EXT-ORNL (sub protection zone)
The EXT-ORNL sub-enclave contains devices that reside in front of ORNL's border firewall and are used to manage and monitor ORNL's border network.

## Visitor (sub protection zone)
The Visitor sub-enclave is for use by systems of ORNL visitors.

## XLAN (sub protection zone)
The only systems allowed in the XLAN are Extranet.ORNL.GOV domain controllers, administrative workstations and patching system.

# Open Research
The Open Research protection zone includes those systems used to conduct open research that creates, stores, and processes fundamental research information. Open research is defined as fundamental research that is openly shared with a broad community, does not include applied or export controlled research, and does not collect or process any sensitive information (as defined by Sensitive Information Categories in SBMS). This protection zone may contain desktops and servers used by staff and collaborators. Note: Desktops in this PZ can not be used to access sensitive information including email. Desktops used to read email, record timekeeping in PALS, access SAP resources, or access other sensitive information must remain in the Research Area Network (RAN) PZ. Systems in the RAN PZ can be used to access resources in the Open Research PZ.

## CSIIR_V6 (sub protection zone)
The CSIIR_V6 sub-zone was created to provide V6 capability for a project within the CSIIR group in the Computer Science & Engineering Division at ORNL

## ARM-DMF (sub protection zone)
The ARM-DMF sub-zone was created for the ARM DMF project