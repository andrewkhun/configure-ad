<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Active Directory Deployed in the Cloud (Azure)</h1>
This hands-on tutorial guides you through the seamless setup of an on-premises Active Directory within Azure Virtual Machines. Gain practical experience installing Active Directory on a Domain Controller VM and seamlessly joining a Client VM to the Domain. Dive into vital tasks like configuring Remote Desktop and effectively managing user accounts. Ensure proper resource management by confidently deleting the resource group and verifying its successful removal.

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Step 1 - Setup Resources in Azure
- Step 2 - Ensure Connectivity Between the Client and Domain Controller
- Step 3 - Install and Configure Active Directory
- Step 4 - Create Admin and Normal User Accounts and Organizational Units in Active Directory
- Step 5 - Join Client-1 to your Domain
- Step 6 - Setup Remote Desktop for Non-Administrative Users on Client-1
- Step 7 - Create Additional Users and Attempt to Log-In
- Step 8 - Delete Resource Groups or Continue to Next Lab

<h2>Step 1 - Setup Resources in Azure</h2>

<p>
We will be setting up two virtual machines to complete this lab. The first virtual machine will be a Domain Controller VM (Windows Server 2022) named "DC-1" and our second will be the Client VM(Windows 10) named "Client-1".

Navigate to the Azure Portal, search for "Virtual Machine" and click "Create Azure Virtual Machine".

Choose your Subscription (i.e. Azure Subscription 1). Create a new Resource group and create a name (i.e. RG-Lab). Name the VM "DC-1" for Domain Controller 1. Pick a region where the virtual machine is to be created. 

For Image, select Windows Server 2022 Datacenter: Azure Edition - Gen2 (free services eligible). For Size, choose Standard_E2s_v3 - 2 vcpus, 16 GiB memory.

Set a username and password for the machine. Then navigate to the Networking tab and ensure a virtual network is created.

Once all settings have been applied click Review + Create, then Create once the validation has passed.
</p>
<p>
<img src="https://imgur.com/Ox0KZZZ.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://imgur.com/J9SmXmM.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://imgur.com/vCawGQL.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>
</p>
<p>
Navigate to DC-1 in the Azure Portal and click on Networking and then Network Interface (aka NIC, which should say something like dc-1324). Click on IP configurations, click on ipconfig1, and change the private IP address settings, Allocation to Static. Static means the IP address is always going to be this and it is not going to change (regardless of if we turn the computer off and on).
</p>
<p>
<img src="https://imgur.com/8QNwHCD.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://imgur.com/Iy4zPx7.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>
</p>


<p>
Next, create another virtual machine for Client-1. Ensure the VM is placed inside the same resource group and region. 

For Image, select Windows 10 Pro, version 22H2. For Size, choose Standard_E2s_v3 - 2 vcpus, 16 GiB memory.

Set a username and password for the machine. Then navigate to the Networking tab and ensure Client-1 joins the same virtual network created during DC-1's creation.

Once all settings have been applied click Review + Create, then Create once the validation has passed.
</p>
<p>
<img src="https://imgur.com/s4ABn33.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://imgur.com/s4ABn33.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://imgur.com/g1vc41M.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>
</p>
</br>






<h2>Step 2 - Ensure Connectivity Between the Client and Domain Controller</h2>
<p>
Retrieve the copy IP address for DC-1 from the Azure Portal. Open Remote Desktop Connection on your local machine and connect to DC-1 using the private IP address and credentials we previously created.

Once inside DC-1, search for Windows Defender Firewall. Click on Advanced Settings. We must enable ICMP Echo requests to test the reachability between Client-1 and DC-1. Click on Inbound rules on the left side and within the main window filter by Protocol and enable the ICMP requests.
</p>
<p>
<img src="https://imgur.com/klfuPbg.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://imgur.com/BbXwfJ9.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://imgur.com/An9YTjf.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>
</p>
<p>
Once ICMP requests have been enabled, head back to your local machine and connect to Client-1 through Remote Desktop Connection. Copy the Public IP address of Client-1 from the Azure Portal and log in using the credentials created during Client-1's setup. 

