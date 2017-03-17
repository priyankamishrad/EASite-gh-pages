# SSH Authentication With A SmartCard From Linux

## Client Prerequisites
* HSPD-12 or LSSO (site issued) SmartCard badge. **The eToken cannot be used for SSH.**
* You must know your PIN, or visit the badging office to have it reset.
* [Supported](../smartcard-readers) SmartCard reader for your desktop/laptop.
* If you need to use an SSH jump server, you must first request access. See [this](https://portal04.ornl.gov/sites/its/ornllogin/Wiki%20Pages1/JumpServersLookup.aspx) page for the server list.


<br />
## Before You Begin
* Consult the table below, install the appropriate packages on your Linux desktop and make a note of the path to the PKCS11 module.

|Operating System|Packages to install|Path to PKCS11 module|
|----------------|-------------------|---------------------|
|Fedora          |dnf install opensc |/usr/lib64/pkcs11/opensc-pkcs11.so|
|CentOS/RHEL 6   |**Not Supported**  |**Not Supported**    |
|CentOS/RHEL 7   |yum install opensc|/usr/lib64/pkcs11/opensc-pkcs11.so|
|Ubuntu 14.04    |sudo apt-get install opensc|/usr/lib/x86_64-linux-gnu/opensc-pkcs11.so|
|Ubuntu 16.04    |sudo apt-get install opensc|/usr/lib/x86_64-linux-gnu/pkcs11/opensc-pkcs11.so|
|Ubuntu 16.10    |sudo apt-get install opensc|/usr/lib/x86_64-linux-gnu/pkcs11/opensc-pkcs11.so|


* Insert your SmartCard and verify that it can be seen by running ```opensc-tool -l```

* You need to decide whether to use the SSH Agent or the PKCS11 module to SSH to Linux computers that require SmartCard authentication. The PKCS11 module is the simple and most reliable approach, but has some inherent limitations. Use the table below to help decide if you need to use the SSH Agent instead of the PKCS11 module. Generally speaking, you should use only one of these methods and not try to combine the two.

|Use Case|Example|Recommended Method|
|--------|-------|------------------|
|You require the ability to SSH or SCP from server to server.|You SSH from your desktop to server1, and from server1 you need to SSH/SCP to server2.|SSH Agent|
|You frequently connect to multiple Linux computers in a short period of time.|You are the system administrator for a large group of computers.|SSH Agent|


<br />
## SSH Using the Agent
<hr>

### SSH Configuration
1. Add the following to ~/.ssh/config:
	* Replace *host1\* host2\** with a space separated list of SmartCard enforced computers that you connect to.
	* If the **Host** line becomes too long or you wish to organize your entries, you can replicate the entire section as many times as you need.
	* You will need to add an entry for every host that you connect to using a SmartCard.
	* If you have existing entries in the configuration file, manually merge them in. Order is important.


```
# SmartCard authentication to general hosts
Host host1* host2*
   ForwardAgent yes
   IdentitiesOnly no
   PasswordAuthentication no

# SmartCard authentication to jump servers
Host dmzv3login* meclogin* opslogin* orlogin* ranlogin*
   IdentitiesOnly no
   PasswordAuthentication no

# default setting for all hosts
Host *
   ForwardAgent no
   ForwardX11 yes
   ForwardX11Trusted yes
   GSSAPIAuthentication no
   IdentitiesOnly yes
   Protocol 2
```

<br />
### Invoking the Agent

The SSH agent is very sensitive to SmartCard removal, which will cause the agent and all existing SSH sessions to lose access to the card. This means that when you remove your badge from the reader (for example because you need to step away from your desk), the SSH agent and all existing SSH sessions will forget your SmartCard keys. If you remove your badge, your existing SSH sessions will remain active, but you will not be able to SSH/SCP to a second server from them. In addition, you will not be able to establish any new SSH connections from your desktop. **The solution to both problems is to re-register your card with the agent and recreate your SSH sessions.**

1. Insert your SmartCard into the reader.
2. Register your card with the SSH agent: ```ssh-add -s /path/to/opensc-pkcs11.so```
	* Complete step 4 if you encounter an error when running this command.
3. SSH as you normally would (make sure that you have entries for the hosts that require a SmartCard in your ~/.ssh/config file).
4. Unregister your card **before** removing it from the reader: ```ssh-add -e /path/to/opensc-pkcs11.so```


<br />
### Use the jump servers as a proxy
In many instances it can be convenient to use the jump servers as a proxy when accessing your hosts in the DMZ and OPS. This simplifies the process of using SSH, SCP and SFTP to hosts in the other protection zones. You will need to register your SmartCard with the SSH Agent (see the above section) and when you subsequently connect to a computer behind the jump server, you will only need to enter your credentials for the destination computer.

* Add the lines below to the top of ~/.ssh/config
	* The first section is for access to the jump servers and may already be present in your configuration file.
	* Add the correct destination server names to the **Host** lines. It is recommended that you type the short name followed by an asterisk.
	* Specify the correct path to the opensc-pkcs11.so library on the ProxyCommand line (see OS table above).
	* Replace the user names in <> with your Protection Zone specific user name.

```
# SmartCard authentication to jump servers
Host dmzv3login* meclogin* opslogin* orlogin* ranlogin*
   IdentitiesOnly no
   PasswordAuthentication no

# SmartCard jump server proxy - DMZ
Host dmz-server*
   ProxyCommand ssh <your-dmz-username>@dmzv3login02.ornl.gov nc %h %p
   ForwardAgent yes

# SmartCard jump server proxy - OPS
Host ops-server*
   ProxyCommand ssh <your-ops-username>@opslogin02.ornl.gov nc %h %p
   ForwardAgent yes
```

<br />
## SSH Using the PKCS11 Module
<hr>

### SSH Configuration
1. Add the following to the top of ~/.ssh/config:
	* Replace *host1\* host2\** with a space separated list of SmartCard enforced computers that you connect to.
	* If the **Host** line becomes too long or you wish to organize your entries, you can replicate the entire section as many times as you need.
	* Specify the correct path to the opensc-pkcs11.so library on the ProxyCommand line (see OS table above).
	* You will need to add an entry for every host that you connect to using a SmartCard.

```
# SmartCard authentication to general hosts
Host host1* host2*
   PKCS11Provider /path/to/opensc-pkcs11.so

# SmartCard authentication to jump servers
Host dmzv3login* meclogin* opslogin* orlogin* ranlogin*
   PKCS11Provider /path/to/opensc-pkcs11.so
```

<br />
### Use the jump servers as a proxy
In many instances it can be convenient to use the jump servers as a proxy when accessing your hosts in the DMZ and OPS. This simplifies the process of using SSH, SCP and SFTP to hosts in the other protection zones. You will be prompted for your credentials twice: the first prompt will be for your SmartCard PIN and the second prompt will be for your credentials on the destination server.

* Append the lines below to ~/.ssh/config
	* The first section is for access to the jump servers and may already be present in your configuration file.
	* Specify the correct path to the opensc-pkcs11.so library on the ProxyCommand line (see OS table above).
	* Replace the user names in <> with your Protection Zone specific user name.
	* Add the correct destination server names to the **Host** lines. It is recommended that you type the short name followed by an asterisk.

```
# SmartCard
Host dmzv3login* meclogin* opslogin* orlogin* ranlogin*
   PKCS11Provider /path/to/opensc-pkcs11.so

# SmartCard jump server proxy - DMZ
Host dmz-server*
   ProxyCommand ssh -I /path/to/opensc-pkcs11.so <your-dmz-username>@dmzv3login02.ornl.gov nc %h %p

# SmartCard jump server proxy - OPS
Host ops-server*
   ProxyCommand ssh -I /path/to/opensc-pkcs11.so <your-ops-username>@opslogin02.ornl.gov nc %h %p
```

