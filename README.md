<p align="center">
<img src="https://imgur.com/oNr4Sqk.png" alt="osTicket logo"/>
</p>

<h1>Active Directory Installation, Configuration and Troubleshooting</h1>

This creates a Windows Server environment using Microsoft Azure. A domain controller will be created and configured, a Windows client will be added to the domain, and Active Directory will be utilized to manage user accounts, permissions, and group policies. This walkthrough covers user provisioning, domain joins, firewall configs, DNS redirection, and remote access setup for domain users.<br /> 

<h1>Skills Demonstrated</h1>

- Provisioned and configured Azure VMs
- Installed and promoted Windows Server to Domain Controller
- Created and managed Users, Groups, and Organizational Units (OUs)
- Joined a Client VM to a Domain
- Set static IP, DNS, and networking rules
- Granted RDP access to Domain Users 
- Unlocked/reset passwords via ADUC
- Bulk created Users with Powershell script

 
<h2>Environments and Technologies Used</h2>

- Microsoft Azure
- Windows 10 Computer
- Windows 2022 Server
- Remote Desktop
- Active Directory 

<h2>Create Resources</h2>

<p>
<img src="https://imgur.com/D3xi2vk.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
 
 1. Go to resource group and create a new group. You can name the resource group “AD-Lab". Review and create.
</p>
<br />
</p>
<br />
<p>
2. Next create the Domain Controller VM (Windows Server 2022). Go to virtual machines, name the virtual machine “DC-1”. Choose a location (and make sure your next VM, "Client-1" has the same one). Expand the list next to image and Choose windows server 2022 and 2 CPUs. Create your Username and Password (My username will be LabUser), save it! Allow selected ports RDP only. Review and create machine
</p>
<br />
</p>
<br />
<p>
<img src="https://imgur.com/R2Nwn9Z.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
 
2a. After VM is created go to resource and go to the networking tab. Click on the blue font by IP configuration. Click on blue font ipconfig and within that set Domain Controller’s NIC Private IP address to be static then save.
</p>
<br />
</p>
<br />
<p>
 <img src="https://imgur.com/C6Z85az.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
 
2b. The DC must have a static IP so that it doesn’t change, if it did change (after a reboot for example) any computers connected to the domain will experience trouble since they would be trying to connect to the DC’s prior IP! 
</p>
<br />
</p>
<br />
<p>
 <img src="https://imgur.com/ZJL87cs.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
3.Create the Client VM (Windows 10) and name it “Client-1”. Use the same Resource Group that was created in Step 1. Include the same region location to keep everything close and reduce latency. Create a new username and password. Click next, and go to the networking tab to make sure the machine is connected to the same vnet as the DC. The client needs to be on the same virtual network as the Domain Controller to communicate properly. Then create!
</p>
<br />
4. Click on each VM, then check under VirtualNetwork/Subnet to see if its connected to AD-lab
</p>
<br />
<h2>Ensure Connectivity between the Client and Domain Controller</h2>

5. Login to Client-1 with Remote Desktop, search it's public IP and use the login you created. Open command line and ping DC-1’s private IP address with "ping -t "<ip address> (perpetual ping). The ping should fail because the firewall on the DC is blocking traffic! This test checks basic network connectivity from the client to the domain controller and security settings. 
</p>
<br />
</p>
<br />
<p>
 <img src="https://imgur.com/nTbgxtT.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
 
6a. Login to the Domain Controller and enable ICMPv4 on the local windows Firewall, which is the protocol Ping uses. So RDP to DC’s public IP address, login with the username and password you made. In search bar at the bottom dash type firewall and click on windows defendant firewall.
</p>
<br />
</p>
<br />
<p>
 <img src="https://imgur.com/fV0Hguk.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
 
6b. Next click on inbound rules> protocol. Scroll down until you find ICMPv4, right click to enable all of the rules with that protocol
</p>
<br />
</p>
<br />
<p>
 <img src="https://imgur.com/4REr85C.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
 
7. Go back to Client-1 where the continuous ping (ping -t <DC-1_private_IP>) was running.
You’ll notice that the initial pings timed out — this was before ICMPv4 was enabled on the Domain Controller.
As soon as the firewall rule was enabled on DC-1, successful ping replies should begin appearing.
Press Ctrl + C to stop the ping.
Now that the necessary resources are deployed and basic connectivity is confirmed, we can begin installing Active Directory Domain Services (AD DS) on the Domain Controller.
</p>
<br />
<h1>Active Directory Installation</h1>
</p>
<br />
 </p>
