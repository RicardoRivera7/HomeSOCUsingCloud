<h1>Home SOC with Cloud (Azure)</h1>


<h2>Description</h2>
This is a guide to build a security operations center on the cloud utilizing Microsoft Azure. We will be building an attack surface with a honeypot, SIEM, and logging those attack attempts
<br />

<h2>Prerequisites</h2>
- <b>None</b> 

<h2>Software Used</h2>

- <b>Microsoft Azure (using free subscription) </b> 

<h2>Environments Used </h2>

- <b>Microsoft</b>

<h2>Links</h2>

-[Azure Free Subscription](https://azure.microsoft.com/en-us/pricing/purchase-options/azure-account)
<br/>
-[Azure Virtual Machines](https://portal.azure.com)
<br/>

<h2>Downloads</h2>

-[Geographic IP Information](https://drive.google.com/file/d/13EfjM_4BohrmaxqXZLB5VUBIz2sv9Siz/view?usp=sharing)
<br/>
-[Attack Map](https://drive.google.com/file/d/1ErlVEK5cQjpGyOcu4T02xYy7F31dWuir/view?usp=drive_link)
<br/>





<h2>Linux/Windows Machine Setup:</h2>

<p align="left">
Download Virtual box and an ISO of your choice <br/> 
Go through the Virtualbox installation Wizard (you can choose all default options): <br/>
To begin creating a new machine click on the New button 
<img src="https://i.imgur.com/i4ayIkD.png" height="80%" width="80%" alt="Firewall Steps"/>
<br />
<br />
Enter any name you want for example if you're using the ubuntu iso type in ubuntu  <br/>
In the iso image section select where you downloaded your iso <br/>
<img src="https://i.imgur.com/p3QIkZL.png" height="80%" width="80%" alt="Firewall Steps"/> 
<br/>
<br/>
In the hardware tab make sure to allocate at least 1gb of memory (1000 mb) and as many processors as you want <br/>
<img src="https://i.imgur.com/gfkJxew.png" height="80%" width="80%" alt="Firewall Steps"/> 
<br/>
<br/>
In the hard disk tab allocate at least 99gb and click finish (this will launch the Ubuntu installation) <br/>
<img src="https://i.imgur.com/PZ3MNbx.png" height="80%" width="80%" alt="Firewall Steps"/> 
<br />
<br />
Once done login to your virtual machine (default password is changeme if you put a password on) <br/>
<img src="https://i.imgur.com/fncBWS0.png" height="80%" width="80%" alt="Firewall Steps"/>
<br/>
<br/>
  
<h2>Security Onion IDS installation:</h2>

<p align="left">
Download the Security Onion ISO and follow the steps from the previous section to create a new machine <br/>
Once you click finish this will begin the installation process but will be aborted <br/> 
<img src="https://i.imgur.com/4lvnJMo.png" height="80%" width="80%" alt="Firewall Steps"/>
<br />
<br />
We will need to mount the iso to the machines storage: <br/>
Select the machine and click settings <br/>
Click on the storage tab <br/>
Under the Controller: IDE section click on the disk that says unattended <br/>
On the line that says Optical drive click on the disk with an arrow icon and select your security onion iso <br/> 
<img src="https://i.imgur.com/DuVt0px.png" height="80%" width="80%" alt="Firewall Steps"/>
<br />
<br />
Click the start button to begin the installation <br/>
It will prompt you to type yes if you want to continue, type yes and enter any admin name and password you want <br/>
Wait for the installation process to complete then login with the admin name and password you created <br/>
<img src="https://i.imgur.com/6kqS24I.png" height="80%" width="80%" alt="Firewall Steps"/>
<br />
<img src="https://i.imgur.com/1tYim7v.png" height="80%" width="80%" alt="Firewall Steps"/>
<br />
<br />
When prompted for which installation you want, choose the EVAL one <br/>
Select the Standard Option for the node (if you get an error saying you need more NICs just add another adpater to the machine in the network tab) <br/>
Enter Any Hostname you would like for the device or leave it at default <br/>
<img src="https://i.imgur.com/v52nmHP.png" height="80%" width="80%" alt="Firewall Steps"/>
<br />
<br />
Choose whichever adapter you would like to use as your management or main one <br/>
Select a static IP so that you can manage it easier <br/>
Enter an IP that you would like, or if you already have a firewall setup then choose an IP thats within the LANs domain (EX: LAN = 192.168.1.1/24 then SecurityOnion = 192.168.1.2/24) <br/>
<img src="https://i.imgur.com/LGefq8P.png" height="80%" width="80%" alt="Firewall Steps"/>
<br />
<br />
In the next step enter the IP that gives you access to the internet or firewall (EX: 192.168.1.1)  <br/>
For the DNS servers enter this: 8.8.8.8,8.8.4.4 <br/>
If you encounter an error on the next step about the IP being routed then go to your network tab and change the second adapter to be enabled be not attached <br/>
Select Direct <br/
<img src="https://i.imgur.com/jHl1PgF.png" height="80%" width="80%" alt="Firewall Steps"/>
<br />
<img src="https://i.imgur.com/kAZ92rm.png" height="80%" width="80%" alt="Firewall Steps"/>
<br />
<br />
When prompted to enter an email note that this does not have to be a vaild email address. Make anything up (example: analyst@acme.com)  <br/>
Enter any password <br/>
<img src="https://i.imgur.com/l0Oa74S.png" height="80%" width="80%" alt="Firewall Steps"/> 
<br/>
<br/>
Here we will choose to access the web interface via IP but you can choose any of the other options if those suit you more <br/>
Click Yes <br/>
Enter the IP or subnet that allows you online (EX: 192.168.1.1/24) <br/>
<img src="https://i.imgur.com/aerxhEy.png" height="80%" width="80%" alt="Firewall Steps"/> 
<br/>
<br/>
If you want to allow SOC Telemetry just so that it helps their analytics click yes, this wont affect you so this one is completely optional if you want it on (I clicked yes) <br/>
<br/>
Open a web browser and type in the LAN ip, you will get a warning since there is no certificate, you can just ignore that an advance <br/>
This is the final box, if everything looks right to you click yes and it will finish the installation <br/>
And don't worry if it seems like its stuck or taking awhile, leave it be and it should show progress in about 5 minutes <br/>
<img src="https://i.imgur.com/4YmWnKY.pngg" height="80%" width="80%" alt="Firewall Steps"/> 


  
</p>
