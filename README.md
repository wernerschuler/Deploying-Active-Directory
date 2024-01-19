<p align="center">
<img src="https://i.imgur.com/iNhknBe.png" alt="Active Directory logo"/>
</p>

<h1>Active Directory Deployment in the Cloud (Azure)</h1>

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Computer)
- Remote Desktop
- Active Directory Domain Services
- PowerShell
- Command Prompt 

<h2>Operating Systems Used </h2>

- Windows 10</b> (21H2)
- Windows Server 2022 

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

  <img src="https://i.imgur.com/Vg5blOJ.png" height="50%" width="50%" alt="Creating Domain controller in Azure"/>
    
  - Click Next until you get to Networking page --> Check that a Virtual Network has been created and make a note of the name of it
  - In the example below a Virtual network called 'DC-1-vnet' is created

  <img src="https://i.imgur.com/eyUSnYX.png" height="50%" width="50%" alt="Creating Domain controller in Azure"/>
    
  - Click Review + create --> Once validation has passed click Create 
  - Wait until deployment is complete, then move on to the next step
      
<br> 
 
  **2. Set DC-1's NIC Private IP address to static:** 
  - Go to Virtual machines --> DC-1 --> Under Settings click Networking --> Click the link that is next to Network Interface (Image below)

    <img src="https://i.imgur.com/eu2h84a.png" height="70%" width="70%" alt="Network interface screen"/>
     
  - Under Settings click IP configurations --> ipconfig1 --> Under Private IP address settings, select Static --> Save

    <img src="https://i.imgur.com/RucRQvC.png" height="70%" width="70%" alt="Change IP address to static"/>
  

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

  <img src="https://i.imgur.com/oHVilFE.png" height="50%" width="50%" alt="Virtual network screen"/>
  
  - Review + create
  - Once validation has passed --> Create

  
  **4. Check that DC-1 and Client-1 are in the same virtual network:**
   - Go to Virtual machines --> DC-1 --> Look under 'Virtual network/subnet' --> Client-1 --> Look under 'Virtual network/subnet'
   <img src="https://i.imgur.com/g3AILcD.png" height="80%" width="80%" alt="Virtual network screen"/>
    
  
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

**16. From the Azure portal, set Client-1's DNS settings to the DC-1's Private IP address:**
 - Go to the Azure portal --> Virtual machines --> DC-1 --> Networking --> Next to NIC Private IP, copy this address

  <img src="https://i.imgur.com/HEPUjd1.png" height="70%" width="70%" alt="Creating Client"/>

  - Go to Virtual machines --> Client-1 --> Networking --> Click the link next to 'Network Interface'

  <img src="https://i.imgur.com/rz9UYIQ.png" height="70%" width="70%" alt="Creating Client"/>

  - Under Settings, click DNS Servers --> Custom --> Paste DC-1's private IP address under DNS server --> Save

  <img src="https://i.imgur.com/9LlaVaj.png" height="70%" width="70%" alt="Creating Client"/>
    
  **17. From the Azure portal, restart Client-1:**
   - Go to Virtual Machines, Client-1 --> Restart
  
  **18. Login to Client-1 and join it to the domain:**
   - Login to Client-1 using Remote Desktop connection
   - Start --> cmd --> ipconfig /all
   - Next to DNS Servers should be DC-1's private IP address
    
     <img src="https://i.imgur.com/3DhboEO.png" height="70%" width="70%" alt="Creating Client"/>

   - Right click Start --> System --> Rename this PC (advanced) --> Change --> Domain --> Type your domain name --> OK
   - Enter the username and password of your admin account
     - For example: mydomain.com\werner_admin

<img src="https://i.imgur.com/0m2JQSF.png" height="40%" width="50%" alt="Creating Client"/>

<img src="https://i.imgur.com/HKDeXoE.png" height="40%" width="50%" alt="Creating Client"/>

<img src="https://i.imgur.com/lVx7bAt.png" height="40%" width="50%" alt="Creating Client"/>
     
    
  **19. Login to DC-1 and check to see if Client-1 appears in Active Directory Users and Computers (ADUC) inside the "Computers" container:**
   - Go to DC-1
   - Start --> Active Directory Users and Computers --> select your domain --> Computers

<img src="https://i.imgur.com/Ez8b7Rk.png" height="40%" width="50%" alt="Creating Client"/>
  
 **20. Login to Client-1 as (domain name\admin user) and open system properties:**
  - Login to Client-1 as (domain name\admin user) using Remote desktop connection
  - Right click Start --> System --> Remote desktop --> Select users that can remotely access this PC
  - Add --> Type 'domain users' --> Check Names --> OK
  
  <img src="https://i.imgur.com/qNi9X2a.png" height="65%" width="65%" alt="Log in as admin"/>
  
Create many additional users and attempt to log into Client-1 with one of the users
-- 
 
**21. Login to DC-1 as an admin account**
  
**22. Open PowerShell ISE as an administrator:**
 - Start --> Type 'Powershell ISE' --> Right click --> Run as administrator

**23. Create a new File and paste the contents of the script into it:**
 - Click New Script (White symbol in the top left hand corner)
 - Copy and paste the contents of this link into the script: https://1drv.ms/t/s!Asco8VMuK30rkCdIKbGkgFkZQSmI?e=63Zbig 

**24. Run the script and observe the accounts being created:**
 - Click the Green arrow to run the script (image below)

<img src="https://i.imgur.com/OzUBskO.png" height="60%" width="60%" alt="Log in as admin"/>
  
**25. Open ADUC and observe the accounts in the _EMPLOYEES OU:**
    
<img src="https://i.imgur.com/idcs1CJ.png" height="60%" width="60%" alt="Log in as admin"/>

**26. Login to Client-1 with one of the accounts created from the script:**
 - **Note:** Password is Password1

<img src="https://i.imgur.com/WHvAkU5.png" height="60%" width="60%" alt="Log in as admin"/>
   
  

  

  
  

  
    
    

    

