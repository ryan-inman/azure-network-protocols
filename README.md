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
</p>
<br />

<p>
We will now create two VMs in the Azure Portal to send traffic between. Start by searching for "virtual machine" and selecting the "Create" option. Use the same subscription, resource group, and region for both VMs. Name the first machine as "VM1" and choose the Windows 10 operating system. Make the machine size 2 vCPUs and create a username/password for authentication purposes.
</p>
<br />

<p>
Proceed to the Networking Section and note that a Virtual Network and subnet will be created by default. When we create the second VM we will use these. Select "Review + create" and then "Create" to finish making VM1.
</p>
<br />

<p>
Let's now create our 2nd VM. This VM will use the same subscription, resource group, and region. Name this machine "VM2" and choose Linux Ubuntu Server image. Make this machine size 2vCPUs and create another username/password. Select the existing Virtual network and Subnet we previously used. Select "Review + create" and then "Create" to finish creating VM2.
</p>
<br />

<p>
Login to VM1 using Remote Desktop Connection by entering the public IP address of the VM and then the username/password previously created for it.
</p>
<br />

<p>
We are going to install a program called Wireshark to inspect traffic as we interact with VM2. Go to the browser in VM1 and navigate to https://www.wireshark.org/download.html. Download the Windows Installer (64-bit) and open the file. Install Wireshark with the default settings to finish installation.
</p>
<br />

<p>
We can now open Wireshark from the start menu by searching for it. Select Ethernet and click the blue fin in the top left to begin capturing packets. Notice all the traffic that is happening in the background.
</p>
<br />

<p>
Go back to the Azure Portal and select VM2 to find it's Private IP Address. We will ping the Private IP from VM1 to test the connection between the two machines.
</p>
<br />

<p>
Go back to VM1 and first filter by ICMP traffic in Wireshark. Additionally, open Powershell and ping the Private IP Address of VM2. Notice the filtered traffic we see in Wireshark from the VMs communicating. We can also ping other IP addresses and domains such as www.google.com.
</p>
<br />

<p>
Lets initiate a perpetual ping to VM2 to continously ping it. You can do this by adding "-t" to the end of the normal ping command.
</p>
<br />

<p>
Now lets block ICMP traffic on VM2's firewall and see what happens. Go back to the Azure Portal and search for "Network security groups". Select VM2's network security group and go to Inbound security rules. Click Add to begin creating a new rule, change the Protocol to ICMP and the Action to Deny. Set the priority to a number before 300 so that the rule takes place before others. Name the rule "DENY_ICMP_PING_FROM_ANYWHERE" and finally click Add.
</p>
<br />

<p>
Go back to VM1 and view that our ping request has timed out in Powershell. Also, observe in Wireshark that requests are only being shown as we are no longer receiving replies.
</p>
<br />

<p>
Let's re-enable this rule by going back to VM2's network security group and selecting Allow under the rule. We can now see in VM1 that our ping request is no longer time out in Powershell. Wireshark is also showing that we are receiving replies again.
</p>
<br />

<p>
Now we will observe SSH traffic in Wireshark. Filter by SSH in Wireshark and open up Powershell. In Powershell enter the login credentials for the Linux Ubuntu Server (VM2) using the format "ssh username@ip address". Next type yes and enter the password for VM2. Note that the password will not show up on your screen. Notice the SSH traffic we are receiving in Wireshark. To close the SSH connection type exit.
</p>
<br />

<p>
Next we will observe DHCP traffic in Wireshark. Start by filtering for DHCP in Wireshark and then go back to Powershell. In Powershell type "ipconfig /renew" and view the traffic in Wireshark.
</p>
<br />

<p>
To observe DNS traffic in Wireshark, filter by DNS or the port (udp.port == 53) and use the nslookup command in Powershell. Type nslookup www.google.com and observe the new traffic.
</p>
<br />

<p>
Lastly, we will observe RDP traffic. Filter by RDP and observe how it is spamming nonstop. This is because there is a live RDP session from your machine to VM1!
</p>
<br />

<p>
Congratulations on completing the lab! We have successfully observed various network traffic to and from Azure Virtual Machines using Wireshark and experimented with Network Security Groups!
</p>
<br />
