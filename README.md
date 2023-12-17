<p align="center">
<img src="https://i.imgur.com/Clzj7Xs.png" alt="osTicket logo"/>
</p>

<h1>Active Directory Deployment</h1>
This tutorial outlines the prerequisites and installation of the open-source help desk ticketing system osTicket.<br />


<h2>Video Demonstration</h2>

- ### [YouTube: How To Install osTicket with Prerequisites](https://www.youtube.com)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Computer)
- Remote Desktop
- Internet Information Services (IIS)

<h2>Operating Systems Used </h2>

- Windows 10</b> (21H2)

<h2>List of Prerequisites</h2>

- Item 1
- Item 2
- Item 3
- Item 4
- Item 5

<h2>Steps</h2>

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<h3>Setup Resources in Azure</h3>
  <p>
  1. Create the Domain Controller VM named DC-1: <br>
  &nbsp &nbsp - Go to portal.azure.com <br>
  &nbsp &nbsp - Virtual machines <br>
  &nbsp &nbsp - Click Create, then Azure virtual machine <br>
  &nbsp &nbsp - Create a Resource group <br>
  &nbsp &nbsp - Give the VM a name of DC-1, which stands for Domain controller <br>
  &nbsp &nbsp - Set your region <br>
  &nbsp &nbsp - Set Image to Windows Server 2022 <br>
  &nbsp &nbsp - Set Size to 2 vcpu or more <br>
  &nbsp &nbsp - Enter a username and password <br>
  &nbsp &nbsp - Click Review + create <br>
  &nbsp &nbsp - Once validation is finished click Create <br> </p>
   
 <p>
  2. Set the Domain Controller's NIC Private IP address to static: <br>
  &nbsp &nbsp - Go to Virtual machines <br>
  &nbsp &nbsp - Click DC-1 <br>
  &nbsp &nbsp - Under Settings click Networking <br>
  &nbsp &nbsp - Click the link that is next to Network Interface <br> 
  &nbsp &nbsp - Under Settings click IP configurations <br>
  &nbsp &nbsp - Click the Private IP address <br>
  &nbsp &nbsp - Change the Private IP address to Static <br>
  &nbsp &nbsp - Click Save <br> </p>

  <p>
  3. Create the Client VM:<br>
  &nbsp &nbsp - Go to Virtual machines <br>
  &nbsp &nbsp - Click Create, then Azure virtual machine <br>
  &nbsp &nbsp - Put this VM in the same resource group as DC-1 <br>
  &nbsp &nbsp - Name VM Client-1 <br>
  &nbsp &nbsp - Set the same region as DC-1 <br>
  &nbsp &nbsp - Set the Image to Windows 10 <br>
  &nbsp &nbsp - Set the username and password, you can use the same as DC-1 <br>
  &nbsp &nbsp - Click Review + create <br>
  &nbsp &nbsp - Click Create <br> </p>

  <p>
    4. Ensure both VMs are in the same Vnet:
  </p>

  <p>
    <h3>Ensure Connectivity between the client and Domain Controller</h3> 
    5. Login to Client-1 with Remote Desktop and ping DC-1's private IP address: <br>
    &nbsp &nbsp - Click Start <br>
    &nbsp &nbsp - Search Remote Desktop Connection <br>
    &nbsp &nbsp - Enter the public IP address of Client-1 <br>
    &nbsp &nbsp - Click More choices <br>
    &nbsp &nbsp - Click Use a different account <br>
    &nbsp &nbsp - Enter the username and password you created for Client-1 <br>
    &nbsp &nbsp - Go to Azure portal and copy DC-1's private IP address <br>
    &nbsp &nbsp - From Client-1 VM click Start <br>
    &nbsp &nbsp - Enter cmd, then click Open <br>
    &nbsp &nbsp - Enter ping -t (paste DC-1's private IP address) <br>
    &nbsp &nbsp - You should get 'Request timed out.' <br>  
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
    &nbsp &nbsp - Under the Protocol row look for ICMPv4 <br>
    &nbsp &nbsp - Right click and enable both 'Core Networking Diagnostics - ICMP Echo Request (ICMPv4-In) <br>
  </p>
  
   <p>
    7. Check back at Client-1 to see the ping succeed: <br>
    &nbsp &nbsp - Go back to Client-1 and check to see if the ping to DC-1 is now working <br>
    &nbsp &nbsp - Press Ctrl + C to stop the ping <br>  
  </p>

  <p>
    <h3>Install Active Directory</h3>
    8. Login to DC-1 and install Active Directory Domain Services <br>
    &nbsp &nbsp - If not already, log in to DC-1 using Remote Desktop Connection <br>
    &nbsp &nbsp - If Server Manager is not already opened, click Start and click Server Manager <br>
    &nbsp &nbsp - Click Add roles and features <br>
    &nbsp &nbsp - Click Next <br>
    &nbsp &nbsp - For Installation Type click Next <br>
    &nbsp &nbsp - For Server Selection make sure that DC-1 is highlighted then click Next <br>
    &nbsp &nbsp - For Server Roles check Active Directory Domain Services, click Add Features, then click Next <br>
    &nbsp &nbsp - For AD DS click Next <br>
    &nbsp &nbsp - Click Install <br>  
  </p>

  <p>
    9.Promote as a Domain controller <br>
    &nbsp &nbsp - Top right corner of the screen there is a yellow exclamation symbol. Click this symbol, then click Promote this server to a domain controller <br>
    &nbsp &nbsp - Click Add a new forest <br>
    &nbsp &nbsp - Add a root domain name, then click Next <br>
    &nbsp &nbsp - Add a password, then click Next <br>
    &nbsp &nbsp - At DNS Options, click Next <br>
    &nbsp &nbsp - At Additional Options, click Next <br>
    &nbsp &nbsp - Click Next until it gives you the option to Install <br> 
  </p>

  <p>
    10. Restart and then log back into DC-1 as the user (domain name\username): <br>
    &nbsp &nbsp - Restart DC-1 VM <br>
    &nbsp &nbsp - In Azure portal Refresh DC-1 <br>
    &nbsp &nbsp - Log back into DC-1 as (domain name\username) <br> 
  </p>

  <p>
    <h3>Create an Admin and Normal User Account in AD</h3>
    11. In Active Directory Users and Computers (ADUC), create an Organisational Unit (OU) called _EMPLOYEES <br>
    &nbsp &nbsp - Click Start <br>
    &nbsp &nbsp - Type and go to Active Directory Users and Computers <br>
    &nbsp &nbsp - In the domain you created <br>
    &nbsp &nbsp - Right click, New, Organizational Unit <br>
    &nbsp &nbsp - Give it a name of _EMPLOYEES <br>
  </p>

  <p> 
   12. Create an Organisational Unit named _ADMINS <br>
  </p>

  <p>
    13. Create an admin account <br>
    &nbsp &nbsp - Go to the _ADMINS OU you created <br>
    &nbsp &nbsp - Right click, New, User <br>
    &nbsp &nbsp - Give the accout a name <br>
    &nbsp &nbsp - I would recommend the login name have the word 'admin' in eg werner_admin <br>
    &nbsp &nbsp - Click Next <br>
    &nbsp &nbsp - Add a password <br>
    &nbsp &nbsp - Usually would tick 'User must change password at next login' but for now tick 'Password never expires' <br>
    &nbsp &nbsp - Click Next <br>
    &nbsp &nbsp - Click Finish <br>
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
    &nbsp &nbsp - Click Apply, then OK <br>   
  </p>

  <p>
    15. Log out the Remote Desktop connection to DC-1 and log back in as (domain name\admin account): <br>
    &nbsp &nbsp - Log out of DC-1 <br>
    &nbsp &nbsp - Bring up Remote Desktop connnection <br>
    &nbsp &nbsp - Enter DC-1's public IP address <br>
    &nbsp &nbsp - Log in with (domain name\admin account) <br>  
  </p>

  <p>
    <h3>Join Client-1 to your domain</h3>
    17. From the Azure portal, set Client-1's DNS settings to the DC's Private IP address: <br>
    &nbsp &nbsp - Go to the Azure portal <br>
    &nbsp &nbsp - Go to Virtual machines, then DC-1 <br>
    &nbsp &nbsp - Click Networking <br>
    &nbsp &nbsp - Next to NIC Private IP, copy this address <br>
    &nbsp &nbsp - Go the virtual machines, then Client-1 <br>
    &nbsp &nbsp - Click Networking <br>
    &nbsp &nbsp - Click the link next to 'Network Interface' <br>
    &nbsp &nbsp - Under Settings, click DNS Servers <br>
    &nbsp &nbsp - Click Custom <br>
    &nbsp &nbsp - Paste DC-1's private IP address under DNS server, then click Save <br>  
  </p>

  <p>
    18. From the Azure portal, restart Client-1: <br>
    &nbsp &nbsp - Go to Virtual Machines, Client-1 <br>
    &nbsp &nbsp - Click Restart <br>
  </p>

  <p> 
    19. Login to Client-1 and join it to the domain: <br>
    &nbsp &nbsp - Login to Client-1 using Remote Desktop connection <br>
    &nbsp &nbsp - Go to command line, then enter ipconfig /all <br>
    &nbsp &nbsp - Next to DNS Servers should be DC-1's IP address <br>
    &nbsp &nbsp - Right click Start <br>
    &nbsp &nbsp - Click System <br>
    &nbsp &nbsp - Click Rename this PC (advanced) <br>
    &nbsp &nbsp - Click Change <br>
    &nbsp &nbsp - Click Domain, then type your domain name, click OK <br>
    &nbsp &nbsp - Enter the username and password of the admin account you created previously eg mydomain.com\werner_admin <br>
    &nbsp &nbsp - The computer will restart <br>   
  </p>

  <p>
    <h3>Setup Remote Desktop for non-administrative users on Client-1</h3> <br>
    22. Log into Client-1 as (domain name\admin user) and open system properties: <br>
    &nbsp &nbsp - Open Remote desktop and log into Client-1 as (domain name\admin user) <br>
    &nbsp &nbsp - Right click the Start menu <br>
    &nbsp &nbsp - Click Systems <br>
    &nbsp &nbsp - Click Remote desktop <br>
    &nbsp &nbsp - Click Select users that can remotely access this PC <br>
    &nbsp &nbsp - Click Add <br>
    &nbsp &nbsp - Type 'domain users' <br>
    &nbsp &nbsp - Click Check Names <br>
    &nbsp &nbsp - Click OK <br> 
  </p>

  <p>
    <h3>Create many additional users and attempt to log into Client-1 with one of the users</h3> <br>
    27. Login to DC-1 as an admin account eg werner_admin <br>
    28. Open PowerShell ISE as an administrator: <br>
    &nbsp &nbsp - Click Start, type PowerShell ISE <br>
    &nbsp &nbsp - Right click Windows PowerShell ISE <br>
    &nbsp &nbsp - Click Run as administrator <br>
    29. Create a new File and paste the contents of the script into it: <br>
    &nbsp &nbsp  https://1drv.ms/t/s!Asco8VMuK30rkCdIKbGkgFkZQSmI?e=63Zbig <br>
    30. Run the script and observe the accounts being created <br>
    31. Open ADUC and observe the accounts in the _EMPLOYEES OU: <br>
    &nbsp &nbsp - Go to _EMPLOYEES <br>
    &nbsp &nbsp - Right click, then click Refresh <br>
    32. Attempt to log into Client-1 with one of the accounts created from the script (make a note of the password in the script: <br>
    &nbsp &nbsp - Login in the context of a domain (domain name\account name) <br>
    
    
    
  </p>

    
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />
