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

1. Create the Domain Controller VM (Windows Server 2022) named “DC-1”. Go to virtual machines, name the resource group “AD-Lab", name the virtual machine “DC-1”. Choose a location (and make sure your next VM "Client-1" has the same one), choose window server 2022 and 2 CPUs. Create your Username and Password (My username will be LabUser), save it! Allow selected ports RDP only. Check licensing boxes at the bottom! And click create.

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
3.Create the Client VM (Windows 10) named “Client-1”. Use the same Resource Group and Vnet that was created in Step 1. Include the same region location. Use the same username (LabUser) and password if you want (for this lab, not a good practice for IRL). Click next, and go to the networking tab. In the virtual network make sure it is connected to AD Lab. Then create!
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
 </p>
<br />
 <img src="https://imgur.com/4HXds4o.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
  </p>
<br />
 <img src="https://imgur.com/ynybIMT.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
8. Login to DC-1 and install Active Directory Domain Services. Go to server manager>add roles and features. Hit next, make sure it's selected on “role based installation” then hit next, make sure DC-1 is the selected server then hit next. Then you must select the server role, choose Active Directory Domain server, in the next tab select add feature. On the select server roles window hit next, on select features hit next, on active directory domain service hit next,then click install!
The server has the necessary software installed but it is still not a DC yet!


</p>
<br />
</p>
<br />
<img src="https://imgur.com/U3itFYu.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />
<img src="https://imgur.com/0EbGXnO.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
9. Next you will promote it as DC. After installation there will be an exclamation point in the top left corner. Click on it then click the blue text, “Promote the server to a DC".  On the deployment configuration page select “Add a new forest”, then create a root domain name, it will be mydomain.com, click next on Domain Controller options and create a DSRM(Directory Services Restore Mode) password! Click next, on DNS options, click next and click next on additional notes. Click next on review and click next on prerequisites, click next on install!
</p>
<br />

</p>
<br />
<img src="https://imgur.com/5EusubC.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />
<img src="https://imgur.com/0lIWNs2.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
10.Restart and then log back into DC-1 as user: mydomain.com\username 
To login you must add “MyDomain.com/” to the username so that you login as a DC with your user.

</p>
<br />
<h1>Create an Admin and Normal User Account in AD </h1>
11. In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES”
Within the server menu click tools at the upper right corner>“Active Directory Users and Computers”. On mydomain.com right click>new>Organizational Units, click that. And name it “_Employees”

</p>
<br />
12. Create a new OU named “_ADMINS”
On mydomian.com right click>new>Organizational Units. And name it _ADMINS. Refresh page and both should move up the list.
</p>
<br />
</p>
<br />
<img src="https://imgur.com/s5jUSDP.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
13. Create a new employee named “Jane Doe” with the username of “jane_admin”
Click on the admins folder then on the empty space off to the right, right click>new>user. Name user “Jane Doe”. For the username type “Jane_Admin” click next and create a password. 
</p>
<br />
</p>
<br />
<img src="https://imgur.com/5AHq3tk.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
14. Add jane_admin to the “Domain Admins” Security Group
The user is not an admin yet, in order for that to happen you must add the user to the DA security group. So right click the user>properties>member of. Click add  and within the “Enter Objects” box type in “Domain” then click “Check names” and click “Domain Admins” group. Say ok then apply then ok again!
</p>
<br />
15. Log out/close the Remote Desktop connection to DC-1 and log back in as “mydomain.com\jane_admin”
</p>
<br />
</p>
<br />
<img src="https://imgur.com/fMTpCKA.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
16. Use jane_admin as your admin account from now on
</p>
<br />
17A. From the Azure Portal, set Client-1’s DNS settings to the DC’s Private IP address. This will join Client-1 to your domain mydomain.com. That way you can log onto Client 1 using the accounts you made in DC. Also, Client-1 is connected to the automatic DNS system hosted by VMWare so you must make it connect to our DC’s DNS. If you try to find mydomain.com through VMware it won't be able to find the right one. In client 1 if you type “IPConfig/All” in cmd line right now on the DNS line you’ll see it does not have the I.P address of your DC.
</p>
<br />
17B. B) FInd out what DC-1’s  private IP is. So click on DC under the VM list within Azure Portal and then go to the networking tab. Copy the NIC Private IP address.
</p>
<br />
</p>
<br />
<img src="https://imgur.com/t2GHOsE.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
17C. So go back to Client 1 VM and click on it. Go to networking and then click the blue text next to “Network Interface:” >DNS servers on the left side list. Under DNS servers “Inherit from Virtual Network” will be checked so change it to “custom”. Paste DC’s Private IP address into the “Add DNS server” text box. Click the save button towards the top of the page.
</p>
<br />
18. From the Azure Portal, restart Client-1
You must restart to flush the DNS cache so that it can forget VMWare’s IP address as the DNS and start connecting to DC-1’s DNS. After you restarted, go to system settings, and click on “Rename This PC” then underneath “member of” click on domain and in the text box type in “MyDomain.com”. After this you will be prompted to log in. So login using your admin account. Afterwards the computer will restart again

</p>
<br />
<h1>Set Up Desktop for Non Admins on Client 1</h1>
19.  
</p>
<br />
</p>
<br />
<img src="https://imgur.com/Tcf1nhi.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
20. 
</p>
<br />
</p>
<br />
<img src="https://imgur.com/GPlKJnb.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
21. 
</p>
<br />
22. 
