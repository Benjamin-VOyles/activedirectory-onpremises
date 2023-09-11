<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-Premises Active Directory Deployed Within the Cloud (Azure)</h1>
This tutorial will show how to implement Active Directory within an Azure VM.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10

<h2>Deployment and Config Steps</h2>

1. Create 2 Virtual Machines in Azure and name one as Domian Control-1(DC-1) and the other as Client-1.
2. Install Active Directory on DC-1.
3. Join Client-1 to the Main Domain.
4. Create a list of User Accounts.

![image](https://github.com/Benjamin-VOyles/activedirectory-onpremises/assets/143958072/d254c227-a827-4cd0-82f8-2cd3c99b373f)

1.Create the first VM, name it DC-1 as in Domain Controller and use Windows server 2022 (2 VCPUs or more are reccommended) and create.

2. Create another vm Naming it Client-1 making sure it is with in the same network as your first VM using Windows 10. Click Create. 

*IMPORTANT!!*

3. Make sure to set the Domain Controler VM IP address to static.

4. Go to Networking and then Network Interface, from IP Configurations change the VM IP address from dynamic to static.
 
5. Confirm both VMs have a connection.
   
6. Use Remote Desktop to login to Client-1. Then run the command prompt Ping DC-1 (Insert the Private IP Address) and then type after the Private IP Address -t for an infinite ping.
 
7. Next, log in to DC-1 with Remote Desktop and click the start button and search firewall. Open up Windows Firewall under advanced settings inbound rules and sort protocol section, search for icmpv4, allow via Enable Rule
   
8. Ping will now be able to show a connection to the Client-1 VM.

![image](https://github.com/Benjamin-VOyles/activedirectory-onpremises/assets/143958072/ed5eb233-6ce7-4a3f-9675-9566f5aca7a7)

Installing Active Directory on DC-1.

 1. Open up the DC-1 VM via Remote Desktop and then open up Service Manager and under Add Roles and Features proceed by clicking next until  clicking Active Directory (AD) Domain Services is available. Then click next several more times to install then the install will be available to install AD.
    
 2. Within the service manager, click on the yellow flag at the very top of the page to allow the VM become the Domain Controller and finish up installing AD.
    
 3. Then name the Domain Server mydomain.com and set the password (Make sure to click Remember It for personal convenience) and proceed by clicking next on all the prompts until finished.
    
 4.The computer will shut down (This is normal) and it will need to be reconnected to the DC-1 VM. Log in via the domain controller name. mydomain.com/"username" if not able log in the usual way then click on the start search Active Directory. Use Right click on mydomain.com, click new and then for organizational unit call it _EMPLOYEES and click ok. Now do the same thing making a folder for _ADMIN and refresh the directory.

 5. Under _ADMIN create a new user and name it as an admin account, make a password and be sure to uncheck change password.
    
 6. Right click the user account just made and then click on add  domain, then check names and domain admins group. Confirm with Apply and then ok.
     
 7. Now Log out of DC_1 and then log back in under the new admin account.
  
<br />

![image](https://github.com/Benjamin-VOyles/activedirectory-onpremises/assets/143958072/450a22e3-94fd-48eb-9c85-902faae840bc)

1. Within the Azure Portal under virtual machines click DC-1 and then in the networking section there is a NIC Private IP. Copy the Private IP.
   
2. Now move to the VMs and click Client-1 and under the networking section of this VM click the network interface ID and then proceed to DNS Servers on the panel to the side. Under Custom, paste the copied Private IP Address that belongs to DC-1 and click save.
   
3. Return to the VMs page in Azure and restart Client-1.
   
4. Log into Client-1 via Remote Desktop as the local admin account(The account that was made prior to the admin account).
   
5. Open up Command Prompt within Client-1 and in Remote Desktop type in ipconfig/all and DC-1 IP Address should be visible.
    
6. Then on the Client-1 VM right click on the Start Menu and under System, click Rename This PC Advanced. Click change and then Domain, type in domain.com. and click ok, a new window should appear for Computer Name/ Domain Changes. Now use the Domain Admin log in that was created the second time (NOT the local admin account).

7. The computer will to restart to join the domain.

![image](https://github.com/Benjamin-VOyles/activedirectory-onpremises/assets/143958072/6eecf96e-29d0-409d-9520-eef6bb7d9eac)

1. Log in to the Client-1 VM as the Admin account that was created for domains (NOT the local admin account).
   
2.  Then right click the Start Menu and under System, click on remote desktop and click on Select Users That Can Remotely Access This PC. Now Click on Add Type in Domain Users and then Check Names. Click ok on the following prompt.
   
3. Now within the DC-1 Start Menu type in Active Directory, select Users and Computers.
   
4. Under User Folder click Domain Users, now anyone will be able to log in to both devices.
  
5. Create additional users and log into Client-1 with a user account.
    
6.Login to DC-1 as the Domain Controler Admin account and open up PowerShell_ise as an administrator.

7.Create a brand new file and paste the following script into it (https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1)

8.Run the script, observe the accounts that are being rapidly created.

9.When finished, open up ADUC and observe the accounts within the correct OU and log into Client-1 with one of the accounts ( *IMPORTANT!!* Remember the password in the script!)


<br />
