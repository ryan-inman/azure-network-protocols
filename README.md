<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- Create Resources
- Install Wireshark
- Observe Multiple Network Protocols

<h2>Actions and Observations</h2>

<p>
Let's start by creating a Resource group that will hold both of our VMs. To create a resource group search for it in Azure and select "Create Resource Group". Name this resource group "RG-LAB-02" and select "Review + create".
<p align="center"><img src="https://i.imgur.com/uBBfFes.png" height="70%" width="70%" alt="create resource group"/> </p>
</p>
<br />

<p>
We will now create two VMs in the Azure Portal to send traffic between. Start by searching for "virtual machine" and selecting the "Create" option. Use the same subscription, resource group, and region for both VMs. Name the first machine as "VM1" and choose the Windows 10 operating system. Make the machine size 2 vCPUs and create a username/password for authentication purposes.
<p align="center"><img src="https://i.imgur.com/nFLwFK5.png" height="70%" width="70%" alt="create virtual machines"/> </p>
<p align="center"><img src="https://i.imgur.com/UeZOCIR.png" height="70%" width="70%" alt="create virtual machines"/> </p>
<p align="center"><img src="https://i.imgur.com/zrl7m7K.png" height="70%" width="70%" alt="create virtual machines"/> </p>
</p>
<br />

<p>
Proceed to the Networking Section and note that a Virtual Network and subnet will be created by default. When we create the second VM we will use these. Select "Review + create" and then "Create" to finish making VM1.
<p align="center"><img src="https://i.imgur.com/MTplLEc.png" height="70%" width="70%" alt="create virtual machines"/> </p>
<p align="center"><img src="https://i.imgur.com/QGjzv5P.png" height="70%" width="70%" alt="create virtual machines"/> </p>
</p>
<br />

<p>
Let's now create our 2nd VM. This VM will use the same subscription, resource group, and region. Name this machine "VM2" and choose Linux Ubuntu Server image. Make this machine size 2vCPUs and create another username/password. Select the existing Virtual network and Subnet we previously used. Select "Review + create" and then "Create" to finish creating VM2.
<p align="center"><img src="https://i.imgur.com/GKhBfvB.png" height="70%" width="70%" alt="create virtual machines"/> </p>
<p align="center"><img src="https://i.imgur.com/7EB4gdD.png" height="70%" width="70%" alt="create virtual machines"/> </p>
<p align="center"><img src="https://i.imgur.com/TYFwdb8.png" height="70%" width="70%" alt="create virtual machines"/> </p>
</p>
<br />

<p>
Log into VM1 using Remote Desktop Connection by entering the public IP address of the VM and then the username/password previously created for it.
<p align="center"><img src="https://i.imgur.com/l7QZG9g.png" height="70%" width="70%" alt="log into vm1"/> </p>
</p>
<br />

<p>
We are going to install a program called Wireshark to inspect traffic as we interact with VM2. Go to the browser in VM1 and navigate to https://www.wireshark.org/download.html. Download the Windows Installer (64-bit) and open the file. Install Wireshark with the default settings to finish installation.
<p align="center"><img src="https://i.imgur.com/sUHzl2E.png" height="70%" width="70%" alt="download and install wireshark"/> </p>
<p align="center"><img src="https://i.imgur.com/q8qq2Hp.png" height="70%" width="70%" alt="download and install wireshark"/> </p>
<p align="center"><img src="https://i.imgur.com/PsHKkBQ.png" height="70%" width="70%" alt="download and install wireshark"/> </p>
</p>
<br />

<p>
We can now open Wireshark from the start menu by searching for it. Select Ethernet and click the blue fin in the top left to begin capturing packets. Notice all the traffic that is happening in the background.
<p align="center"><img src="https://i.imgur.com/hi3prbb.png" height="70%" width="70%" alt="capture packets with wireshark"/> </p>
<p align="center"><img src="https://i.imgur.com/arTgfgP.png" height="70%" width="70%" alt="capture packets with wireshark"/> </p>
</p>
<br />

<p>
Go back to the Azure Portal and select VM2 to find it's Private IP Address. We will ping the Private IP from VM1 to test the connection between the two machines.
<p align="center"><img src="https://i.imgur.com/yNSp7tA.png" height="70%" width="70%" alt="vm2 private ip"/> </p>
</p>
<br />

