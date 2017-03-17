---
layout: page
title: Access Control (AC)
category: pages
---


# ***DRAFT FOR COMMENT***

----------


Within this document, interactive login refers to a user who is able to interact directly with the operating system, typically through directly logging into the system (such as on a laptop or desktop computer or a server console) or through an OS-level remote access protocol such as Remote Desktop Protocol (RDP) or secure shell (ssh).  Application access refers to a user who is interacting with a program or abstraction layer above the operating system, generally through a web browser.  


## AC-01 Access Control Policies and Procedures

### AC-01.Low

The ORNL Computer Security Program Plan (CSPP) and this Common Control document establish the policies associated with Access Control.  These documents are reviewed as part of the continuous monitoring program and updated as needed.

### AC-01.Moderate

No additional requirements for moderate systems


## AC-02: Account Management

### AC-02.a Types of information system accounts

ORNL has four Credential and Access Management Systems (CAMS): XCAMS, UCAMS, DCAMS, and SNOCAMS. Except as noted below, XCAMS is classified as a Level 1 authentication system.  UCAMS, DCAMS, and SNOCAMS when used with passwords are classified as Level 2 authentication systems.  UCAMS, DCAMS, and SNOCAMS when used with multifactor authentication (SecurID or smart card) are classified as a Level 3+ (enhanced Level 3) authentication system.  Witin all of these CAMS, accounts are not shared between people (except where documented in an exception).

Within XCAMS, any internet user may establish one or more XCAMS primary accounts, which are used for standard user access to low controls systems.  ORNL staff may establish one or more XCAMS secondary accounts, which may be used for service accounts and privileged user access to low contols systems.  Because of the higher level of identity proofing, an XCAMS secondary account is considered a Level 2 credential.

XCAMS permits a user to bind supported external Level 1 or greater credentials to an XCAMS primary account, allowing that external credential to replace the XCAMS password as the credential validator for an authentication transaction. Examples of this include binding a Google account to an XCAMS primary account, letting the user authenticate via the OAuth or SAML protocols with that Google account to assert the XCAMS primary account identity.  External account binding is not permitted for XCAMS secondary accounts.

Within UCAMS, primary accounts are ones used for standard user access to typcial work systems.  A given person has a single primary account.  UCAMS secondary accounts are used for service accounts, privileged user access to systems, some forms of testing, and other cases where using a primary account is problematic or insecure.  UCAMS primary accounts are (by default) replicated to XCAMS, allowing this Level 2 credential to be used in environments where a Level 1 credential is permitted. 

### AC-02.b Account Managers

Each CAMS has a defined set of ORNL staff who manage that system and approve changes to the system. 

### AC-02.c Conditions for group and role memberships

### AC-02.d Specifies authorized users for the information system

### AC-02.e Requires approvals to create accounts

### AC-02.f Creates, enables, modifies, and removes accounts with procedrues

### AC-02.g Monitors user of information system accounts

### AC-02.h Notifies account managers

### AC-02.i Authorizes access to the information system

### AC-02.j Reviews accountsfor compliance

### AC-02.k Establishes a process for reissuing shared/group accounts when individuals are removed from the group

### AC-02.Low

Read access to information classified as Low Confidentiality requires a Level 1 or greater credential.

Write access to information classifed as Low Integrity requires a Level 1 or greater credential.

Standard user interactive login to a system classified as Low Availability requires a Level 1 or greater credential.  Privileged user interactive login to such systems requires a Level 2 or greater credential.


#### AC-02.Low.OpenResearch

The Open Research enclave uses the XCAMS account management system and the associated extranet.ornl.gov Active Directory for all accounts.

##### AC-02.Low.OpenResearch.

#### AC-02.Low.IND


### AC-02.Moderate

Read access to information classified as Moderate Confidentiality requires a Level 2 or greater credential.  

Write access to information classfied as Moderate Integrity requires a Level 2 or greater credential.

Standard user interactive login to a system classified as Moderate Availability requires a Level 2 or greater credential.  Privileged user interactive login to such systems requires a Level 3+ or greater credential.

#### AC-02.Moderate.DMZ

#### AC-02.Moderate.Servers

#### AC-02.Moderate.Workstations

#### AC-02.Moderate.ICS

#### AC-02.Moderate.Enh-1

#### AC-02.Moderate.Enh-2

#### AC-02.Moderate.Enh-3

#### AC-02.Moderate.Enh-4