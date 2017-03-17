# Root Accounts
---

Direct SSH access as the root user is greatly discouraged and should primarily be used to perform emergency administration tasks. To SSH to the root account, you must first SSH to a Workstations PZ jump server using your SmartCard and then SSH to the root account on the target computer.

In rare cases, there may be a need to perform root to root SSH or SCP between computers. In these instances, you may configure an SSH key pair for the root user. If at all possible, this key pair should have a strong passphrase set and the authorized_keys file should restrict which hosts the connection can originate from.

<br />
### Example of an authorized_keys file which restricts incoming hosts

*/root/.ssh/authorized_keys* (mode 600)
```
from="stig.ornl.gov,160.91.110.248" ssh-rsa AAAAB3NzaC1yc3EAAAABIjAAAP8393lUQUGlchTGRlaGwIT0/PK9n831LvuLBOjfzSTt6kfWYNcvLgOMN9nIpo1kUTrK2TKuYCSHCm0luLSu8jgTHRMKZqWwBUqY0chRhmU/7lIaomoTp311D9/J23vrFok/84yjt3kz/GESPDBVboXuZsq3ljizRAeHQIBcvAZO93J4zu3o7cCST204bITIMTlhxOfacJ1p6G2h5yxl5kPq1CPTRo0KhU1rLkVq4sRYfUBf81jg8W1I3jD1Om37kGJ0rQks+0dteFFgNW10++iGomyUmO1YPDQnBfuC/eGXftsj1PBbK4s6fb4jXOP7FI30LRfP9jIx9j3aD9EtDbE= user@remote-host
```
