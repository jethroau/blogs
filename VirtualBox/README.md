# Create a VM with CentOS and enable Internet connection.

## Install CentOS 7 
Click "New" to create a Virtual machines   
![installation](img/Snipaste_1.png)

Click "Start" to install  
![installation](img/Snipaste_1a.png)

Open chrome to download CentOS 7 Minimal ISO file from [https://www.centos.org/download/](https://www.centos.org/download/)  
![installation](img/Snipaste_4.png)

Client "folder" button   
![installation](img/Snipaste_2.png)

Select ISO file path   
![installation](img/Snipaste_3.png)

Click "Installation Destination"  
![installation](img/Snipaste_5.png)

Click "Done" and confirm the destination VDI you created in previous steps during a VM creation.   
![installation](img/Snipaste_6.png)

Enter your root password  
![installation](img/Snipaste_7.png)

Click "Reboot" after installation is completed  
![installation](img/Snipaste_8.png)

Installation is successful when you see below screen. 
![installation](img/Snipaste_9.png)

## disable firewall servcie 
```
systemctl stop firewalld
reboot
```

## Network adaptor setting to bridge (using router to connect internet)
Select "bridge net-card" option to get another IP from DHCP server. 
![use bridge](img/Snipaste_2018-07-09_00-02-34.png)


## Set Host-only network adapter (using direct Lan to connect internet)

Click "Host-Network-Manager" and create a new Host-Only Network adapter for your PC
![alt text](img/vb-network0.png)

Open your VM setting, enable netowrk connection to "Host-only" and select the adapter you created at previous step
![alt text](img/vb-network1.png)

Open "PC netowrk & internet" setting and right click ethernet icon
![alt text](img/vb-network2.png) 

change to share tab and enable Internet share open and select the VB network adapter 
![alt text](img/vb-network3.png)
