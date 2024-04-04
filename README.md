<p align="center">
<img src="https://imgur.com/iQIP14f.png" alt="osTicket logo"/>
</p>

<h1>Active Directory Installation, Configuration and Troubleshooting</h1>
This tutorial outlines the installation, configuration and troublehsooting of Active Directory on a windows server.<br />


<h2>Environments and Technologies Used</h2>

-Microsoft Azure
- Windows 10 Computer
- Windows 2022 Server
- Remote Desktop
- Active Directory 

<h2>Operating Systems Used </h2>

- Windows 2022 Server
- Windows 10</b> (21H2)


<h2>Create Resources</h2>

<p>
<img src="https://imgur.com/D3xi2vk.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
 
1. Create the Domain Controller VM (Windows Server 2022) named “DC-1”. Go to virtual machines, name the resource group “AD-Lab", name the virtual machine “DC-1”. Choose a location (and make sure your next VM, "Client-1" has the same one). Choose window server 2022 and 2 CPUs. Create your Username and Password (My username will be LabUser), save it! Allow selected ports RDP only. Check licensing boxes at the bottom! And click create.
</p>
<br />
</p>
<br />
<p>
<img src="https://imgur.com/R2Nwn9Z.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
 
2a. Set Domain Controller’s NIC Private IP address to be static.
Go to the networking tab>blue # next to the NIC interface>IP configurations>IP address
</p>
<br />
</p>
<br />
<p>
 <img src="https://imgur.com/C6Z85az.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
 
 2b. Under assignment change the dynamic to static! Then click save on the upper left.
The DC must have a static IP so that it doesn’t change, if it did change (after a reboot for example) any computers connected to the Domain will experience trouble since they would be trying to connect to the DC’s prior IP!
</p>
<br />
</p>
<br />
<p>
 <img src="https://imgur.com/ZJL87cs.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
3.Create the Client VM (Windows 10) named “Client-1”. Use the same Resource Group and Vnet that was created in Step 1. Include the same region location. Use the same username (LabUser) and password if you want (for this lab, not a good practice for IRL). Click next, and go to the networking tab. In the virtual network make sure it is connected to AD Lab. Then create!
</p>
<br />
4. Click on each VM, then check under VirtualNetwork/Subnet to see if they're connected to AD-lab
</p>
<br />
5. Ensure Connectivity between the Client and Domain Controller
Login to Client-1 with Remote Desktop, open command line and ping DC-1’s private IP address with ping -t <ip address> (perpetual ping). The ping should fail because the firewall on the DC is blocking traffic!
</p>
<br />
</p>
<br />
<p>
 <img src="https://imgur.com/nTbgxtT.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
 
6a. Login to the Domain Controller and enable ICMPv4 on the local windows Firewall, which is the protocol Ping uses. So RDP to DC’s public IP address, login with the username and password you made. Click start menu>server manager. In search bar at the bottom type firewall and click on windows defendant firewall.
</p>
<br />
</p>
<br />
<p>
 <img src="https://imgur.com/fV0Hguk.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
 
 6b. Next click on inbound rules> protocol. Scroll down until you find ICMPv4, right click to enable all of the ones with that protocol
</p>
<br />
</p>
<br />
<p>
 <img src="https://imgur.com/4REr85C.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
 
7. Check back at Client-1 to see the ping succeed. It was continuously pinging the whole time so the first part that says "time out" was before we enabled ICMPv4 and you can see that Client-1 has started to get a reply as soon as we changed DC-1's settings!
Type in ctrl+c to make it stop. Now that we are done setting up the resources necassary for Active Directory, we can start installing the Active Directory software onto the Domain Controller.
</p>
<br />
<h1>Active Directory Installation</h1>
</p>
<br />
 </p>
<br />
 <img src="https://imgur.com/4HXds4o.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
 
8a. Login to DC-1 and go to server manager>add roles and features. Hit next, make sure it's selected on “role based installation” then hit next, make sure DC-1 is the selected server then hit next. 
</p>
<br />
 </p>
<br />
</p>
<br />
 <img src="https://imgur.com/ynybIMT.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
8b.Then you must select the server role, choose Active Directory Domain server, in the next tab select add feature. On the select server roles window hit next, on select features hit next, on active directory domain service hit next,then click install!
The server has the necessary software installed but it is still not a complete DC!
</p>
<br />
 </p>