Once connected to Client-1, open up the Command Prompt and ping the private IP address of DC-1 and replies should be returned ensuring connectivity between the two virtual machines.
</p>
<p>
<img src="https://imgur.com/vsZTSOj.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>
</p>






<h2>Step 3 - Install and Configure Active Direcory</h2>
<p>
Go back into the DC-1 VM. In the Server Manager application, click Add Roles and Features (this is how we will install Active Directory). Click Next (3 times). Check the box next to Active Directory Domain Services, a pop-up screen may appear, just click Add Features. Click Next (3 times), then click Install, then close once it has finished installing.
</p>
<p>
<img src="https://imgur.com/vaZgGWo.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://imgur.com/karuhfT.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>
</p>
<p>
To turn DC-1 into a Domain Controller, click on the flag with an exclamation symbol on the top right of the Service Manager application. Click on Promote this server to a domain controller, then click on Add a new forest.

Enter a root domain name (i.e. mydomain.com), click Next, create a password, click Next until the Install option appears, then click Install. Once complete, the remote connection will disconnect due to DC-1 restarting (you will need to reconnect).
</p>
<p>
<img src="https://imgur.com/4TSBTAi.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://imgur.com/duGIpM6.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://imgur.com/zYEFXal.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://imgur.com/aa6gxkE.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>
</p>
<p>
Open another window of the Remote Desktop Connection application on your local machine and log back into DC-1. You will notice that your credentials no longer work because DC-1 is a domain controller. To reconnect click on "Use a different account". For "Email address", enter the FQDN (aka Fully Qualified Domain Name; i.e. mydomain.com\labuser), then enter your password for labuser.
</p>
<p>
<img src="https://imgur.com/I9TdcDc.png" height="30%" width="30%" alt="Disk Sanitization Steps"/>
</p>
</br>






<h2>Step 4 - Create Admin and Normal User Accounts and Organizational Units in Active Directory</h2>
<p>
Now that you are logged back into DC-1, within the Server Manager, click on Tools, then click on Active Directory Users and Computers. Right-click on mydomain.com, go to New, then click Organizational Unit. Enter a name (i.e. _EMPLOYEES) and click OK. Create another Organizational Unit and name it (i.e. _ADMINS), then click OK. 
</p>
<p>
<img src="https://imgur.com/RvJNjzt.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://imgur.com/RDO21Os.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next, you will create a new admin and add them to the "Domain Admins" Security Group. Right-click on _ADMINS and create a new User. We will create a new user named "Jane Doe" with the username "jane_admin" and a password (i.e. Password1). When creating the user, you can uncheck "User must change password at next logon" and check the box for "Password never expires" for this lab.
</p>
<p>
<img src="https://imgur.com/0XB18En.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://imgur.com/CHZ1WhG.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>
</p>
<p>
Once Jane Doe has been created, right-click on her name and click on Properties. Type in Domain Admins to grant Jane admin privileges.
</p>
<p>
<img src="https://imgur.com/7RegEJv.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>
</p>
<p>
Log out of DC-1, then log back in as jane doe. Re-open the Remote Desktop Connection application on your local machine and log in with the new credentials we have just created for Jane Doe. Ensure you use the FQDN for your domain (i.e. Username: mydomain.com\jane_admin and Password: Password1).
</p>
<p>
<img src="https://imgur.com/PzkZXNz.png" height="30%" width="30%" alt="Disk Sanitization Steps"/>
</p>



<h2>Step 5 - Join Client-1 to your Domain</h2>
<p>
Earlier, the Virtual Network (Vnet) created a "hidden" DNS server that Client-1 is currently using. To join Client-1 to our domain (mydomain.com), Client-1 needs to use DC-1 as its DNS server. This is because DC-1 knows what our domain is. If we allow Client-1 to maintain its current DNS server, it will not be able to join the domain.

