# Standard Users
---

Standard users must SSH with a [SmartCard](../home#ssh-secure-shell) when connecting to SmartCard enforced computers. The exception to this policy is when the connection originates from login1/login2, ranlogin01/ranlogin02 or Citrix; in these cases you may SSH to the target computer with a password. In order for this authentication to work correctly, the target computer must be correctly configured on the network and your remote username must match your 3-character UCAMS userid. Listing your UCAMS username in brackets in the GECOS field is not sufficient for making this work. The example below shows how the root user can change a username.


<br />
### Example to change a username from 'elise' to '2zz'

```
[root@host ~]# usermod elise -l 2zz
```

**Notes:**
- The home directory name and path will not be changed with this command. If necessary, these can be changed manually.
- Verify that mail spools, crontabs, etc. still work after changing a username.


<br />
### Recommended way to add a new UCAMS user

The **wsadduser** utility should be present on most ORNL Linux machines, or it can be installed via apt-get or yum (depending on your distribution). The utility will automatically fill in the user details and setup the initial environment.

**Usage:** wsadduser *3-character-username*
```
[root@topgear ~]# wsadduser stg
Loading defaults.

Adding user "stg".
Checking username.

What is user stg's primary group? (name or number) [users]

List any other groups "stg" should belong to.  Use names
or numbers, and separate them with spaces:

Getting user information from network database.

User information: Ben Collins, 865-555-5512, 5900 MS-6121
Is this information OK? (y/n) [y] y

What directory do user directories go in? [/home]

Login shell: [/bin/bash]

Account stg : Password updated
User "stg" added to /etc/passwd.

Next username or RETURN to exit:
```