<br />
</p>
<br />
<img src="https://imgur.com/U3itFYu.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

9a. Next you will promote it as DC. After installation there will be an exclamation point in the top left corner. Click on it then click the blue text, “Promote the server to a DC".
</p>
<br />
 </p>
<br />
</p>
<br />
<img src="https://imgur.com/0EbGXnO.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
9b. On the deployment configuration page select “Add a new forest”, then create a root domain name, it will be mydomain.com, click next on Domain Controller options and create a DSRM(Directory Services Restore Mode) password! Click next, on DNS options click next and click next on additional notes. Click next on review and click next on prerequisites, click next on install.
(The forest is highest organizational structure within AD. The forest is made up of domain trees, which are made up of domains, which are made up of objects. Anything within this forest has similar configurations and will inherently trust each other.)
</p>
<br />
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
</p>
<br />
<img src="https://imgur.com/x6TEu5r.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
11a. In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES”
Within the server menu click tools at the upper right corner>“Active Directory Users and Computers”.
</p>
<br />
 </p>
<br />
</p>
<br />
<img src="https://imgur.com/BUCTrpw.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
11b. Within ADUC clink on mydomain.com right click>new> click Organizational Units, and name it “_Employees”
</p>
<br />
 </p>
<br />
</p>
<br />
<img src="https://imgur.com/CKRc6hW.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
12. Create a new OU named “_ADMINS”
On mydomain.com right click>new>Organizational Units. And name it _ADMINS. Refresh page and both should move up the list.
</p>
<br />
 </p>
<br />
</p>
<br />
<img src="https://imgur.com/mMvnGd0.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
13. Create a new employee named “Jane Doe” with the username of “jane_admin”
Click on the admins folder then on the empty space off to the right, right click>new>user. Name user “Jane Doe”. For the username type “Jane_Admin” click next and create a password. 
</p>
<br />
 </p>
<br />
</p>
<br />
<img src="https://imgur.com/lkeWVhK.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
14a. Add jane_admin to the “Domain Admins” Security Group.
The user is not an admin yet, in order for that to happen you must add the user to the Domain Admins security group. So within the Admins folder, right click Jane Doe's username and go to properties
</p>
<br />
 </p>
<br />
</p>
<br />
<img src="https://imgur.com/Os02gwP.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
14b. Go to the "member of" tab. Click add.
</p>
<br />
 </p>
<br />
</p>
<br />
<img src="https://imgur.com/7CtBJjH.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
14c. Within the “Enter Objects” box type in “Domain” then click “Check names” and click “Domain Admins” group. Say ok then apply then ok again!
</p>
<br />
 </p>