Access the Azure portal and go to DC-1, go to Networking, and take note of the Private IP Address. Access Client-1's Network Interface and click on DNS servers. Set the DNS server to custom enter DC-1's private IP address and save. Go to Client-1 in the Azure Portal and restart the VM.
</p>
<p>
<img src="https://imgur.com/dQQ98NO.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>
</p>
<p>
Log back into Client-1 with the credentials made during the VM setup. Right-click the Start Menu in the bottom left corner of the screen, click System, then click Rename this PC (advanced), then click Change...

Select "Domain" type mydomain.com and Click OK, this will join Client-1 to DC-1's domain. A pop-up will appear asking for credentials, type in Jane Doe's credentials and Click OK (i.e. mydomain.com\jane.doe, Password1).
</p>
<p>
<img src="https://imgur.com/737qkBu.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://imgur.com/6YDAXg1.png" height="30%" width="30%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://imgur.com/Hol4sVt.png" height="30%" width="30%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://imgur.com/Dwu2EQ8.png" height="30%" width="30%" alt="Disk Sanitization Steps"/>
</p>
<p>
Allow Client-1 to restart to finish joining the domain. When reconnecting to Client-1 through Remote Desktop Connection, we will now be able to log in using Jane Doe's admin credentials for mydomain.com.
</p>





<h2>Step 6 - Setup Remote Desktop for Non-Administrative Users on Client-1</h2>
<p>
Log back into Client-1 with jane doe's credentials. Right-click the Start menu and click System, then Remote Desktop. Click Select users that can remotely access this PC. Notice you can see who is currently allowed access to remote desktop into the computer. Click Add and type in "Domain Users", click Check Names, then OK. This will allow any users in the Domain Users security group remote desktop access to Client-1.
</p>
<p>
<img src="https://imgur.com/tl46Wsn.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://imgur.com/0gUwnes.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>
</p>



<h2>Step 7 - Create Additional Users and Attempt to Log In</h2>
<p>
Next, we will use a script to add additional users to our domain. Head back into DC-1 log in as jane_admin and open Powershell ISE as administrator. 

Copy this <a href="https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1">code</a> and paste it into Powershell ISE. This script will generate 10,000 new users and place them into the Domain Users security group.

Once the code has been pasted, click on Run Script to begin adding users to the domain.
</p>
<p>
<img src="https://imgur.com/MMAsAnB.png" height="30%" width="30%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://imgur.com/cDVYQUA.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>
</p>
<p>
After the script has finished running you can view the results in the Server Manager under Active Directory's Users and Computers within the _EMPLOYEES OU. Each of the newly generated users has their password set to Password1. If you have not already, log out of Client-1 and select a user from the list of new users to log in.
</p>
<p>
<img src="https://imgur.com/DGcsvEI.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>
</p>
<p>
Open up Remote Desktop Connection, input the new user's credentials and password (i.e. username: mydomain.com\bat.ciq password: Password1), and connect to Client-1 with your new user setup in Active Directory!
</p>
<p>
<img src="https://imgur.com/ZLek2DM.png" height="30%" width="30%" alt="Disk Sanitization Steps"/>
</p>





<h2>Step 8 - Delete Resource Groups or Continue to Next Lab</h2>
<p>
Now that you have completed the lab, you can either navigate to the Azure Portal and delete the Resource Groups to prevent any incurring costs or you can continue to the next lab, which will use the Active Directory we have set up. 

If you would like to delete the resource groups, navigate to the Azure Portal and click Resource Groups. Click on each Resource Group, then click on Delete Resource Group. Copy and paste the name of the resource group to confirm deletion. Then confirm each group has been deleted.

If you would like to continue to the next lab click on an option below:
  - [Understanding and Building Intuition for DNS](https://github.com/andrewkhun/dns)
  - [Network File Shares and Permissions](https://github.com/andrewkhun/network-fileshare)
</p>

