# SSH to an ORNL Computer From Off-Site
<hr>

SSH access from off-site is challenging in a SmartCard environment. IT has implemented configuration policies to allow off-site access under various scenarios, with some restrictions on privileged access. Use the table below to determine the appropriate access method for your use case.


|Computer Type|Access Method|SSH Program|SSH Authentication Method|Supports Privileged Users|
|-------------|-------------|-----------|-------------------------|-------------------------|
|ORNL managed Windows computer|[DirectAccess](https://ornl.service-now.com/ess/kb_view_customer.do?sysparm_article=KB0010298)|SecureCRT|SmartCard|Yes|
|ORNL managed Linux computer|[VPN ](https://ornl.service-now.com/its/kb_view_customer.do?sysparm_article=KB0010241)|ssh command|SmartCard|Yes|
|ORNL managed Mac computer|[VPN ](https://ornl.service-now.com/its/kb_view_customer.do?sysparm_article=KB0010241)|ssh command|SmartCard|Yes|
|Personally owned computer|[Linux Login Server](https://ornl.service-now.com/its/kb_view_customer.do?sysparm_article=KB0010249#login)|ssh command|Password|No|
|Personally owned computer|[ORNLAccess](https://ornl.service-now.com/its/kb_view_customer.do?sysparm_article=KB0010048)|PuTTY|Password|No|
