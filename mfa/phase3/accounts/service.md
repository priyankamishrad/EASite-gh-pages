# Service Accounts
---

Service accounts are frequently used to run specific applications, automated tasks and automated data transfers. Service accounts that need inbound SSH/SCP/SFTP will need to have an SSH key pair preconfigured to continue working under the new MFA implementation. This document will guide you through the steps of configuring a key pair.

<br />
### Check for an existing key pair
- [ ] Gain administrative privileges on the **target** computer by using sudo or logging in as root
- [ ] su to the service account

```
[root@target-server ~]# su - reporter
```

- [ ] Look for an existing authorized_keys file

```
[reporter@target-server ~]$ ls -l ~/.ssh/authorized_keys
```

The authorized_keys file (if it exists) will list all of the public keys that are allowed to SSH in to that service account. The keys are listed one per line and may end with an optional comment that gives a clue as to their origin. If you have an existing authorized_keys file and data transfer to the service account is working properly, then your work is likely done. If this is not the case, read the next section to configure a new key pair.


<br />
### Create a new key pair on the source server

- [ ] Gain administrative privileges on the **source** computer by using sudo or logging in as root
- [ ] su to the service account

```
[root@source-server ~]# su - data
```

- [ ] Look for an existing key pair

```
[data@source-server ~]$ ls -l ~/.ssh/
```

The key pair will be stored as two files: ( id_dsa and id_dsa.pub ) or ( id_rsa and id_rsa.pub ). If these files exist, copy the contents of the .pub file and skip to the next section.

- [ ] Create a new key pair

**Note:** I used an empty passphrase for the example below. From a security standpoint, it is better to set a passphrase on the key as it helps prevent unauthorized use. However, it is often very difficult or even impossible to configure automated file copies when using password protection key pairs, which is why they have been omitted here. Your intended usage should dictate your choice.

```
[data@source-server ~]$ ssh-keygen -t rsa -b 2048
Generating public/private rsa key pair.
Enter file in which to save the key (/home/data/.ssh/id_rsa):
Created directory '/home/data/.ssh'.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/data/.ssh/id_rsa.
Your public key has been saved in /home/data/.ssh/id_rsa.pub.
```

- [ ] Copy the contents of ~/.ssh/id_rsa.pub and proceed to the next section


<br />
### Configure authorized_keys on the target server

- [ ] Gain administrative privileges on the **target** computer by using sudo or logging in as root
- [ ] su to the service account

```
[root@target-server ~]# su - reporter
```

- [ ] Create the authorized_keys file

```
[reporter@target-server ~]$ mkdir -p ~/.ssh
[reporter@target-server ~]$ chmod 700 ~/.ssh
[reporter@target-server ~]$ touch ~/.ssh/authorized_keys
[reporter@target-server ~]$ chmod 600 ~/.ssh/authorized_keys
```

- [ ] Append the contents of your .pub key file into ~/.ssh/authorized_keys (note that it should be added as a single line). An example of this process (which assumes that you have copied id_rsa.pub from the source server to /tmp on the target server) is documented below.

```
[reporter@target-server ~]$ cat /tmp/id_rsa.pub >> ~/.ssh/authorized_keys
```

- [ ] Test SSH from the source user/server to the target user/server. You should not be prompted for a password.

```
[data@source-server ~]$ ssh reporter@target-server
```

