---
layout: page
title: SSH Authentication with a SmartCard from Linux
category: pages
---

### Client Prerequisites
* HSPD-12 or LSSO (site issued) SmartCard badge. **The eToken cannot be used for SSH.**
* You must know your PIN (this can be reset at the badging office).
* [Supported](../smartcard-readers.html) SmartCard reader for your desktop/laptop.
* You must request access to the SSH jump servers before using them. See [this](https://portal04.ornl.gov/sites/its/ornllogin/Wiki%20Pages1/JumpServersLookup.aspx) page for the server list.

<br />
### Fedora

1. Install OpenSC: dnf install opensc
2. Verify that your SmartCard reader can be seen: opensc-tool -l
3. Add the following to ~/.ssh/config

```
# SmartCard
Host dmzv3login* meclogin* opslogin* orlogin* ranlogin*
	PKCS11Provider /usr/lib64/pkcs11/opensc-pkcs11.so
```

<br />
### RHEL 6

*Not working as of 7/29/2016. The recommendation is to transition to RHEL 7.*


<br />
### RHEL 7

1. Install OpenSC: yum install opensc
2. Verify that your SmartCard reader can be seen: opensc-tool -l
3. Add the following to ~/.ssh/config

```
# SmartCard
Host dmzv3login* meclogin* opslogin* orlogin* ranlogin*
	PKCS11Provider /usr/lib64/pkcs11/opensc-pkcs11.so
```

<br />
### Ubuntu 14.04

1. Install OpenSC: sudo apt-get install opensc
2. Verify that your SmartCard reader can be seen: opensc-tool -l
3. Add the following to ~/.ssh/config

```
# SmartCard
Host dmzv3login* meclogin* opslogin* orlogin* ranlogin*
	PKCS11Provider /usr/lib/x86_64-linux-gnu/opensc-pkcs11.so
```

<br />
### Ubuntu 16.04

1. Install OpenSC: sudo apt-get install opensc
2. Verify that your SmartCard reader can be seen: opensc-tool -l
3. Add the following to ~/.ssh/config

```
# SmartCard
Host dmzv3login* meclogin* opslogin* orlogin* ranlogin*
	PKCS11Provider /usr/lib/x86_64-linux-gnu/pkcs11/opensc-pkcs11.so
```

<br />
### Ubuntu 16.10

1. Install OpenSC: sudo apt-get install opensc
2. Verify that your SmartCard reader can be seen: opensc-tool -l
3. Add the following to ~/.ssh/config

```
# SmartCard
Host dmzv3login* meclogin* opslogin* orlogin* ranlogin*
	PKCS11Provider /usr/lib/x86_64-linux-gnu/pkcs11/opensc-pkcs11.so
```

<br />
### Use the jump servers as a proxy
In many instances it can be convenient to use the jump servers as a proxy when accessing your hosts in the DMZ and OPS. This simplifies the process of using SSH, SCP and SFTP to hosts in the other protection zones. You will be prompted for your credentials twice: the first prompt will be for your SmartCard PIN and the second prompt will be for your credentials on the destination server.

* Append the lines below to ~/.ssh/config
	* Substitute the correct path to the opensc-pkcs11.so library on the ProxyCommand line (see above).
	* Replace the user names in <> with your PZ specific user name.
	* Add the correct destination server names to the **Host** lines. It is recommended that you type the short name followed by an asterisk.

```
# SmartCard jump server proxy - DMZ
Host dmz-server*
   ProxyCommand ssh -I /path/to/opensc-pkcs11.so <your-dmz-username>@dmzv3login02.ornl.gov nc %h %p

# SmartCard jump server proxy - OPS
Host ops-server*
   ProxyCommand ssh -I /path/to/opensc-pkcs11.so <your-ops-username>@opslogin02.ornl.gov nc %h %p
```

