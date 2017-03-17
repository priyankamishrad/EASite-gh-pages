---
layout: page
title: RDP Authentication With a SmartCard From macOS
category: pages
---

### Client Prerequisites
* HSPD-12 or LSSO (site issued) SmartCard badge.
* You must know your PIN (this can be reset at the badging office).
* [Supported](mfa/smartcard-readers) SmartCard reader for your computer.


<br />
### Launch Citrix Receiver
1. Launch **Applications** -> **Citrix Receiver**
2. The first time you launch Receiver you will be prompted for your e-mail address. Enter your ORNL e-mail address ( e.g. smithja@ornl.gov ) and click Next.
![enter your e-mail address](images/rdp-client-mac-citlaunch.png)
3. Enter your 3 character UCAMS username and password when prompted.
![login to receiver](images/rdp-client-mac-citlogin.png)
4. Click the plus ( + ) sign on the left side of the screen to add application launch icons to the receiver. Applications will also be added to your ~/Applications folder and can be launched from there without first opening receiver and can also be dragged to the Dock.
![add app launchers](images/rdp-client-mac-citaddapp.png)
5. Add a launcher for **ORNL General Desktop** found under **All Applications**.
6. Verify that your SmartCard is in the reader **before** launching the desktop session within Citrix.
7. Launch **ORNL General Desktop**.
![launch ORNL General Desktop](images/rdp-client-mac-citlogd.png)
8. Launch **Remote Desktop Connection** found in Programs -> Accessories.
![launch ORNL General Desktop](images/rdp-client-mac-citrdp.png)
9. Enter the hostname for the jump server you wish to connect to.
![enter jump server address](images/rdp-client-mac-citjumpaddr.png)
10. Enter your SmartCard PIN and jump server username.
![enter jump server credentials](images/rdp-client-mac-citjumpcreds.png)

