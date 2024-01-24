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
- Step 3 - Install Active Directory
- Step 4 - Create an Admin and Normal User Account in Active Directory
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
