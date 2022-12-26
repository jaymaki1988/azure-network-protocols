# azure-network-protocols
<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />


<h2>Video Demonstration</h2>

- ### [YouTube: Azure Virtual Machines, Wireshark, and Network Security Groups](https://www.youtube.com)

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

- Step 1
- Step 2
- Step 3
- Step 4

<h2>Actions and Observations</h2>

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
We are going to be setting two virtual machines and observing the traffic between them. The first thing we are goint to do is create a Resource group withing Azure. Go to Microsoft Azure and type in the search bar "Rescource Group". At the top left hit the "Create button". Give a name for the rescource group, which we will use "LAB-3" and select an area. At the bottom left, hit the "Review + Create" and wait for it to validate it. Once that is done, hit the "Create" button.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Go to the Search bar and type "virtual machinse" and select "Virtual Machines". At the top left hit the "Create" button. Make sure to select the rescource group we made. Give an name for the virtual machine. We will name ours "VM1". Select a location and the first one we will be making a Windows 10. Under "image" select Windows 10. For the size, I would recommend 2 cores and 16 GiB memory. Create a user name and password and select review and create. 
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
While that virtual machine is being created, go back to the search bar and type "Virtual Machine" and select it again. This next Virtual machine, we will be making a linux machine. Make sure to select the same Rescource group and same Region and zone as the other virtual machine. Under image, select "Ubuntu Server 20.04 LTS - x64 Gen2". I recommend selecting 2 cores with 16 GiB memory. Under "Administrator account" select "Password" Create a user name and password. I will use labuser as my username.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Click next to Disks and then Next again to Networking section. Make sure on the "Network interface" make sure it is in the same Virtual network. For example mine created a "LAB-1-vnet" Yours should be the name of the rescource group with a -vnet. Click "Review and Create". Wait for validation to pass and finally hit "Create".
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
What you can do next is type in the search bar "Network watcher" and open it. Select "Topology" on the left side of the screen. Select your rescource group you made and the vnet that we talked about in the previous section. You can see the to virtual network interface cards (vnic) are coming form the rescource group vnet. Each vnic represents a Vitual machine, The Network security group, and the public ip address. Next we will be logging into VM1.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next
</p>
<br />
Go to virtual machinse and click on "VM1". Find where it says "Public IP address" which should be on the right top. To the right of that should be a paper looking icon to copy the IP address. Click on that. Go to the search bar to the right of the start screen and type "Remote Desktop" and select it. Paste the IP address and click "Connect". Type in the username and password you created and click "Ok". Then click "Yes on the next screen. It should start loading. Deactivate the begining settings it pops up with and click "Accept". We are next going to install a program named Wire Shark to inspect traffic. 
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
In the VM1 click on Microsoft Edge. Click "Start withou my data. Type in search Wire Shark. Click on the wireshark.org. There should be a download for Windows x64 and click on that. It should start downloading it at the top right of the screen. Once it is done, click on the download. Install with all the default settings by selecting next until installed. Once it is fully installed, close Microsoft Edge and go to the Search bar near the start button and type "wireshark"and open application.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Once wireshark is open, click on the fin icon on the top left to start capturing packets. This will show all the activity on the virtual machine. Click in the search bar near the blue ribbon and type "icmp". This will now show traffic from icmp whi there shouldn't be any traffic. icmp is what track the ping command. Minimize VM1 and go to Azure. Go to Vitrual Machines and click on VM2. Under the "Networking section", copy the Private IP address. Go back to VM1 and go to Windows Search bar and search for "Powershell" and open it. 
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Once PowerShell is open. Type ping and paste or type the Private IP address from VM2. You should see a reply from VM2 and activity from Wire Shark. On Wire Shark, you shoulcd see the source from VM1's IP address to the destination, which is VM2's IP address. The next line is the reply from VM2 being the source and VM1 being the destination.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next we can type ping www.google.com into PowerShell. You should see activity coming back from google. At the bottom, you can see what google is sending us, which is a bunch of random numbers and letters. We are now going to do a propetual ping to VM2. Clear out the WireShark data by clicking the "green fin looking icon' and click "Continue without Saving". Type "ping 10.0.0.5 -t" and you should start seeing it ping forever. The "-t" just continues to ping. We are now going to change the firewall to not allow icmp traffic. Ping uses icmp and it should stop it from replying to the ping. 
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
We are going to start by minimizing VM1. Its the minus sign on that blue bar on the right. Go back to Azure portal and search for "Network Security Groups". It should bring up the security for both virtual machines. Select VM2 security. Click on inbound security rules on the left. Click on the "Add rule" button. Slect Any source, and any Destination. Under the "Destination port ranges, select the "ICMP" bubble since that is what ping uses. Under "Action" select "Deny". Priotiry is when it chooses to complete this action. As you see the other rules, they have a priority on when they happen. We want to make sure ours comes first, so change Priority to 200. Give it a name such as DENY_ICMP_FROM_ANY_DEVICE. Click the "Add" button.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Go back to VM1 and Wire Shark. You will now notice that the request is being timed out. That is because we set the security to deny ICMP requests. In the next section ww will enable ICMP to see the effect it has with VM1's pinging.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
In Azure, go back to Network Security group. Select VM2 and select "imbound security rules". Click on the new rule we made to edit it. We could also delete the rule as well and that would work too. Lets edit the rule. Click on the rule and instead of Deny we will click on the "Allow" and click save.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Now we will go back to VM1. We will observe the ping while the security rule for VM2 is being saved. As you will see, the ping switches from timing out to successfully pinging. Press "Control C" in Powershell to stop it from pinging. Press the green fin icon to clear out wire shark. We are now going to filter through SSH. 
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
In the search bar in Wire Shark, type in "ssh". We are going to log into VM2 through Power Shell. In Power Shell, type "ssh (username of vm2)@(vm2's private ip). For example, mine is "ssh labuser@10.0.0.5". You should see some traffic from Wire Shark when trying to log into VM2. It will ask if you want to continue connecting, type "yes". It will then have you enter the password to VM2. It will not show the password. If successful, it will log you into VM2, where you can try out a bunch of different commands from linux. Try id, pwd, ls -lasth. You will notice the activity in Wire Shark. To exit, just type "exit" and it will tell you the connection is closed.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Go back up to the search bar in Wire Shark and type in dhcp. DHCP is used to automatically assign a IP address. We can config the DHCP to config a new IP address. Go to PowerShell and type "ipconfig /renew. We should see some DHCP traffic in Wire Shark. The Virtual Destop connection might go out for a second.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
We will try dns traffic. Go up to the search bar in Wire Sharp and type "dns". Click on the green fin icon to clear Wire Shark. In PowerShell type "nslookup www.google.com" we should see some DNS traffic come in. It will send us some google IP addresses.  In the Wire Shark search bar, you can type "udp.port==53". Clear out the Wire SHark info and type "nslookup www.disney.com". It should display IP addresses from Disney.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lastly we will be inspecting rdp traffic. In the Wiore Shark, type in "rdp" or type in "tcp.port==3389" and we will see it spamming traffic. That's because whatever your local computer is doing, (typing, moving the mouse, and processes) are going to be causing non stop traffic. 
</p>
<br />
