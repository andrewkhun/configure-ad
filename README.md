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
<img src="https://imgur.com/xlZqVNV.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>
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

Once connected to Client-1, open up the Command Prompt and ping the private IP address of DC-1 and replies should be returned ensuring connectivity between the two virtual machies.
</p>
<p>
<img src="https://imgur.com/vsZTSOj.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>
</p>






<h2>Install and Configure Active Direcory</h2>
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