<p>
Go back to VM1 and first filter by ICMP traffic in Wireshark. Additionally, open Powershell and ping the Private IP Address of VM2. Notice the filtered traffic we see in Wireshark from the VMs communicating. We can also ping other IP addresses and domains such as www.google.com.
<p align="center"><img src="https://i.imgur.com/TinUlc5.png" height="70%" width="70%" alt="filter by icmp traffic"/> </p>
<p align="center"><img src="https://i.imgur.com/0W2wdla.png" height="70%" width="70%" alt="filter by icmp traffic"/> </p>
<p align="center"><img src="https://i.imgur.com/xTJNjyo.png" height="70%" width="70%" alt="filter by icmp traffic"/> </p>
</p>
<br />

<p>
Lets initiate a perpetual ping to VM2 to continously ping it. You can do this by adding "-t" to the end of the normal ping command.
<p align="center"><img src="https://i.imgur.com/JPLK0Y5.png" height="70%" width="70%" alt="initiate perpetual ping"/> </p>
</p>
<br />

<p>
Now lets block ICMP traffic on VM2's firewall and see what happens. Go back to the Azure Portal and search for "Network security groups". Select VM2's network security group and go to Inbound security rules. Click Add to begin creating a new rule, change the Protocol to ICMP and the Action to Deny. Set the priority to a number before 300 so that the rule takes place before others. Name the rule "DENY_ICMP_PING_FROM_ANYWHERE" and finally click Add.
<p align="center"><img src="https://i.imgur.com/GAdEjSA.png" height="70%" width="70%" alt="block icmp traffic"/> </p>
<p align="center"><img src="https://i.imgur.com/zP2ykqK.png" height="70%" width="70%" alt="block icmp traffic"/> </p>
<p align="center"><img src="https://i.imgur.com/bWRo1RH.png" height="70%" width="70%" alt="block icmp traffic"/> </p>
</p>
<br />

<p>
Go back to VM1 and view that our ping request has timed out in Powershell. Also, observe in Wireshark that requests are only being shown as we are no longer receiving replies.
<p align="center"><img src="https://i.imgur.com/TFJJH2D.png" height="70%" width="70%" alt="block icmp traffic"/> </p>
</p>
<br />

<p>
Let's re-enable this rule by going back to VM2's network security group and selecting Allow under the rule. We can now see in VM1 that our ping request is no longer time out in Powershell. Wireshark is also showing that we are receiving replies again.
<p align="center"><img src="https://i.imgur.com/uZV40uD.png" height="70%" width="70%" alt="enable icmp traffic"/> </p>
<p align="center"><img src="https://i.imgur.com/jMISTOu.png" height="70%" width="70%" alt="enable icmp traffic"/> </p>
</p>
<br />

<p>
Now we will observe SSH traffic in Wireshark. Filter by SSH in Wireshark and open up Powershell. In Powershell enter the login credentials for the Linux Ubuntu Server (VM2) using the format "ssh username@ip address". Next type yes and enter the password for VM2. Note that the password will not show up on your screen. Notice the SSH traffic we are receiving in Wireshark. To close the SSH connection type exit.
<p align="center"><img src="https://i.imgur.com/cFXU1E2.png" height="70%" width="70%" alt="observe ssh traffic"/> </p>
</p>
<br />

<p>
Next we will observe DHCP traffic in Wireshark. Start by filtering for DHCP in Wireshark and then go back to Powershell. In Powershell type "ipconfig /renew" and view the traffic in Wireshark.
<p align="center"><img src="https://i.imgur.com/JF9US5t.png" height="70%" width="70%" alt="observe dhcp traffic"/> </p>
</p>
<br />

<p>
To observe DNS traffic in Wireshark, filter by DNS or the port (udp.port == 53) and use the nslookup command in Powershell. Type nslookup www.google.com and observe the new traffic.
<p align="center"><img src="https://i.imgur.com/SmoRTbr.png" height="70%" width="70%" alt="observe dns traffic"/> </p>
</p>
<br />

<p>
Lastly, we will observe RDP traffic. Filter by the port (tcp.port == 3389) and observe how it is spamming nonstop. This is because there is a live RDP session from your machine to VM1!
<p align="center"><img src="https://i.imgur.com/Pp7WGHu.png" height="70%" width="70%" alt="observe rdp traffic"/> </p>
</p>
<br />

<p>
Congratulations on completing the lab! We have successfully observed various network traffic to and from Azure Virtual Machines using Wireshark and experimented with Network Security Groups!
</p>
<br />
