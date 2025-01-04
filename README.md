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


<h2>Deployment and Configuration Steps</h2>

<p>
</p>
<p>
In this lab, we will create two Virtual Machines (VMs) within the same Virtual Network (VNET). One VM will be configured as a Domain Controller (DC), while the other will serve as a Client machine. The Domain Controller will be assigned a static IP address, as it will be providing Active Directory services to the Client. The Client machine will be joined to the domain, and we will configure its DNS settings to use the Domain Controller as its DNS server, ensuring proper resolution of domain-related resources. 
</p>
<br />

<p>
<img src="https://i.imgur.com/d22FHIm.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
The DC-1 virtual machine must be assigned a static private IP address. The Client-1 machine will attempt to connect to DC-1 to verify connectivity by pinging it. Initially, the ping will fail due to ICMPv4 being blocked by the firewall on DC-1. To resolve this, we need to enable ICMPv4 on the firewall. After making this change, Client-1 will be able to successfully ping DC-1, confirming proper network connectivity.
</p>
<br />

<p>
<img src="https://i.imgur.com/HvZBWzc.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
<img src="https://i.imgur.com/1lrrGPw.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
<p>
Next, log back into DC-1 and install the Active Directory Users and Computers tool. Then, promote the VM to a Domain Controller by setting up a new forest with the domain name mydomain.com. After the promotion, restart the VM. Once it has rebooted, log back into DC-1 using the domain account mydomain.com\labuser. If the steps were followed correctly, you should be able to access Active Directory Users and Computers as demonstrated below.
</p>
<img src="https://i.imgur.com/cGjvRke.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
</p>
Let's begin by creating Organizational Units (OUs). First, create an OU called _EMPLOYEES, and then create another OU named _ADMINS. To do this, right-click on the domain, select New > Organizational Unit, and complete the required fields.

Next, navigate to the _ADMINS OU, right-click inside it, and choose New > User. Enter the details for the new user: Jane Doe. Since Jane will be an administrator, assign her the username Jane_admin.

Finally, add Jane to the Domain Admins security group to grant her administrative privileges.
</p>
<img src="https://i.imgur.com/hL7g5Y5.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
</p>
<img src="https://i.imgur.com/kcgvzdE.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
From this point forward, we'll use Jane_admin as the administrator account. Next, we will join Client-1 to the domain mydomain.com. To do this, go to the Azure portal and update Client-1's DNS settings to point to the DC-1's private IP address. After making this change, restart Client-1 from within the Azure portal.

The screenshot below verifies that Client-1 is successfully using DC-1 as its DNS server.
</p>
<img src="https://i.imgur.com/jbrGTXW.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
</p>
<img src="https://i.imgur.com/kvcm2cY.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<p>
To join Client-1 to the domain, follow these steps:

Navigate to System Settings and select About.
On the right side, click Rename this PC (Advanced).
In the window that appears, click Change to modify the computer’s domain.
Enter the domain name mydomain.com.
When prompted, enter your credentials as mydomain.com\labuser.
After completing these steps, your computer will restart, and Client-1 will successfully be joined to the mydomain.com domain.
</p>
<br />
<p>
  <p>
<img src="https://i.imgur.com/Ze0Em5e.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Client-1 is now successfully joined to the domain. Next, we'll configure Remote Desktop access for non-administrative users.

Log into Client-1 using an admin account.
Open System Properties and navigate to the Remote Desktop section.
Enable Remote Desktop and select the option to allow Domain Users access.
Once you've completed these steps, non-administrative users should be able to log into Client-1 via Remote Desktop.

</p>
<br />

<p>
  <p>
<img src="https://i.imgur.com/SApOKiE.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Finally, to verify that normal users can successfully use Remote Desktop to connect to Client-1, we will generate a large number of users in the domain using a PowerShell script. Once the users are created, we will select one of them and attempt to RDP into Client-1 to confirm functionality.
</p>
<br />
<img src="https://i.imgur.com/EzWG8ug.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<p>
<p>
  <p>
<img src="https://i.imgur.com/Gkpe68K.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
<img src="https://i.imgur.com/n3gMwQV.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
<p>
As demonstrated, the PowerShell script successfully created a user with the username "bab.hubo". We were then able to log into Client-1 using his credentials, confirming that the login worked as expected for a normal user.
</p>
