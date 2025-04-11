# DNS-with-Domain-Controller
  ![azure-dns](https://github.com/user-attachments/assets/3bf8286f-4575-4b62-998e-8ce1f529d5d7)


<h1>Using DNS with Domain Controller and Client Virtual Machines (Azure)</h1>
This tutorial outlines the implementation of DNS Commands to interact with multiple Azure Virtual Machines.<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell
- DNS

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)


<h2>Deployment and Configuration Steps</h2>

<p>
From our Client Virtual Machine, I attempted to ping "mainframe" in Powershell, after our VM checked it's Local DNS Cache, Local Host File, and finally it's DNS server, it could not find the host. This is because we did not create "mainframe" yet.
</p>
  
  ![Screenshot (1) client](https://github.com/user-attachments/assets/db7acff4-68a1-41a0-8ab8-dea8bc59046a)


<br />
<p>
Next, I opened Notepad as an Administrator, in order to navigate to "C:\Windows\System32\drivers\etc\hosts" to then edit the host file. I assigned "zebra" to the local loopback address 127.0.0.1. Then, if we ping "zebra" in Powershell, it returns our local loopback address successfully.
</p>

 <!-- ![Screenshot (2) client](https://github.com/user-attachments/assets/05cbd4a7-b8e5-4d90-a798-2859e93d6509) -->
  ![Screenshot (3) client](https://github.com/user-attachments/assets/406688e4-f4d4-48e9-86fb-2a6c73847c90)



<br />
<p>
Next, I used Nslookup "mainframe" to show that it still cannot be found. To resolve this, I created an A-record for "mainframe" to have it point to our Domain Controllers Private IP address 10.0.0.4.
</p>

  ![Screenshot (4) client](https://github.com/user-attachments/assets/88500567-dde1-4f9a-8c12-053fbb8c94b1)
  ![Screenshot (4) DC-1](https://github.com/user-attachments/assets/4e2c41f7-e954-4994-a06f-eb8b3f14b4e7)



<br />
<p>
My next step was changing the record address of "mainframe" to 8.8.8.8 on our Domain Controller VM, when I went back to our Client VM to ping it, it was still showing the old 10.0.0.4 address. I then used ipconfig /flushdns to flush the DNS Cache, and pinged "mainframe". The updated address 8.8.8.8 is now showing up.
</p>

  ![Screenshot (5) client](https://github.com/user-attachments/assets/29790864-1dc8-4706-b382-caa9f57ebecc)
  ![Screenshot (6) client](https://github.com/user-attachments/assets/6feff1a0-dcab-422b-8d27-03691419b721)



<br />
<p>
On the Domain Controller, I created a CNAME record that points the host "search" to "www.google.com". When we attempt to ping "search" on our Client VM, it now pings www.google.com and returns the IP address. I then use nslookup "search" to observe the DC resolving and showing the addresses of www.google.com
</p>

  ![Screenshot (7) DC-1](https://github.com/user-attachments/assets/58463459-ba7f-4458-81cd-a967b1caa7c8)
  ![Screenshot (7) client](https://github.com/user-attachments/assets/23b0f3f1-1488-4cb3-982c-f4516d9ecc16)
  ![Screenshot (8) client](https://github.com/user-attachments/assets/5b167d70-701a-4b92-9ae2-8be61aceb491)




<br />
