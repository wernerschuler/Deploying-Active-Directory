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
  
   <p>
     <img src="https://i.imgur.com/noj8rzI.png" height="80%" width="80%" alt="Creating Client"/>
   </p> 

  <p>
    6. Login to the Domain Controller and enable ICMPv4 in the local windows Firewall: <br>
    &nbsp &nbsp - Go to the Azure portal <br>
    &nbsp &nbsp - Copy DC-1's public IP address <br>
    &nbsp &nbsp - Open another instance of Remote Desktop Connection <br>
    &nbsp &nbsp - Paste DC-1's IP address and click Connect <br>
    &nbsp &nbsp - Click More choices <br>
    &nbsp &nbsp - Click Use a different account <br>
    &nbsp &nbsp - Enter the username and password you created for DC-1 <br>
    &nbsp &nbsp - Click Start <br>
    &nbsp &nbsp - Type firewall, then select Windows Defender Firewall with Advanced Security <br>
    &nbsp &nbsp - Click Inbound Rules <br>
    &nbsp &nbsp - Click Protocol <br>
    &nbsp &nbsp - Under Protocol look for ICMPv4 <br>
    &nbsp &nbsp - Right click and enable 'Core Networking Diagnostics - ICMP Echo Request (ICMPv4-In) <br> <br>
  </p>

  <hr>
  
   <p>
    7. Check back at Client-1 to see the ping succeed: <br>
    &nbsp &nbsp - Go back to Client-1 and check to see if the ping to DC-1 is now working <br>
    &nbsp &nbsp - Press Ctrl + C to stop the ping <br>  <br>
  </p>

  <hr>

  <h3>(Install Active Directory)</h3>
  
  <p>
    <img src="https://i.imgur.com/iC8sgPC.png" height="80%" width="80%" alt="Creating Client"/>
  </p>  

  <p>
    8. Login to DC-1 and install Active Directory Domain Services: <br>
    &nbsp &nbsp - If not already, login to DC-1 using Remote Desktop Connection <br>
    &nbsp &nbsp - If Server Manager is not already opened, click Start and click Server Manager <br>
    &nbsp &nbsp - Click Add roles and features <br>
    &nbsp &nbsp - Click Next <br>
    &nbsp &nbsp - For Installation Type click Next <br>
    &nbsp &nbsp - For Server Selection make sure that DC-1 is highlighted then click Next <br>
    &nbsp &nbsp - For Server Roles check Active Directory Domain Services, click Add Features, then click Next <br>
    &nbsp &nbsp - For AD DS click Next <br>
    &nbsp &nbsp - Click Install <br>  <br>
  </p>

  <hr>

  <p>
    9. Promote as a Domain controller: <br>
    &nbsp &nbsp - Top right corner of the screen there is a yellow exclamation symbol. Click this symbol, then click Promote this server to a domain controller <br>
    &nbsp &nbsp - Click Add a new forest <br>
    &nbsp &nbsp - Add a root domain name, then click Next <br>
    &nbsp &nbsp - Add a password, then click Next <br>
    &nbsp &nbsp - At DNS Options, click Next <br>
    &nbsp &nbsp - At Additional Options, click Next <br>
    &nbsp &nbsp - Click Next until it gives you the option to Install <br> 
  </p> <br>

  <hr>

  <p>
    10. Restart and then log back into DC-1 as the user (domain name\username): <br>
    &nbsp &nbsp - Restart DC-1 VM <br>
    &nbsp &nbsp - In Azure portal Refresh DC-1 <br>
    &nbsp &nbsp - Log back into DC-1 as (domain name\username) <br> <br>
  </p>

  <hr>

  <p>
    <h3>(Create an Admin and Normal User Account in AD)</h3>
    11. In Active Directory Users and Computers (ADUC), create an Organisational Unit (OU) called _EMPLOYEES <br>
    &nbsp &nbsp - In DC-1 <br>
    &nbsp &nbsp - Click Start <br>
    &nbsp &nbsp - Type and go to Active Directory Users and Computers <br>
    &nbsp &nbsp - In the domain you created <br>
    &nbsp &nbsp - Right click, New, Organizational Unit <br>
    &nbsp &nbsp - Give it a name of _EMPLOYEES <br> <br>
  </p>

  <hr>

  <p>
    <img src="https://i.imgur.com/Cajormq.png" height="80%" width="80%" alt="Creating OU"/>
  </p> 

  <p> 
   12. Create an Organisational Unit named _ADMINS <br> <br>
  </p>

  <hr>

  <p>
    13. Create an admin account: <br>
    &nbsp &nbsp - Go to the _ADMINS OU you created <br>
    &nbsp &nbsp - Right click, New, User <br>
    &nbsp &nbsp - Give the account a name <br>
    &nbsp &nbsp - I would recommend the login name have the word 'admin' in eg werner_admin <br>
    &nbsp &nbsp - Click Next <br>
    &nbsp &nbsp - Add a password <br>
    &nbsp &nbsp - Usually would tick 'User must change password at next login' but for now tick 'Password never expires' <br>
    &nbsp &nbsp - Click Next <br>
    &nbsp &nbsp - Click Finish <br> <br>
  </p>

  <hr>
  
  <p>
   <img src="https://i.imgur.com/jGdK7oP.png" height="80%" width="80%" alt="Creating 
    OU"/> <br>
  </p> 

  <p>
    14. Add the account you made previously to the Domain Admins Security Group: <br>
    &nbsp &nbsp - Right click the account you made<br>
    &nbsp &nbsp - Click Properties <br>
    &nbsp &nbsp - Go to Member of <br>
    &nbsp &nbsp - Click Add <br>
    &nbsp &nbsp - Type domain admins, then click Check Names <br>
    &nbsp &nbsp - Click Domain Admins <br>
    &nbsp &nbsp - Click OK <br>
    &nbsp &nbsp - Click Apply, then OK <br> <br> 
  </p>

  <hr>

   <p>
   <img src="https://i.imgur.com/Ac7IbTV.png" height="80%" width="80%" alt="Log in as admin"/>
   </p>

  <p>
    15. Log out the Remote Desktop connection to DC-1 and log back in as (domain name\admin account): <br>
    &nbsp &nbsp - Log out of DC-1 <br>
    &nbsp &nbsp - Bring up Remote Desktop connnection <br>
    &nbsp &nbsp - Enter DC-1's public IP address <br>
    &nbsp &nbsp - Login with (domain name\admin account) <br>  <br>
  </p>

  <hr>

  <h3>(Join Client-1 to your domain)</h3>
  
  <p>
   <img src="https://i.imgur.com/ezOqQxR.png" height="80%" width="80%" alt="Log in as admin"/> <br> <br>
  </p>

 <p>
  <img src="https://i.imgur.com/rBQmVfm.png" height="80%" width="80%" alt="Log in as admin"/>
 </p>

  <p>
    16. From the Azure portal, set Client-1's DNS settings to the DC's Private IP address: <br>
    &nbsp &nbsp - Go to the Azure portal <br>
    &nbsp &nbsp - Go to Virtual machines, then DC-1 <br>
    &nbsp &nbsp - Click Networking <br>
    &nbsp &nbsp - Next to NIC Private IP, copy this address <br>
    &nbsp &nbsp - Go the virtual machines, then Client-1 <br>
    &nbsp &nbsp - Click Networking <br>
    &nbsp &nbsp - Click the link next to 'Network Interface' <br>
    &nbsp &nbsp - Under Settings, click DNS Servers <br>
    &nbsp &nbsp - Click Custom <br>
    &nbsp &nbsp - Paste DC-1's private IP address under DNS server, then click Save <br> <br>
  </p>

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
    
    
    

    

