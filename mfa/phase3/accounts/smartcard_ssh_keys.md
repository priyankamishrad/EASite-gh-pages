# Extract SSH Public Keys From Your SmartCard
---

In some cases, it may be useful to extract your SSH public key(s) from your SmartCard and add them to an authorized_keys file for a given account. The SmartCard derived SSH keys are retrieved on-demand for UCAMS accounts, but there is no automated lookup mechanism for other account types. Make a note of the appropriate PKCS11 module for your platform and then run the command below to retrieve your key.


<br />
### Path to PKCS11 module for common platforms

|Operating System|Path to PKCS11 module|
|----------------|---------------------|
|macOS / OS X    |/Applications/Charismathics/libcmP11.dylib|
|Fedora          |/usr/lib64/pkcs11/opensc-pkcs11.so|
|CentOS/RHEL 6   |**Not Supported**|
|CentOS/RHEL 7   |/usr/lib64/pkcs11/opensc-pkcs11.so|
|Ubuntu 14.04    |/usr/lib/x86_64-linux-gnu/opensc-pkcs11.so|
|Ubuntu 16.04    |/usr/lib/x86_64-linux-gnu/pkcs11/opensc-pkcs11.so|
|Ubuntu 16.10    |/usr/lib/x86_64-linux-gnu/pkcs11/opensc-pkcs11.so|


<br />
### Extract the keys
```
ssh-keygen -D /Path/To/PKCS11/Module
```
