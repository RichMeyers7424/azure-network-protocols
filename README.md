<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we will observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. This will give us an understaning of how differnet traffic flows with the use of Wireshark.


- [How to Setup VMs and a Virtual Network in Azure](https://github.com/)<br />
<h1>Environments and Technologies Used</h1>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Command-Line Tools
- Network Protocols (SSH, RDP, DNS, ICMP, DHCP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>Overview</h2>

- Step 1: Use Remote Desktop to connect to your Windows 10 Virtual Machine
- Step 2: Within your Windows 10 Virtual Machine, Install Wireshark
- Step 3: Ping the Ubuntu VM to observe ICMP traffic, disable and re-enable ICMP traffic through the NSG
- Step 4: Observe SSH traffic by connecting through the command line on the Windows VM
- Step 5: Observe DNS traffic on the Windows VM
- Step 6: Observe RDP traffic on the Windows VM
- Step 7: Clean up by deleting the resource groups 

<h2>Creating your VM's</h2>


<p>
Hello, and welcome!! To get started, go to your Azure portal https://portal.azure.com and create your resource group for this lab. Inside your resource group, begin to create your VMs, one running Windows and the other running Linux. Copy the IP address of your Windows VM and remote desktop in.
</p>
<img src="https://i.imgur.com/HJ2dQ6S.png"/>
<br />

<p>
In the VM, open up microsoft edge and look up Wireshark.  Make sure you go to the acutal website and download the Wireshark x64 installer.
</p>
<img src="https://i.imgur.com/Hh8a8sq.png"/>
<br />

<p>
Now open the installer and install Wireshark to the defaults.
</p>
<img src="https://i.imgur.com/Y4HTfwF.png"/>
<br />

<p>

Once Wireshark opens, filter by "icmp" packets and hit the blue shark fin icon in the top left to start capturing traffic.
</p>
<img src="https://i.imgur.com/dqp13Gx.png"/>
</p>
<img src="https://i.imgur.com/cYHah9o.png"/>
</p>
<br />


<p>
Find your Ubuntu VM's private IP and copy it. To do so, go to the VM inside of the Azure portal. Once your in VM2, the Private IP address is under the networking section.
</p>
<img src="https://i.imgur.com/OiFbjX4.png"/>
</p>
<br />

<p>
Now go back to VM1 open PowerShell and ping -t the private IP address of the Ubuntu VM; this will make continuous ICMP traffic that will be captured by Wireshark. We should see requests from the Ubuntu VM's private IP, and replies from our Windows private IP as well.
</p>
<img src="https://i.imgur.com/nZ8HRnN.png"/>
</p>
<br />

<p>
Now we are going to disable ICMP traffic from the Ubuntu VM's NSG (network security group) in the Azure portal. To do this, look up Network Security Group in the Azure portal searchbar. Click on VM2s NSG.
</p>
<img src="https://i.imgur.com/FOkvu4G.png"/>
</p>
<br />

<p>
Once your in the NSG for VM2, click on inbound security rules.
</p>
<img src="https://i.imgur.com/43lzXLW.png"/>
<br />


<p>
Add a new rule with ICMP as the selected protocol, set the action to deny, and set the priority to any number that is lower than the first rule in the list (the lower the number, the higher the priority that the rule has) I set it to 200 as it has higher priority than 300. Now add the rule and wait for it to apply.
</p>
<img src="https://i.imgur.com/507lKpN.png"/>
</p>
<br />


<p>
Now going back into VM1, try to ping the private IP of VM2 again, now notice that our ping requests are now being timed out as we are not getting replies from VM2.
</p>
<img src="https://i.imgur.com/8qzElIZ.png"/>
</p>
<br />


<p>
So, to re-enable the ping, we will go back to the NSG for VM2, and just set the rule to allow traffic instead of deny, essencially just turning the rule off.
</p>
<img src="https://i.imgur.com/RNRP286.png"/>
</p>
<br />


<p>
Now try to ping the Ubuntu VM again using the ping command. We now should see that the Ubuntu VM is now sending replies again!
</p>
<img src="https://i.imgur.com/gu8NzIs.png"/>
</p>
<br />


<p>
Now that we have observed ICMP traffic, we will now connect to VM2 via SSH (secure shell) within the command line inside of VM1. We will also switch over from ICMP traffic to ssh traffic on Wireshark. To do this, simply click on the green bar under the sharkfin, and type ssh and hit enter.  This should switch over to only viewing ssh traffic. To log into VM2, in powershell, type ssh and the username for VM2 @ the ip address of VM2. Note that when you are typing in your password, it will not show up, just trust that you are entering the correct password. If it does fail, just keep trying until you get in.
</p>
<br />

<p>
<img src="https://i.imgur.com/gtB9imf.png"/>
</p>
<p>
Now in Wireshark filter for ssh traffic and refresh, again continue without saving.
</p>
<br />

<p>
<img src="https://i.imgur.com/gtB9imf.png"/>
</p>
<p>
In the remote SSH connection within the command line type some linux shell commands like man, ls, pwd, etc and observe the SSH traffic.
</p>
<br />

<p>
<img src="https://i.imgur.com/fvMYtrn.png"/>
</p>
<br />


<p>
Now let's observe DHCP traffic.  
In wireshark filter by dchp and refresh. In the command line type "ipconfig /renew" to issue a new IP address to the Windows VM, this will use the DHCP protocol and we will be observable in wireshark.
</p>
<img src="https://i.imgur.com/r65YEyC.png"/>
</p>
<br />  

<p>
Lets try to observe DNS traffic by typing in the command line "nslookup google.com", or filter for RDP traffic in wireshark by typing the in the filter "tcp.port == 3389" and see the constant RDP traffic as we are currently inside of a remote desktop connection.
</p>
<img src="https://i.imgur.com/6DAmtnl.png"/>
</p>
<br />  

<p>
Congrats! You have made it to the end of this lab! Now for the very last step lets clean up our Azure resources so we don't incur much costs. Just go to the Resource group in the Azure portal and delete the whole group. 
</p>
<br />


