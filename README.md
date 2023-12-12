<p align="center">
<img src="https://i.imgur.com/Clzj7Xs.png" alt="osTicket logo"/>
</p>

<h1>Active Directory Deployment</h1>
This tutorial outlines the prerequisites and installation of the open-source help desk ticketing system osTicket.<br />


<h2>Video Demonstration</h2>

- ### [YouTube: How To Install osTicket with Prerequisites](https://www.youtube.com)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
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
    &nbsp &nbsp - If not already, log in to DC-1 VM using Remote Desktop Connection <br>
    
    
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
