<p align="center">
<img src="https://i.imgur.com/Clzj7Xs.png" alt="osTicket logo"/>
</p>

<h1>Active Directory Deployment</h1>
This tutorial outlines how to deploy Active Directory and create users<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Computer)
- Remote Desktop
- Active Directory Users and Computers
- PowerShell ISE

<h2>Operating Systems Used </h2>

- Windows 10</b> (21H2)
- Windows Server 2022 

<h2>Prerequisites</h2>

- Microsoft Azure subscription
  

<h2>Steps</h2>


Setup Resources in Azure
-- 

  
  **1. Create the Domain Controller VM named DC-1:**
  - Go to portal.azure.com --> Virtual machines --> Create, then Azure virtual machine
  - Create a Resource group 
  - Give the VM a name of DC-1 
  - Set your region 
  - Set Image to Windows Server 2022 
  - Set Size to 2 vcpu or more 
  - Enter a username and password
  - Check the box that says 'I confirm I have an eligible Windows 10/11 license with multi-tenant hosting rights.'

  <img src="https://i.imgur.com/Vg5blOJ.png" height="50%" width="50%" alt="Creating DC"/>
    
  - Click Next until you get to Networking page --> Check that a Virtual Network has been created and make a note of the name of it
  - In the example below a Virtual network called 'DC-1-vnet' is created

  <img src="https://i.imgur.com/eyUSnYX.png" height="50%" width="50%" alt="Creating DC"/>
    
  - Click Review + create --> Once validation has passed click Create 
  - Wait until deployment is complete, then move on to the next step
      
<br> 
 
  **2. Set DC-1's NIC Private IP address to static:** 
  - Go to Virtual machines --> DC-1 --> Under Settings click Networking --> Click the link that is next to Network Interface (Image below)

    <img src="https://i.imgur.com/eu2h84a.png" height="70%" width="70%" alt="Creating Client"/>
     
  - Under Settings click IP configurations --> ipconfig1 --> Under Private IP address settings, select Static --> Save

    <img src="https://i.imgur.com/RucRQvC.png" height="70%" width="70%" alt="Creating Client"/>
  