<br />
 <img src="https://imgur.com/4HXds4o.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

8a. Login to DC-1 and go to server manager>add roles and features. Hit next, make sure it's selected on “role based installation” then hit next, make sure DC-1 is the selected server then hit next. 

<br />
 </p>
<br />
</p>
<br />
 <img src="https://imgur.com/ynybIMT.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
8b. 
In the selecet Server Role window, check Active Directory Domain Services.
A pop up will appear, click add features to confirm.
Click next through the server role summary, features, and overview. Finally, click install ADDS.
The server has the necessary software but it's not a DC yet.
</p>
<br />
 </p>
<br />
</p>
<br />
<img src="https://imgur.com/U3itFYu.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

9a. Next you will promote it as DC. After installation there will be a yellow exclamation point in the top right corner. Click on it then click the blue text, “Promote the server to a DC". ,This step begins the process of promoting the server so it can act as a Domain Controller, enabling it to manage authentication, DNS, and directory services across your environment.

</p>
<br />
 </p>
<br />
</p>
<br />
<img src="https://imgur.com/0EbGXnO.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
9b. On the deployment configuration page select “Add a new forest”,  which is the top-level security boundary in Active Directory. Next create a root domain name, (mydomain.com). This is the first domain in the structure aka your root domain. click next to Domain Controller options and 
create a DSRM(Directory Services Restore Mode) password! DSRM is a local-only administrator password used to log into a Domain Controller in Directory Services Restore Mode, typically for repairing or recovering Active Directory or the DC itself. Click next on DNS options, click next on additional notes. Then click next on review and click next on prerequisites, click install.
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
10.Restart to apply the configurations and then log back into DC-1 as mydomain.com\LabUser Adding the domain prefix (mydomain.com\) tells Windows to authenticate against Active Directory instead of the local Security Accounts Manager database. A successful logon confirms that DC‑1 has fully promoted and is now serving domain authentication.
</p>
<br />
<h1>Create an Admin and Normal User Account in AD </h1> 
</p>
<br />
<img src="https://imgur.com/x6TEu5r.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
On DC-1, open Server Manager.

11. In the top-right corner, click Tools, select Active Directory Users and Computers (ADUC).

In the left-hand pane, expand your domain (e.g., mydomain.com).

Right-click on the domain name> select New> Organizational Unit.

Name the new OU: _EMPLOYEES (make sure the format and spelling is exact, we will be using a script that will send users to this file if it's mispelled the users will not be sent to the correct location.)

Click OK.
</p>
<br />
 </p>
<br />
</p>
<br />
<img src="https://imgur.com/BUCTrpw.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
11b. Within ADUC click on mydomain.com right click>new> click Organizational Units, and name it “_Employees”
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
16. User Jane has succesfully become an admin!
</p>
<br />
<h1>Connecting Client-1 to DC </h1>
</p>
<br />
<img src="https://imgur.com/NhISrFO.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
17a. From the Azure Portal, set Client-1’s DNS settings to the DC’s Private IP address. This will join Client-1 to your domain mydomain.com. That way you can log onto Client 1 using the accounts you made in the DC. By default, Client-1 will use Azure’s default DNS, which won’t recognize your AD domain. Pointing Client 1's DNS to DC-1 ensures that Client-1 can join the domain successfully.
</p>
<br />
</p>
<br />
</p>
<br />
<img src="https://imgur.com/hCh7iCO.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
17b. Within Clinet-1's Command Prompt type “IPConfig/All”  and on the DNS line you’ll see it does not have the I.P address of your DC. So this confirms that Client-1 is not fully connected to the DC yet. The IP address we want to connect to is 10.2.0.4
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
17d. So go back to Client 1 VM and click on it. Go to networking and then click the blue text next to “Network Interface:” >DNS servers on the left side list. Under DNS servers “Inherit from Virtual Network” will be checked so change it to “custom”. Paste DC’s Private IP address into the “Add DNS server” text box. Click the save button towards the top of the page. Changing it to custom and specifying the DC’s IP ensures Client-1 uses the Domain Controller’s DNS to properly resolve domain names and services required to join and operate within the domain.
</p>
<br />
</p>
<br />
</p>
<br />
<img src="https://imgur.com/iUomZ6a.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
18. From the Azure Portal, restart Client-1
You must restart to flush the DNS cache so that it can forget VMWare’s IP address as the DNS and start connecting to DC-1’s DNS. After you restarted, RDP back in, go to system settings, go to the "about" tab, and click on “Rename This PC (advanced)”. Then click the "change button" next to "rename this compuer", underneath “member of”, click on domain and in the text box type in “MyDomain.com”. After this you will be prompted to log in. So login using your admin account. Afterwards the computer will restart again
</p>
<br />
<h1>Creating Multiple Users on Active Directory</h1>
</p>
<br />
19. Login to DC-1 as jane_admin 
</p>
<br />
20. Right click to Open PowerShell_ise as an administrator.
</p>
<br />
</p>
<br />
</p>
<br />
<img src="https://imgur.com/p4j8aiT.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
21a. Create a new File and paste the contents of the script into it. In order to do that click on the white box under file (top left) in powershell to create a new file and paste script into the white box.
(https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1)
</p>
<br />
</p>
<br />
</p>
<br />
<img src="https://imgur.com/sfuq6ih.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

21b. Click “copy raw contents” on the githbub website to copy the whole script. It will create 10k accounts specified on line 3 and they will all have the same password specified on line 2. On line 43 the script specifies all accounts will be added in OU Employees. 

22. Click run script (Green triangle underneath “Help” circled in step 22a) and it’ll start up.
</p>
<br />
</p>
<br />
</p>
<br />
<img src="https://imgur.com/1aS08zc.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>    
23. When finished, Back in the server menu from DC In the top-right corner, click Tools, select Active Directory Users and Computers (ADUC). open Active Directory Users Computer and observe the accounts in the appropriate OU Employees being created (ADUC>mydomain.com>_EMPLOYEES). (Right click and refresh, the page should start populating.)
</p>
<br />
</p>
<br />
</p>
<br />
<img src="https://imgur.com/BpKkEKX.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>   
<h1>Set Up Desktop for Non Admins on Client 1</h1>
</p>
<br />
</p>
<br />
<img src="https://imgur.com/hgxyvha.png" height="200%" width="200%" alt="Disk Sanitization Steps"/>
24.  Allow “domain users” access to remote desktop. To do that go to system settings. Go to the Remote Desktop tab and click on “Select users that can remotely access this PC”. Then click on the add button.
</p>
<br />
</p>
<br />
<img src="https://imgur.com/vXuui0T.png" height="200%" width="200%" alt="Disk Sanitization Steps"/>
25b.Type “Domain Users” then click the “check names” button, the list should populate (it is a group in Active Directory where all users automatically become apart of) and click ok. Now the prior text box should have MyDomain\ Domain Users included. You can now log into Client-1 as a normal, non-administrative user.
</p>
<br />
</p>
<br />
</p>
<br />

26. Attempt to log into Client-1 with one of the accounts, take note of the password in the script ("Password1"). In order to do that click on any user and go to the general tab, and copy their display name.
RDP into Client one, and paste their name after “MyDomain.com\” (“MYDomain.com\DISPLAYNAME") 
</p>
<br />
</p>
<br />
</p>
<br />
<h1>Password Troubleshooting and Unlocking accounts</h1>

27. Next log into a different user onto Client 1 from the list and purposely lock out their account by messing up the password many times. Enter it wrong about 7 times and the account will not let you login anymore.
</p>
<br />
</p>
<br />
</p>
<br />
<img src="https://imgur.com/iHNlwPF.png" height="80%" width="80%" alt="Disk Sanitization Steps"/> 
28. Open up DC-1 and find the user again within the user’s list in ADUC>_Employees>Name> rightclick to properties>from general tab go to Account tab so you can click the unchecked box that says “Unlock Account” and hit ok
</p>
<br />
</p>
<br />
</p>
<br />
<img src="https://imgur.com/SMKzQVx.png" height="80%" width="80%" alt="Disk Sanitization Steps"/> 
29. You can also right click their name and click on “Reset Password” and create a new one! Check the box by unlock account and hit ok!
</p>
<br />
</p>
<br />
</p>
<br />
<img src="https://imgur.com/MWOsq3B.png" height="80%" width="80%" alt="Disk Sanitization Steps"/> 
30. You can also right click a name to disable an account . You can right click to re enable it. 
</p>
<br />
30. Active Directory is a useful software for helping organize users and their permissions! Hopefully this gave you valuable insight on how
Active Directory works! That is the end of this lab.


