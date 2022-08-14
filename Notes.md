# Create Resource Group
1. Create Resource Group called AD101_Lab
    a. Note that we aren't using Microsoft Azures active directory, we are telling the server to use the AD and DNS settings for the server itself

# Create Virtual Network
2. Navigate to Virtual Network Tab. 
3. Click create and call the network OnPremNetwork and select the 10.0.0.0/16 subnet range
4. In the security page, make sure everything is disabled (will add cost if endabled. Select Review and Create.

# Provision Windows Server
5. Navigate to virtual machines and select add. 
6. Make sure you have the correct resource group selected and name the virtual machine DC01. 
7. Make sure region is set to AU-EAST and select the windows 2019 server. 
8. Create your credentials and select next. 
9. In disks, select OS disk type to be standard and then select "Create and attach a new disk"
10. Click Change size and change the disk to "Standard HDD" and select 10GB.
11. In management, disable boot diagnostics as we don't need it
12. Click Review and Create

# Configure DC01
13. Once complete, click view resource and take note of the the private IP address
14. Click connect and download the RDP file. 
15. Connect to the server via RDP using the creds set when provisioning the windows server
16. Open server manager -> Tools> Computer Mangement and format partition as a MBR
17. Then click Manage and select add roles and features 
18. In Server roles, make sure to select Active Directory Domain Servers
19. In Confirmation, select restart if needed. 
20. Note this hasn't installed AD, this is just a preparation for the AD. We have just added the features needed by the AD. Once the role wizard has finished, we will need to promote the server to domain controller
21. Click Promote to domain controller
22. Select add a new forest. Call it starwars.local
23. Set forest functional level to 2016
24. Create password for DSRM, this will be needed if we want to uninstall active directory
25. As we have not set up DNS, we will receive an error. This can be ignored for now
26. For the NETBIOS name, leave it as the default
27. Change the paths to our F drive which will use the disk we created. 
28. Once the prereq check is done, intall the AD DS
Note: watch from 19:20 for DNS info

# Set Static IP for DC01
29. In the Azure console, select DC01 virtual machine and click Networking under settings. Take note of the IP address
30. Click network adapter -> ip configuration -> ip adapter then set the ip address to be static and click save

# Configure the DNS
31. Go to Virtual Network and select our onPremNetwork
32. Click DNS servers, note that the DNS servers has defaulted to Azure provided. 
33. Click Custom and put the IP address of the domain server in

# Create Windows 10 Machine
1. Create a new virtual machine with OS as Windows 10 Pro
2. Create credential and then review + create
3. Set Ip address to static, same procedure as for DC
4. RDP into machine
5. Open Settings -> Accounts -> Access work or school -> Connect - > Join this device to a local Active directory domain
6. type in starwars.local and user the admin account creds. Then click skip and restart now
7. In the DC, you should see it in Users and Computers

# Provision Ubuntu Machine
1. Create a new virtual machine with OS as Ubuntu