<br />
</p>
<br />
<img src="https://imgur.com/vutYJCj.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
15. Log out/close the Remote Desktop connection to DC-1 and log back in as “mydomain.com\jane_admin”
</p>
<br />
16. Use jane_admin as your admin account from now on
</p>
<br />
<h1>Connecting Client-1 to DC </h1>
</p>
<br />
<img src="https://imgur.com/NhISrFO.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
17a. From the Azure Portal, set Client-1’s DNS settings to the DC’s Private IP address. This will join Client-1 to your domain mydomain.com. That way you can log onto Client 1 using the accounts you made in the DC. Also, Client-1 is connected to the automatic DNS system hosted by VMWare so you must make it connect to our DC’s DNS. If you try to find mydomain.com through VMware it won't be able to find the right one. 
</p>
<br />
</p>
<br />
</p>
<br />
<img src="https://imgur.com/hCh7iCO.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
17b. In client 1 if you type “IPConfig/All” in cmd line right now on the DNS line you’ll see it does not have the I.P address of your DC. The IP address we want to connect to is 10.2.0.4
</p>
<br />
</p>
<br />
</p>
<br />
<img src="https://imgur.com/8EHphUk.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
17c. FInd out what DC-1’s  private IP is. So click on DC under the VM list within Azure Portal and then go to the networking tab. Copy the NIC Private IP address.
</p>
<br />
</p>
<br />
</p>
<br />
<img src="https://imgur.com/ledXS6O.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
17d. So go back to Client 1 VM and click on it. Go to networking and then click the blue text next to “Network Interface:” >DNS servers on the left side list. Under DNS servers “Inherit from Virtual Network” will be checked so change it to “custom”. Paste DC’s Private IP address into the “Add DNS server” text box. Click the save button towards the top of the page.
</p>
<br />
</p>
<br />
</p>
<br />
<img src="https://imgur.com/iUomZ6a.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
18. From the Azure Portal, restart Client-1
You must restart to flush the DNS cache so that it can forget VMWare’s IP address as the DNS and start connecting to DC-1’s DNS. After you restarted, go to system settings, and click on “Rename This PC” then underneath “member of” click on domain and in the text box type in “MyDomain.com”. After this you will be prompted to log in. So login using your admin account. Afterwards the computer will restart again
</p>
<br />
<h1>Set Up Desktop for Non Admins on Client 1</h1>
</p>
<br />
</p>
<br />
<img src="https://imgur.com/hgxyvha.png" height="200%" width="200%" alt="Disk Sanitization Steps"/>
19.  Allow “domain users” access to remote desktop. To do that click on “Select users that can remotely access this PC”. Then click on the add button.
</p>
<br />
</p>
<br />
<img src="https://imgur.com/vXuui0T.png" height="200%" width="200%" alt="Disk Sanitization Steps"/>
19b.Type “Domain Users” then click the “check names” button, the list should populate (it is a group in Active Directory where all users automatically become apart of) and click ok. Now the prior text box should have MyDomain\ Domain Users included. You can now log into Client-1 as a normal, non-administrative user.
</p>
<br />
</p>
<br />
</p>
<br />
<h1>Creating Multiple Users on Active Directory</h1>
</p>
<br />
20. Login to DC-1 as jane_admin 
</p>
<br />
21. Right click to Open PowerShell_ise as an administrator.
</p>
<br />
</p>
<br />
</p>
<br />
<img src="https://imgur.com/p4j8aiT.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
22a. Create a new File and paste the contents of a script into it. In order to do that click on the white box under file (top left) in powershell to create a new file and paste script into the white box.
(https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1)
</p>
<br />
</p>
<br />
</p>
<br />
<img src="https://imgur.com/sfuq6ih.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

22b. Click “copy raw contents” on the githbub website to copy the whole script. It will create 10k accounts specified on line 3 and they will all have the same password specified on line 2. On line 43 the script specifies all accounts will be added in OU Employees. 

23. CLick run script (Green triangle underneath “Help” circled in step 22a) and it’ll start up.
</p>
<br />
</p>
<br />
</p>
<br />
<img src="https://imgur.com/1aS08zc.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>    
24. When finished, open Active Directory Users Computer and observe the accounts in the appropriate OU Employees being created (ADUC>mydomain.com>Employees). (Right click and refresh, the page should start populating.)
</p>
<br />
</p>
<br />
</p>
<br />
<img src="https://imgur.com/BpKkEKX.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>   

25. Attempt to log into Client-1 with one of the accounts (take note of the password in the script). In order to do that click on any user and go to the general tab, and copy their display name.
RDP into Client one, and paste their name after “MyDomain.com\” (“MYDomian.com\DISPLAYNAME") 
</p>
<br />
</p>
<br />
</p>
<br />
<h1>Password Troubleshooting and Unlocking accounts</h1>

26. Next log into a different user onto Client 1 from the list and purposely lock out their account by messing up the password many times. Enter it wrong about 7 times and the account will not let you login anymore.
</p>
<br />
</p>
<br />
</p>
<br />
<img src="https://imgur.com/iHNlwPF.png" height="80%" width="80%" alt="Disk Sanitization Steps"/> 
27. Open up DC-1 and find the user again within the user’s list in ADUC>_Employees>Name> rightclick to properties>from general tab go to Account tab so you can click the unchecked box that says “Unlock Account” and hit ok
</p>
<br />
</p>
<br />
</p>
<br />
<img src="https://imgur.com/SMKzQVx.png" height="80%" width="80%" alt="Disk Sanitization Steps"/> 
28. You can also right click their name and click on “Reset Password” and create a new one! Check the unlock account if it is locked and hit ok!
</p>
<br />
</p>
<br />
</p>
<br />
<img src="https://imgur.com/MWOsq3B.png" height="80%" width="80%" alt="Disk Sanitization Steps"/> 
29. You can also right click a name to disable an account and they can’t log in anymore. You can right click to re enable it. 


