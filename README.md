<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Setup Resources in Microsoft Azure
- Create two Virtual Machines in the same VNET DNS Server (Domain Controller and Client)
- Ensure connectivity between the Client and Domain Controller
- Install Active Directory and set up a new forest as mydomain.com
- Create an Admin and Normal User in Active Directory
- Join Client VM to mydomain.com
- Setup Remote Desktop for non-administrative users on Client VM
- Create 1000 of addtional users and log into Client VM with one of the new users

<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://imgur.com/XgrUDDP.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
In this tutorial, I will create Virtual Machines in Microsoft Azure and install on-premises version of Active Directory on the Azure Virtual Machine. Both Virtial Machines will be created in the same VNET DNS Server. First Virtual Machine will be Domain Controller - DC-1 (server that has Active Directory installed on it and running on Windows Server 2022) and second Virtual Machine is Client Machine - Client-1 (running on Windows 10 (21H2).
</p>
<br />

<p>
<img src="https://imgur.com/eWWIfmv.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://imgur.com/QtDyerG.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Created two Virtual Machines - DC-1 and Client-1 in Azure. Changed the DC-1 to a static IP address because its offering Active Directory services to the client machine and Client machine will be joined to the domain. 
</p>
<br />

<p>
<img src="https://imgur.com/7qOCUtY.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://imgur.com/fWFmqKt.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
To ensure connectivity between Client-1 and DC-1, I have enabled ICMPv4 on the Microsoft Defender Firewall on DC-1, logged in into Client-1 and pinged DC-1's private IP address with -t(perpetual ping) and ensured that the ping was successful.
</p>
<br />
<p>
<img src="https://imgur.com/DKpcnBN.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://imgur.com/bBYxF5c.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Logged in to DC-1 and installed Active Directory Domain Services, set up a new forest as mydomain.com, restarted and then logged in back to DC-1 as user: mydomain.com\kbuser.
</p>
<br />
<p>
<img src="https://imgur.com/ZXSB6OB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://imgur.com/X5URyRn.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
In Active Directory Users and Computers (ADUC), created an Organizational Unit (OU) called “_EMPLOYEES”, created a new OU named “_ADMINS” and created a new employee named “Jane Doe” (same password) with the username of “jane_admin”. Added jane_admin to the “Domain Admins” Security Group, loged out/closed the Remote Desktop connection to DC-1 and log back in as “mydomain.com\jane_admin” and I can use jane_admin as my admin account from now on.
</p>
<br />
<p>
<img src="https://imgur.com/NCG2v3V.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://imgur.com/6gFan4R.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
In Azure Portal, I set Client-1's DNS settings to the DC-1's Private IP address. From Azure Portal, I restarted Client-1 and logged in to Client-1 as the original local admin (kbuser) and joined it to the domain.
</p>
<br />
<p>
<img src="https://imgur.com/XuE4pOG.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://imgur.com/UgZpOZ5.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
I will set up Remote Desktop for non-administrative users on Client-1. In order to do that, I logged in to Client-1 as mydomain.com/jane_admin and in System Properties-> Remote Desktop allowed "Domain users" access to remote desktop.
</p>
<br />
<p>
<img src="https://imgur.com/vu33ARF.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://imgur.com/Jt0nbAS.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://imgur.com/mvdMmm3.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
I will create additional users and attempt to log in into Client-1 with one of the users. In order to do that, I logged in to DC-1 as jane_admin, opened PowerShell ISE as administrator, created a new file and pasted a script to generate thousands of users into the domain. I logged into Client-1 with "bim.hik" account credentials as a normal user. Success!
</p>
<br />
