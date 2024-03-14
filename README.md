<p align="center">
<img src="https://imgur.com/iQIP14f.png" alt="osTicket logo"/>
</p>

<h1>Active Directory Installation</h1>
This tutorial outlines the installation of Active Directory on a windows server.<br />


<h2>Environments and Technologies Used</h2>

-Microsoft Azure
- Windows 10 Computer
- Windows 2022 Server
- Remote Desktop
- Active Directory 

<h2>Operating Systems Used </h2>
-Windows 2022 Server
- Windows 10</b> (21H2)


<h2>Create Resources</h2>

<p>
<img src="https://imgur.com/D3xi2vk.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

1. Create the Domain Controller VM (Windows Server 2022) named “DC-1”. Go to virtual machines, name the resource group “AD-Lab", name the virtual machine “DC-1”. Choose a location (and make sure your next VM "Client-1" has the same one), choose window server 2022 and 2 CPUs. Create your Username and Password, save it! Allow selected ports RDP only. Check licensing boxes at the bottom! And click create.

</p>
<br />

</p>
<br />

</p>
<p>

</p>
<br />

<p>
<img src="https://imgur.com/R2Nwn9Z.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
 <img src="https://imgur.com/C6Z85az.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

2. Set Domain Controller’s NIC Private IP address to be static 
Go to the networking tab>blue # next to the NIC interface>IP configurations>IP address> Under assignment change the dynamic to static! Then click save on the upper left.
The DC must have a static IP so that it doesn’t change, if it did change (after a reboot for example) any computers connected to the Domain will experience trouble since they would be trying to connect to the DC’s prior IP!


</p>
<br />

</p>
<br />

</p>
<br />
 <img src="https://imgur.com/ZJL87cs.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
3.Create the Client VM (Windows 10) named “Client-1”. Use the same Resource Group and Vnet that was created in Step 1. Include the same region location. Use the same username and pw if you want (for this lab, not a good practice for IRL). Click next, and go to the networking tab. In the virtual network make sure it is connected to AD Lab. Then create!
</p>
<br />
4. Click on ea VM, then check under VirtualNetwork/Subnet to see if connected to AD-lab
</p>
<br />
5. Ensure Connectivity between the client and Domain Controller
Login to Client-1 with Remote Desktop, open command line and ping DC-1’s private IP address with ping -t <ip address> (perpetual ping). The ping should fail cuz the firewall on DC is blocking traffic!
</p>
<br />
 </p>
<br />
</p>
<br />
 <img src="https://imgur.com/nTbgxtT.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
 </p>
<br />
 <img src="https://imgur.com/fV0Hguk.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
 
6. Login to the Domain Controller and enable ICMPv4 in on the local windows Firewall, which is the protocol Ping uses. So RDP to DC’s public IP address, login with the username and password you made. Click start menu>server manager. In search bar at the bottom type firewall and click on windows defendant firewal click on inbound rules> protocol. Scroll down till you find ICMPv4, right click to enable all of the ones w that protocol

</p>
<br />
 </p>
<br />
 <img src="https://imgur.com/4REr85C.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
7. Check back at Client-1 to see the ping succeed. It was continuously pinging the whole time so the first part that says time out was before we enabled ICMPv4 and you can see that Client-1 has started to get a reply as soon as we changed DC-1 settings!
Type in cntrl c to make it stop. Now that we are done setting up the resources necassary for Active Directory now we can start installing it!
</p>
<br />
<h1>Active Directory Installation</h1>
</p>
<br />
8. Next install PHP (php-7.3.8-nts-Win32-VC15-x86.zip) https://drive.google.com/file/d/1snNMtLdCOpMtkCyD4mvl9yOOmvVIp9fP/view?usp=share_link

</p>
<br />
</p>
<br />
<img src="https://imgur.com/yA0dnbM.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
9. Create a new file named "PHP" for PHP on the local harddrive., unzip the PHP contents into C:/PHP
</p>
<br />
10. Next download Microsoft Visual C + + (VC_redist.x86.exe.) 
https://drive.google.com/file/d/1s1OsGF3-ioO0_9LYizPRiVuIkb3lFJgH/view?usp=share_link
</p>
<br />
11.  Next Install a database ((mysql-5.5.62-win32.msi) https://drive.google.com/file/d/1_OWh9p7VQLcrB0q_V7qT8yHl0xo5gv7z/view?usp=share_link
Select the typical install, click finish to launch the configuariation wizard. Within that check standard configuaration and install.
Create your credentials for the database, your username will automatically be “root”. So create your password.  Do not forget it! Then execute.
</p>
<br />
12. Next search IIS on the windows start menu, right click and open as admin.
</p>
<br />
</p>
<br />
<img src="https://imgur.com/s5jUSDP.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
13. Open the PHP manager that was installed earlier. Click register and it will ask for a location, click the three dots. Locate file C:\PHP\CGI and select it.
</p>
<br />
</p>
<br />
<img src="https://imgur.com/5AHq3tk.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
14. Refresh IIS AFTER selecting the file.
 Then download OSTicket (osTicket v 1.12.8) onto the machine. 
Open the OSTicket file, extract and copy the zipped file "Upload" within it. Copy it into c:\inetpub\wwwroot
</p>
<br />
15. Within c:\inetpub\wwwroot rename the upload file to "OSTicket"
</p>
<br />
</p>
<br />
<img src="https://imgur.com/fMTpCKA.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
16. Reload IIS again and in the upper left corner go click on Ticket\YourName> Sites> Default> osTicket. On the right side of the screen click “Browse *:80” and the welcome webpage to IIS should load. It says “Thanks for choosing osTicket!”
</p>
<br />
17. On that same page, Ticket\YourName> Sites> Default> osTicket.  Double-click PHP Manager within OSticket. Click “Enable or disable an extension”
Enable: php_imap.dll
Enable: php_intl.dll
Enable: php_opcache.dll
Then Refresh osTicket Browser
</p>
<br />
18. Rename ost-sampleconfig.php to ost-config.php
It is located in
C:\inetpub\wwwroot\osTicket\include\ost-sampleconfig.php
</p>
<br />
</p>
<br />
<img src="https://imgur.com/t2GHOsE.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
19. Next you will assign permissions to ost-config.php.  Right click to properties and go to security. Click advanced then disable inheritance, click remove all permissions. Next click add, click select principle and type everyone and click ok. Check box “full control” and apply.
</p>
<br />
20. Instal HeidiSQL so that you can connect to MySQL. https://docs.google.com/document/d/1WovrX2DaS9xkfaSr4LXyB4YnnWpXIgPCMMbbfgHmGVw/edit
Open the file and install it. The app shoud open after install. On the bottom left click new, then enter username:Root then password:
Create a new session, enter username and passowrd.
</p>
<br />
21. Create a new session and connect to it by clicking open.  
</p>
<br />
</p>
<br />
<img src="https://imgur.com/Tcf1nhi.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
22. In Heidi right click unnamed in upper left corner, then click create new then database.then name it osTIcket then say ok
</p>
<br />
</p>
<br />
<img src="https://imgur.com/GPlKJnb.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
23. Continue Setting up osTicket in the ISS webpage, click next. Next fill out the system settings and in database settings enter your MY SQL 
MySQL Table Prefix: ost-config.php
MySQL Database: osTicket
MySQL Username: root
MySQL Password: ***** whatever you created
Click “Install Now!”
</p>
<br />
24. OS TIcket should then load up with a welcome screen. Congratulations you just installed os Ticket! c: 
