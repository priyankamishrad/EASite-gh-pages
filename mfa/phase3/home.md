# Multi-Factor Authentication Phase 3
***

<br />
## Overview
***
To improve security across the lab and promote greater consistency and standardization, the login process to Linux clusters, desktops and servers is changing. These changes apply to computers in the Workstations Protection Zone, which contains most desktops as well as customer clusters and servers. Standard accounts will be required to use a SmartCard for SSH access, and access to root and service accounts will be restricted to pre-configured keys and jump servers. The outline below gives an overview of the access requirements for each device and account type. The enforcement of these new policies will take place automatically on eligible computers, however you will need to consult the sections below to ensure that your accounts, computer OS and SSH client are correctly configured to work in the new environment.

<br>
### Linux Desktops/Laptops

##### Privileged Access
1. Direct SSH access as a root user will require using a L4 Jump Server or a previously configured SSH key pair.
2. sudo privileges may be granted to UCAMS accounts that have a need to perform administrative tasks.

##### SSH Access
1. Standard users will be allowed to SSH with any of the following methods
	* Smart Card
	* on-disk SSH keys
	* password authentication from a L4 jump server or login[1|2] or Citrix
	* the username must match the UCAMS account name
2. Service accounts will be allowed to SSH with any of the following methods
	* on-disk SSH keys
3. Root and alternate root accounts will be allowed to SSH with any of the following methods
	* on-disk SSH keys
	* password authentication from a L4 jump server
4. Off-site users
	* standard users can use login[1|2] or Citrix with a password
5. Host to Host to Host
	* must use agent forwarding or on-disk keys

<br>
### Linux Clusters/Servers

##### Privileged Access
1. Direct SSH access as a root user will require using a L4 Jump Server or a previously configured SSH key pair.
2. SSH access as a privileged user (those that have the privileged access resource) must originate from a L4 Jump Server, use an SSH key, or use a SmartCard.
3. Only the root user and members of the privaccess group will be allowed to use the su command.
4. Users with appropriate UCAMS resource will be granted membership to the privaccess group with full sudo permissions (no password or TTY requirements) for performing administrative tasks.
5. Manual cleanup will need to be done to remove full privileges from standard users.

##### SSH Access
1. Standard users will be allowed to SSH with any of the following methods
	* Smart Card
	* on-disk SSH keys
	* password authentication from a L4 jump server or login[1|2] or Citrix
	* the username must match UCAMS account names
2. Privileged users will be allowed to SSH with any of the following methods
	* SSH keys (SmartCard derived) from LDAP
	* on-disk SSH keys
	* password authentication from a L4 jump server
	* the username must match UCAMS account name
3. Service accounts will be allowed to SSH with any of the following methods
	* on-disk SSH keys
4. Root and alternate root accounts will be allowed to SSH with any of the following methods
	* on-disk SSH keys
	* password authentication from a L4 jump server
5. Off-site users
	* Standard users can use login[1|2] or Citrix with a password
	* Privileged users need to use VPN and a GFE device that supports SmartCard + SSH
6. Host to Host to Host
	* Must use agent forwarding or on-disk keys



<br />
## Account Configuration
***
* [Privileged Users](./accounts/priv)
* [Standard Users](./accounts/standard)
* [Root Accounts](./accounts/root)
* [Service Accounts](./accounts/service)
* [Extract Your SmartCard SSH Public Key](./accounts/smartcard_ssh_keys)


<br />
## ORNL Supported Operating Systems
***
[Supported Unix/Linux versions at ORNL](https://ornl.service-now.com/its/kb_view_customer.do?sysparm_article=KB0100342)


<br />
## SSH Secure Shell
***
### Configure the SSH client on your ORNL desktop to use a SmartCard
* [Linux](./ssh-client/linux)
* [macOS](./ssh-client/mac)
* [Windows](./ssh-client/win)


<br />
### SSH to an MFA enforced ORNL computer from off-site
* [Off-Site Access](./ssh-client/offsite)


<br />
### SSH Jump Servers with SmartCard login
| Network Zone | Primary Server | Alternate Server | Gaining Access |
| ------------ | -------------- | ---------------- |----------------|
| DMZ | dmzv3login01.ornl.gov | dmzv3login02.ornl.gov | Request Linux access in [DCAMS](https://dcams.ornl.gov) |
| M/EC | meclogin01.ornl.gov | meclogin02.ornl.gov | Request Linux Jump Server -> MEC SSH User in [UCAMS](https://ucams.ornl.gov) |
| OPS | opslogin01.ornl.gov | opslogin02.ornl.gov | Request Linux Jump Server -> OPS SSH User in [UCAMS](https://ucams.ornl.gov) with a secondary userid |
| Open Research | orlogin01.ornl.gov | orlogin02.ornl.gov | Request Linux Jump Server -> Ooen Research SSH User in [UCAMS](https://ucams.ornl.gov) with a dedicated secondary userid |
| Workstations | ranlogin01.ornl.gov | ranlogin02.ornl.gov | Request Linux Jump Server -> RAN SSH User in [UCAMS](https://ucams.ornl.gov) |


<br />
### MFA Phase III Presentations
* [R & D Team MFA Overview - February 13, 2017](./presentations/01-mfa-phase3-overview-RD.pptx)
* [R & D Team MFA SSH Configuration - February 13, 2017](./presentations/02-mfa-phase3-training-RD.pptx)


<br />
### MFA Phase III Proposed SSH Configuration (RHEL 7 example)
* [Desktop](./configs/sshd_config.level4_ssh_mod_desk)
* [Cluster or Server](./configs/sshd_config.level4_ssh_mod_srv)