<br>
  
  **3. Create the Client-1 VM (virtual machine):**
  - Go to Virtual machines --> Create --> Azure virtual machine 
  - Use the same resource group that was created in step 1
  - Name the VM Client-1
  - Set the same region as DC-1
  - Set the Image to Windows 10
  - Set Size to 2 vcpus or more
  - Add the username and password
  - Check the box that says 'I confirm I have an eligible Windows 10/11 license with multi-tenant hosting rights.'

  <img src="https://i.imgur.com/Xk6sEVx.png" height="50%" width="50%" alt="Creating Client"/>
    
  - Click Next until you get to the Networking page
  - Make sure the Virtual network is set as the same virtual network created for DC-1. In this case it's DC-1-vnet

  <img src="https://i.imgur.com/oHVilFE.png" height="50%" width="50%" alt="Creating Client"/>
  
  - Review + create
  - Once validation has passed --> Create

  
  **4. Check that DC-1 and Client-1 are in the same virtual network:**
   - Go to Virtual machines --> DC-1 --> Look under 'Virtual network/subnet' --> Client-1 --> Look under 'Virtual network/subnet'
   <img src="https://i.imgur.com/g3AILcD.png" height="80%" width="80%" alt="Creating Client"/>
    
  
  **5. Login to Client-1 with Remote Desktop Connection and ping DC-1's private IP address:**
   - In Azure copy Client-1's public IP address
   - Click Start --> Enter Remote Desktop Connection --> Paste Client-1's IP address --> More choices --> Use a different account
   - Enter the username and password you created for Client-1

     <img src="https://i.imgur.com/9H8qwy3.png" height="50%" width="50%" alt="Creating Client"/>

   - Go to Azure portal and copy DC-1's private IP address

     <img src="https://i.imgur.com/fjYNj77.png" height="70%" width="70%" alt="Creating Client"/>
    
   - From Client-1 click Start --> Enter 'cmd' --> Enter ping -t (paste DC-1's private IP address). In my example: ping -t 10.0.0.4
   - You should get 'Request timed out.' 
  

  **6. Login to DC-1 and enable ICMPv4 in the local windows Firewall:**
   - Go to the Azure portal --> Copy DC-1's public IP address --> Open another instance of Remote Desktop Connection --> Paste DC-1's IP address and click Connect --> Enter username and password for DC-1
   - In DC-1 --> Start --> Enter Windows Defender Firewall with Advanced Security
   - Click Inbound Rules --> Protocol --> Under Protocol look for ICMPv4 --> Right click and enable 'Core Networking Diagnostics - ICMP Echo Request (ICMPv4-In)

     <img src="https://i.imgur.com/HaaBEgt.png" height="80%" width="80%" alt="Creating Client"/>
 
  
  **7. Check back at Client-1 to see the ping succeed:**
   - Go back to Client-1 and check to see if the ping to DC-1 is now working 
   - Press Ctrl + C to stop the ping

Install Active Directory
-- 

**8. Login to DC-1 and install Active Directory Domain Services:**
 - In DC-1
 - Start --> Server Manager
 - Add roles and features
 - Before You Begin: Next
 - Installation Type: Check Role-based or feature-based installation --> Next
 - Server Selection: DC-1 --> Next
 - Server Roles: Check Active Directory Domain Services --> Add Features --> Next
 - Features: Next
 - AD DS: Next
 - Confirmation: Install
 - Screenshot below is the final installation screen

<img src="https://i.imgur.com/FfvG8mJ.png" height="50%" width="60%" alt="Creating Client"/>
 
**9. Promote as a Domain controller:**
 - Top right corner of the screen there is a yellow exclamation symbol. Click this symbol --> Promote this server to a domain controller

 <img src="https://i.imgur.com/twBhGZa.png" height="70%" width="70%" alt="Creating Client"/> 
   
 - Check Add a new forest
 - Root domain name: mydomain.com (or whatever domain you choose) --> Next
 - Add a password --> Next
 - DNS Options: Next
 - Addtional Options: Next
 - Paths: Next
 - Prerequisites Check: Install
 - Once completed DC-1 will restart
  
**10. Log back into DC-1 as the user (domain name\username):**
  - In Azure portal Refresh DC-1
    
    <img src="https://i.imgur.com/l0645bI.png" height="70%" width="70%" alt="Creating Client"/>
    
  - Log back into DC-1 as (domain name\username) <br> <br>

Create an Admin and Normal User Account in Active Directory
-- 

**11. In Active Directory Users and Computers (ADUC), create an Organisational Unit (OU) called _EMPLOYEES**
 - In DC-1 
 - Click Start --> Enter 'Active Directory Users and Computers
 - In the domain you created --> Right click --> New --> Organizational Unit --> _EMPLOYEES
 
**12. Create an Organisational Unit named _ADMINS**
 - In ADUC
 - In your domain --> Right click --> New --> Organizational Unit --> _ADMINS
 - Once both OU has been created should look like the image below

<img src="https://i.imgur.com/bhCEGLP.png" height="70%" width="70%" alt="Creating Client"/>
   
**13. Create an admin account:**
 - Go to the _ADMINS OU you created --> Right click --> New --> User 
 - Give the account a name and login name --> Next

<img src="https://i.imgur.com/nJwr0bl.png" height="50%" width="60%" alt="Creating Client"/>
    
 - Add a password --> Next --> Finish

 

 **14. Add the account you created previously to the Domain Admins Security Group:**
  - Right click the account you made --> Properties --> Member of --> Add --> Type domain admins --> Check Names --> OK --> Apply -- OK

  <img src="https://i.imgur.com/KdPfaBt.png" height="50%" width="50%" alt="Creating Client"/>
   
   
  **15. Log out the Remote Desktop connection to DC-1 and log back in as (domain name\admin account):**
   - Log out of DC-1 --> Bring up Remote Desktop connnection --> Enter DC-1's public IP address --> Login with (domain name\admin account)
   - For example mydomain.com\werner_admin
    
Join Client-1 to your domain
-- 

**16. From the Azure portal, set Client-1's DNS settings to the DC's Private IP address:**
 - Go to the Azure portal --> Virtual machines --> DC-1 --> Networking --> Next to NIC Private IP, copy this address

  <img src="https://i.imgur.com/HEPUjd1.png" height="70%" width="70%" alt="Creating Client"/>

  - Go to Virtual machines --> Client-1 --> Networking --> Click the link next to 'Network Interface'

  <img src="https://i.imgur.com/rz9UYIQ.png" height="70%" width="70%" alt="Creating Client"/>

  - Under Settings, click DNS Servers --> Custom --> Paste DC-1's private IP address under DNS server --> Save

  <img src="https://i.imgur.com/9LlaVaj.png" height="70%" width="70%" alt="Creating Client"/>
    

   
   


  <hr>
  
  <p>
    17. From the Azure portal, restart Client-1: <br>
    &nbsp &nbsp - Go to Virtual Machines, Client-1 <br>
    &nbsp &nbsp - Click Restart <br> <br>
  </p>

  <hr>

  <p>
   <img src="https://i.imgur.com/UjIuuqi.png" height="80%" width="80%" alt="Log in as admin"/> <br>
  </p>
  
  <p>
   <img src="https://i.imgur.com/E2Bbvvi.png" height="80%" width="80%" alt="Log in as admin"/> <br>
  </p>
  
  <p>
   <img src="https://i.imgur.com/86jJAqp.png" height="80%" width="80%" alt="Log in as admin"/> <br>
  </p>
  

  <p> 
    18. Login to Client-1 and join it to the domain: <br>
    &nbsp &nbsp - Login to Client-1 using Remote Desktop connection <br>
    &nbsp &nbsp - Go to command line, then enter ipconfig /all <br>
    &nbsp &nbsp - Next to DNS Servers should be DC-1's private IP address <br>
    &nbsp &nbsp - Right click Start <br>
    &nbsp &nbsp - Click System <br>
    &nbsp &nbsp - Click Rename this PC (advanced) <br>
    &nbsp &nbsp - Click Change <br>
    &nbsp &nbsp - Click Domain, then type your domain name, click OK <br>
    &nbsp &nbsp - Enter the username and password of the admin account you created previously eg mydomain.com\werner_admin <br>
    &nbsp &nbsp - The computer will restart <br> <br>  
  </p>

  <hr>

  <p>
    <img src="https://i.imgur.com/2MNTnjJ.png" height="80%" width="80%" alt="Log in as admin"/>
  </p>
  <p>
    19. Login to DC-1 and check to see if Client-1 appears in Active Directory Users and Computers (ADUC) inside the "Computers" container: <br>
    &nbsp &nbsp - Login to DC-1 <br>
    &nbsp &nbsp - Click Start <br>
    &nbsp &nbsp - Type and select Active Directory Users and Computers <br>
    &nbsp &nbsp - Click mydomain.com <br>
    &nbsp &nbsp - Click Computers <br> <br>
  </p>

  <hr>

  

  <p>
    <h3>(Setup Remote Desktop for non-administrative users on Client-1)</h3> <br>
  </p>
    
  <p>
  <img src="https://i.imgur.com/qNi9X2a.png" height="80%" width="80%" alt="Log in as admin"/>
  </p>

  <p>
    20. Log into Client-1 as (domain name\admin user) and open system properties: <br>
    &nbsp &nbsp - Open Remote desktop and log into Client-1 as (domain name\admin user) <br>
    &nbsp &nbsp - Right click the Start menu <br>
    &nbsp &nbsp - Click Systems <br>
    &nbsp &nbsp - Click Remote desktop <br>
    &nbsp &nbsp - Click Select users that can remotely access this PC <br>
    &nbsp &nbsp - Click Add <br>
    &nbsp &nbsp - Type 'domain users' <br>
    &nbsp &nbsp - Click Check Names <br>
    &nbsp &nbsp - Click OK <br> <br>
  </p>

  <hr>

  <p>
    <h3>(Create many additional users and attempt to log into Client-1 with one of the users)</h3> <br>
  </p>

  <p>
    21. Login to DC-1 as an admin account eg werner_admin <br> <br>
  </p>

  <hr>

  <p>
    22. Open PowerShell ISE as an administrator: <br>
    &nbsp &nbsp - Click Start, type PowerShell ISE <br>
    &nbsp &nbsp - Right click Windows PowerShell ISE <br>
    &nbsp &nbsp - Click Run as administrator <br> <br>
  </p>

  <hr>
  
  <p>
    23. Create a new File and paste the contents of the script into it: <br>
    &nbsp &nbsp - Click New Script <br>
    &nbsp &nbsp - Copy and paste the contents of the link below into the script <br>
    &nbsp &nbsp  https://1drv.ms/t/s!Asco8VMuK30rkCdIKbGkgFkZQSmI?e=63Zbig <br> <br> <br>
  </p>

  <hr>

  <p>
    <img src="https://i.imgur.com/ZLLSOXT.png" height="80%" width="80%" alt="Log in as admin"/>
  </p>

  <p>
    24. Run the script and observe the accounts being created <br>
    &nbsp &nbsp - Click Green button to Run script <br> <br>
  </p>

  <hr>

  <p>
    <img src="https://i.imgur.com/idcs1CJ.png" height="80%" width="80%" alt="Log in as admin"/>
  </p>

  <p>
    25. Open ADUC and observe the accounts in the _EMPLOYEES OU: <br> <br>
  </p>

  <hr>

  <p>
    <img src="https://i.imgur.com/WHvAkU5.png" height="80%" width="80%" alt="Log in as admin"/>
  </p>

  <p>
    26. Attempt to log into Client-1 with one of the accounts created from the script (make a note of the password in the script: <br>
    &nbsp &nbsp - Login in the context of a domain (domain name\account name) <br>
  </p>
    
    
    

    

