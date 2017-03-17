---
layout: page
title: SmartCard Login for SSH on RHEL 6
category: pages
---

* Prerequisites
	* All users of the system will need the Directory Server -> LDAP User resource in [UCAMS](https://ucams.ornl.gov)


* Install packages
```
yum install openssh-ldap openldap-clients
```


* Tweak /etc/openldap/ldap.conf
```
    BASE	dc=ornl,dc=gov
    URI		ldaps://ldapu.ornl.gov

    #SIZELIMIT	12
    #TIMELIMIT	15
    #DEREF	never

    TLS_CACERTDIR	/etc/openldap/cacerts
```


* Test ldapsearch
```
ldapsearch -x
```


* Create directory for override SSH keys
```
    mkdir /etc/ssh/keys
    chown root:root /etc/ssh/keys
    chmod 755 /etc/ssh/keys

    for I in `ls /coreit/`; do
    mkdir /etc/ssh/keys/$I;
    chmod 700 /etc/ssh/keys/$I;
    \cp /coreit/$I/.ssh/authorized_keys /etc/ssh/keys/$I/;
    chown -R $I:$I /etc/ssh/keys/$I/;
    done
```


* Configure LDAP lookup
```
cp /usr/share/doc/openssh-ldap-5.3p1/ldap.conf /etc/ssh/
```

* Edit /etc/ssh/ldap.conf
```
	uri ldaps://ldapu.ornl.gov
	base ou=users,dc=ornl,dc=gov	
	ldap_version 3
	tls_checkpeer hard
	tls_cacertdir /etc/openldap/cacerts	
```


* Test the lookup
```
/usr/libexec/openssh/ssh-ldap-wrapper <username>
```


* Configure /etc/ssh/sshd_config  
**NOTE:** Search for these options in the configuration file and comment out existing entries
```
    # LDAP lookup for SmartCard derived SSH keys
AuthorizedKeysCommand /usr/libexec/openssh/ssh-ldap-wrapper
AuthorizedKeysCommandRunAs nobody
PubkeyAuthentication yes
AuthorizedKeysFile /etc/ssh/keys/%u/authorized_keys

    # Require the use of SSH keys to login
ChallengeResponseAuthentication no
GSSAPIAuthentication no
KerberosAuthentication no
PasswordAuthentication no
PermitRootLogin no

    # Allow CoreIT from skynet1 only
    Match User it7,itk,itz Address 160.91.1.221
    	PubkeyAuthentication yes

    # Allow ServiceNow and Tenable
    Match User lan,z2z
    	PubkeyAuthentication yes
```


* Test the SSHD configuration and restart the service.
```
sshd -t -f /etc/ssh/sshd_config && /etc/init.d/sshd restart
```


* Other tasks
	* Configure SecurID auth for sudo and su
	* On systems where alternate root login accounts exist, you may need to set PermitRootLogin to yes
	* [Configure](../home.html) your SSH client to use SmartCard auth when connecting to this server.

