---
layout: page
title: SSH Authentication with a SmartCard from macOS
category: pages
---

### Client Prerequisites
* HSPD-12 or LSSO (site issued) SmartCard badge. **The eToken cannot be used for SSH.**
* You must know your PIN (this can be reset at the badging office).
* [Supported](../smartcard-readers.html) SmartCard reader for your desktop/laptop.
* You must request access to the SSH jump servers before using them. See [this](../home.html) page for the server list.

<br />
### Client Setup
1. Computers that are doing SmartCard enforcement should already have the required software installed.
2. Insert your SmartCard and verify that it can be seen and unlocked with your PIN in Applications -> Utilities -> Keychain Access.
3. Add the following to ~/.ssh/config

```
# SmartCard
Host dmzv3login* meclogin* opslogin* orlogin* ranlogin*
   PKCS11Provider /Applications/Charismathics/libcmP11.dylib
```

<br />
### Use the jump servers as a proxy
In many instances it can be convenient to use the jump servers as a proxy when accessing your hosts in the DMZ and OPS. This simplifies the process of using SSH, SCP and SFTP to hosts in the other protection zones. You will be prompted for your credentials twice: the first prompt will be for your SmartCard PIN and the second prompt will be for your credentials on the destination server.

* Append the lines below to ~/.ssh/config
	* The first section is for access to the jump servers and may already be present in your config.
	* Replace the user names in <> with your PZ specific user name.
	* Add the correct destination server names to the **Host** lines. It is recommended that you type the short name followed by an asterisk.

```
# SmartCard
Host dmzv3login* meclogin* opslogin* orlogin* ranlogin*
   PKCS11Provider /Applications/Charismathics/libcmP11.dylib

# SmartCard jump server proxy - DMZ
Host dmz-server*
   ProxyCommand ssh -I /Applications/Charismathics/libcmP11.dylib <your-dmz-username>@dmzv3login02.ornl.gov nc %h %p

# SmartCard jump server proxy - OPS
Host ops-server*
   ProxyCommand ssh -I /Applications/Charismathics/libcmP11.dylib <your-ops-username>@opslogin02.ornl.gov nc %h %p
```

