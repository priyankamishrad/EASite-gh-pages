---
layout: page
title: Privileged Access to Unix/Linux Servers & Clusters in the Workstations Protection Zone
category: pages
---


## Overview
MultiFactor Authentication (MFA) Enforcement is part of our contract with DOE and must be enforced for privileged users on Linux servers in the Workstations (previously known as the Research Area Network) PZ by September 30, 2016. Users are considered privileged if they have root level access via sudo or a root level account. Users with a restricted set of privileges (e.g. the ability to run yum via sudo) are not considered privileged.

Privileged users must have a secondary userid in UCAMS that is dedicated to administering servers in the Workstations PZ. These accounts will only be allowed to SSH in directly using a SmartCard or may use a password to authenticate if the login originates from a Workstations PZ level 4 jump server. Privileged users will be automatically granted full sudo rights on the server and any standard users that have full sudo rights or a root account must have those privileges removed and optionally create a privileged account. The ability to su to another user will be restricted to privileged users and the root account.


<br />
## Prerequisites

### System Requirements
- [ ] Running an ORNL [supported](https://ornl.service-now.com/its/kb_view.do?sysparm_article=KB0100342) Linux distribution as of October 1, 2016.
	- [ ] RHEL 5 and Ubuntu 12.04 will not support these changes.
- [ ] Registered as a server or cluster in [NetReg](https://netreg.ornl.gov).
- [ ] Registered in the Workstations protection zone.
- [ ] Systems using SecurID for ssh access are currently excluded.
- [ ] Systems with a **Smart Card Authentication Enforcement Policy - Linux Privileged Access** exception through the [Device Exception System](http://home.ornl.gov/~dex/prod/) are excluded.

### User Requirements for Privileged Access
- [ ] [Review](https://portal02.ornl.gov/sites/BI/_layouts/ReportServer/RSViewerPage.aspx?rv:RelativeReportUrl=/sites/bi/dashboards/deviceswprivaccess.rdl) systems where you have privileged access and remove any unneeded rights.
- [ ] [Configure](mfa/home) your SSH client to use SmartCard auth when connecting to the Workstations PZ jump server.
- [ ] Request a secondary UCAMS userid to be dedicated for privileged access in Workstation (if you do not already have one).
- [ ] Request the **IT Privileged Access** -> **Unix/Linux for RAN** resource in [UCAMS](https://ucams.ornl.gov) for your Workstations PZ account.
	- [ ] You may get multiple notifications to resend your password.
	- [ ] Recommendation is to change your password to 16+ characters after all resources have been approved.
	- [ ] You will be granted access to the Workstations PZ Linux jump server and the workstation password server resources as part of this request.
	- [ ] If you wish to use your Workstations PZ account with SecurID, you will need to manually request the mapping in UCAMS.


<br />
## Technical Enforcement

### Changes being made to the system
* A ranprivaccess group will be created and populated with local users that have the IT Privileged Access -> Unix/Linux for RAN UCAMS resource.
* The ranprivaccess group will be granted full sudo rights via an entry in /etc/sudoers.d/level4_ran_priv.
* The PAM configuration for su will be modified to restrict su access to root and members of the ranprivaccess group.
* The SSHD cofniguration file will be replaced with a centrally managed version.
	* root access will be restricted to originating from a Workstations PZ jump server or using a SSH keypair. See the section below for details.
	* members of the ranprivaccess will be required to use a SmartCard or connect from a Workstations PZ jump server.
	* standard users will be able to (but not required) to SSH in using a SmartCard.

### Root SSH keys

SSH keys for the root user (/root/.ssh/authorized_keys) should only be configured if absolutely necessary (such as to facilitate large file syncing or administration of machines within a cluster). If feasible, restrictions should be put in place to limit access to originating from specific hosts by adding a from section to your key.

**Example SSH key for root (/root/.ssh/authorized_keys)**
```
from="trusted-host.ornl.gov,160.91.11.2" ssh-rsa AAAAB35zaC1Zc2EAAAABIwAAAP8393iUQUGJLTTGRlaGwIT0/PK9n831LvuLBOjfzSTt6kfWYNcvLgOMN9nIpo1kUTrK2TKuYCSHCm0luLSu8jgTHRMKZqWwBUqY0chRhmU/7lIaomoTp311D9/J23vrFok/84yjt3kz/GESPDBVboXuZsq3ljizRAeHQIBcvAZO93J4zu3o7cCST204bITIMTlhxOfacJ1p6G2h5yxl5kPq1CPTRo0KhU1rLkVq4sRYfUBf81jg8W1I3jD1Om37kGJ0rQks+0dteFFgNW10++iGomyUmO1YPDQnBfuC/eGXftsj1PBbK4s6fb4jXOP7FI30LRfP9jIx9j3aD9EuDbE=
```

<br />
## Implementation Timeline

### Deadline of August 31, 2016
- [ ] Verify that you have a secondary account for privileged access in Workstations PZ.
- [ ] Verify that you have the **IT Privileged Access** -> **Unix/Linux for RAN** resource in UCAMS.
- [ ] Review the privileged systems list and remove an unneeded access.
- [ ] Request a DES exception for any ITSD owned systems that cannot enforce the new policy.

### Deadline of September 6, 2016
- [ ] Test your access to the Workstations PZ jump servers (ranlogin01 / ranlogin02).

### Deadline of September 7, 2016
- [ ] ITSD owned systems will enforce the new policy beginning on 9/7
- [ ] Request a DES exception for any ORNL owned systems that cannot enforce the new policy.

### Deadline of September 14, 2016
- [ ] Send any issues or constructive criticism as a result of this change to Brian Zachary and Jim Trater.

### Deadline of September 21, 2016
- [ ] ORNL owned systems will enforce the new policy beginning on 9/21.

### Deadline of September 30, 2016
- [ ] Upgrade, retire or request a DES for any Linux systems running an unsupported (at ORNL) version of Linux.

