<h1>Home SOC with Cloud (Azure) and Microsoft Sentinel</h1>


<h2>Description</h2>
This is a guide to build a security operations center on the cloud utilizing Microsoft Azure. We will be building an attack surface with utilzing Microsoft Sentinel as the SIEM, and logging and analyzing those attack attempts! I also show how to create Alerts and how to test them using Sentinel.
<br />

<h2>Prerequisites</h2>
- <b>Make a Microsoft Azure Account: https://azure.microsoft.com/en-us/pricing/purchase-options/azure-account </b> 

<h2>Software Used</h2>

- <b>Microsoft Azure (using free subscription) </b> 

<h2>Environments Used </h2>

- <b>Microsoft Azure</b>
- <b>Microsoft Sentinel</b>
- <b>Windows 10 VM Image</b>

<h2>Links</h2>

-[Azure Free Subscription](https://azure.microsoft.com/en-us/pricing/purchase-options/azure-account)
<br/>
-[Azure Virtual Machines](https://portal.azure.com)
<br/>
<br/>
<br/>

<!-- <h2>Downloads</h2>

-[Geographic IP Information](https://drive.google.com/file/d/13EfjM_4BohrmaxqXZLB5VUBIz2sv9Siz/view?usp=sharing)
<br/>
-[Attack Map](https://drive.google.com/file/d/1ErlVEK5cQjpGyOcu4T02xYy7F31dWuir/view?usp=drive_link)
<br/> -->



<h1>Azure Virtual Machine Setup:</h1>

<p align="left">

<h2>Creating a Resource Group</h2>

Go to the Azure portal at the homepage and click on "Resource Groups" <br/>
Click "Create" <br/>
If you're using a Free account then the Subscription section will be something like "Azure Subscription 1", it should already be selected for you <br/>
For "Resource Group Name", name it whatever you like, I named mine "SOCLab" <br/>
For "Region" select something near you or that is in a reasonable distance that wont cause connection issues, and keep in mind which one you choose as we'll need this in the future too <br/>
<img src="https://i.imgur.com/oKKF1z5.png" height="80%" width="80%" alt="AzureCloudSOC"/>
<br/>
<br/>

Click the blue "Review + Create" button <br/>
Then click the blue "Create" button <br/>
Refresh the page and you should see your resource group there! <br/>
<em>Note: don't mind the other resource groups you see in my screenshot, those were just for fun, you will only have the one you made</em> <br/>
<img src="https://i.imgur.com/78dmS0D.png" height="80%" width="80%" alt="AzureCloudSOC"/>
<br/>
<br/>

<h2>Creating a Vritual Network</h2>

Now we need to create a virtual network for our VM to connect to once its made <br/>
Navigate back to the home page and click on "Virtual Networks" <br/>
Do the Following: <br/>
<em>For "Subscription", make sure it is set to your correct one, in my case its "Azure Subscription 1" <br/>
For "Resource Group", select the one you made in the previous section, for me it was "SOCLab" <br/>
For "Virtual Network Name", make it any name you want <br/>
For "Region", select the same one you used to make the resource group, mine was US West 2 <br/> </em>
<img src="https://i.imgur.com/IgnjHtg.png" height="80%" width="80%" alt="AzureCloudSOC"/>
<br/>
<br/>

Click the blue "Review + Create" button <br/>
Then click the blue "Create" button <br/>
You should get a depployment page and confirmation it has been deployed, this could take a minute <br/>
<img src="https://i.imgur.com/Uo4W8dc.png" height="80%" width="80%" alt="AzureCloudSOC"/>
<br/>
<br/>

<h2>Creating a Virtual Machine</h2>

It's time to create the virtual machine! <br/>
Navigate back to the home page and click on "Virtual Machines" <br/>
Click "Create" and select the "Virtual Machine" option <br/>
<img src="https://i.imgur.com/5rpizrH.png" height="80%" width="80%" alt="AzureCloudSOC"/>
<br/>
<br/>

Do the Following:
<em>For "Resource Group", select the one you made <br/>
For "Virtual Machine Name", name it whatever you want, I did BlueTeamMachine <br/>
For "Region", select the same one you've been using so far <br/>
For "Zone Options", you can select "Azure-Selected Zone" to let it autopick for you, otherwise you can do "Self-Selected" zone to pick yourself <br/>
You can leave "Security Type" on "Trusted launch Virtual machines" <br/>
<img src="https://i.imgur.com/gft0rYA.png" height="80%" width="80%" alt="AzureCloudSOC"/>
<br/>
<br/>

For "Image" im picking windows 10, to do this click on the dropdown next to image and scroll all the way down to click on "See all images" <br/>
Search for "Windows 10" if you don't see it on the front page <br/>
Select it and choose any of the Gen 2 images <br/>
<img src="https://i.imgur.com/KidmTj7.png" height="80%" width="80%" alt="AzureCloudSOC"/>
<br/>
<br/>

For "Username" and "Password" you can set those to whatever you want <br/>
Make sure "Public Inbound Ports" has the "Allowed Selected Ports" option chosen <br/>
Make sure "Select Inbound Ports" has RDP (Remote Desktop Protocol) 3389 on <br/>
Under "Licensing" check the box </em><br/>
<img src="https://i.imgur.com/3g1yKSm.png" height="80%" width="80%" alt="AzureCloudSOC"/>
<br/>
<br/>

Click "Next" until you get to the "Netowrking section" <br/>
Find the checkbox for "Delete public IP and NIC when VM is deleted" and check it, this will make sure your VM and anything associated with it is completley wiped if you delete it <br/>

<img src="https://i.imgur.com/3g1yKSm.png" height="80%" width="80%" alt="AzureCloudSOC"/>
<br/>
<br/>

Click the blue "Review + Create" button <br/>
Then click the blue "Create" button <br/>
You should get a depployment page and confirmation it has been deployed, this could take a minute <br/>
<img src="https://i.imgur.com/m78nNGU.png" height="80%" width="80%" alt="AzureCloudSOC"/>
<br/>
<br/>

<h2>Creating and Configuring Log Analytics Workspace</h2>

Navigate back to the home page <br/>
Under "Azure Services", click on the arrow that says "More Services" <br/>
In the search bar type in "Log" and select "Log Analytics Workspaces" <br/>
<img src="https://i.imgur.com/yTUhEM6.png" height="80%" width="80%" alt="AzureCloudSOC"/>
<br/>
<br/>

Click "Create" <br/>
Do the following: <br/>
<em>For "Resource Group", select the one you've been using so far <br/>
For "Name", name it whatever you want <br/>
For "Region", select the region you've been using so far </em><br/>
<img src="https://i.imgur.com/jrGo9pj.png" height="80%" width="80%" alt="AzureCloudSOC"/>
<br/>
<br/>

Click the blue "Review + Create" button <br/>
Then click the blue "Create" button <br/>
<img src="https://i.imgur.com/jT6AM6E.png" height="80%" width="80%" alt="AzureCloudSOC"/>
<br/>
<br/>

Click "Go to resource" <br/>
On the lefthand side open up the "classic" dropdown and select "Virtual machines (deprecated)" <br/>
You should see your virtual machine, but it says not connected <br/>
<img src="https://i.imgur.com/8tfSKf1.png" height="80%" width="80%" alt="AzureCloudSOC"/>
<br/>
<br/>

To connect your VM to the Log Analystics workspace, on the current page click on the machine name
At the top click on "Connect" <br/>
It should begin connecting, this may take a minute <br/>
<img src="https://i.imgur.com/Dw9RGhm.png" height="80%" width="80%" alt="AzureCloudSOC"/>
<br/>
<br/>

Navigate back to the homepage of the Log Analytics Workspace <br/>
On the left side click on "Logs" <br/>
Here we'll want to check if the Workspace is detecting the virtual machine <br/>
Close out of the window they give you <br/>
On the right side where it says "Simple Mode", click on it and select "KQL Mode" <br/>
Type in the command "Heartbeat" and click run <br/>
<img src="https://i.imgur.com/KlVnNT7.png" height="80%" width="80%" alt="AzureCloudSOC"/>
<br/>
<br/>

There should be some output, this shows its connected <br/>


<h2>Connecting to the Virtual Machine</h2>
Let's connect to our Windows VM, go to your Desktop and click in the Search bar <br/>
Search for "Remote Desktop Connection" and open it <br/>
<img src="https://i.imgur.com/dkjOmZQ.png" height="80%" width="80%" alt="AzureCloudSOC"/>
<br/>
<br/>

On the Azure Homepage, click on the Virtual Machine you made <br/>
On the leftside click on "Overview" <br/>
Under the Networking section there should be a "Public IP Address" section <br/>
This will be the IP you use to connect to the machine using the "Remote Desktop Connection" you opened earlier <br/>
<img src="https://i.imgur.com/7NiTUqG.png" height="80%" width="80%" alt="AzureCloudSOC"/>
<br/>
<br/>

Enter the IP on your "Remote Desktop Connection" app <br/>
Enter the username you created earlier <br/>
Before we enter the correct password you made earlier, enter some wrong passwords so we can generate some security events <br/>
Now log in using your Password <br/>
Select "Yes" for the certificate pop up and you should be logged into your VM <br/>
<img src="https://i.imgur.com/aguUaYK.jpeg" height="80%" width="80%" alt="AzureCloudSOC"/>
<br/>
<br/>

<h2>Configuration in the Virtual Machine</h2>
Here we can do a couple things to make sure security events are being ingested and that the Monitoring agent was installed correctly <br/>
First to check if the "Microsoft Monitoring Agent" was installed, click on the search bar <br/>
Search for the "Run" app <br/>
In the "Run" app type the following command: appwiz.cpl <br/>
Here you should see programs that are installed, you should see "Microsoft Monitoring Agent" <br/>
<img src="https://i.imgur.com/IHefB2A.png" height="80%" width="80%" alt="AzureCloudSOC"/>
<br/>
<br/>

Now let's verify the security logs we generated earlier are showing up on this side <br/>
In the search bar type in "Event Viewer" and open it <br/>
On the left side click on the arrow next to "Windows Logs" to show the dropdown options <br/>
Click on "Security" <br/>
<img src="https://i.imgur.com/IBa6uAd.png" height="80%" width="80%" alt="AzureCloudSOC"/>
<br/>
<br/>

On the right hand side, click on "Filter Current Log" <br/>
This will open a pop up, here you will see a section thats says "<All Event IDs'>" <br/>
In that section we will type in: 4625 (This is the event ID associated with failed logins which we generated earlier) <br/>
<img src="https://i.imgur.com/aAw4nec.png" height="80%" width="80%" alt="AzureCloudSOC"/>
<br/>
<br/>

Click "OK" and it should now show you all of the failed authentication attempts <br/>
Click on one and you can see more about it <br/>
This is proof that the logs were generated correctly, so when we use Microsoft Sentinel it should appear there too once setup <br/>
<img src="https://i.imgur.com/Lkl1gTA.png" height="80%" width="80%" alt="AzureCloudSOC"/>
<br/>
<br/>

<h3>If Failed logins aren't showing up here's how to fix (Optional)</h3>
If the logs were not showing in the event viewer it's possible the firewall is blocking them, so to make this easy we can turn it off <br/>
Go to the search bar and type in the following: <strong>wf.msc</strong> <br/>
At the top click on "Actions" and then "Properties" <br/>
Go to each of the firewall profiles Domain, Private, and Public and set the "Firewall State" to "OFF" <br/>
<img src="https://i.imgur.com/3q8A8ek.png" height="80%" width="80%" alt="AzureCloudSOC"/>
<br/>
<br/>

Click "Apply" then "Ok" to save your changes <br/>
You have have to logoff the VM and do more failed logins to generate some more <br/>
Then go back to event viewer and you should see the failed logins now! <br/>


<h2>Configuring Microsoft Sentinel</h2>

Let's head back to Azure and Navigate to the homepage <br/>
Search for or click on "Microsoft Sentinel" <br/>
Click "Create" <br/>
Select the Log Analystics Workspace you created and click "Add" <br/> 
This can take a bit of time so just wait <br/>
<img src="https://i.imgur.com/r0Nxuy3.png" height="80%" width="80%" alt="AzureCloudSOC"/>
<br/>
<br/>

On the left side go to the "Configuration" section and in that section click on "Data Connectors" <br/>
Here you can see all of the Add-Ons that are currently connected to your Microsoft Sentinel <br/>
Click on "Content Hub" near the search bar <br/>
<img src="https://i.imgur.com/dBpK7rX.png" height="80%" width="80%" alt="AzureCloudSOC"/>
<br/>
<br/>

In the Search bar for the Content Hub page, search for "Security Events" <br/>
Under "Content Title", find the one that says "Windows Security Events" and click on the checkbox next to it <br/>
Click the blue "Install" button <br/>
<img src="https://i.imgur.com/9qzSAo1.png" height="80%" width="80%" alt="AzureCloudSOC"/>
<br/>
<br/>

Once installed click on the blue "Manage" button where "install" was located previously <br/>
Under "Content Name", find "Windows Security Events via AMA" and select the checkbox next to it <br/>
Click the blue "Open Connector Page" <br/>
Click the "+Create Data Collection Rule" button <br/>
Under "Rule name", set it to whatever you want <br/>
Make sure under "Resource Group" that it's the one you've been using so far <br/>
<img src="https://i.imgur.com/2HUo5NU.png" height="80%" width="80%" alt="AzureCloudSOC"/>
<br/>
<br/>

Click "Next" until you get to the Collect section <br/>
Make sure "All Security Events" is selected <br/>
Click "Next" and then click the blue "Create" button <br/>
Go back to the "Data Connectors" section and look to see if the "Windows Security Events via AMA" is there and connected <br/>
<img src="https://i.imgur.com/DEjdajl.png" height="80%" width="80%" alt="AzureCloudSOC"/>
<br/>
<br/>

<h2>Using Sentinel to Check the Security Events</h2>

Let's Navigate to the Sentinel you created and once in click on "Logs" on the left side <br/>
Close the popup window it gives you <br/>
On the right hand corner where it says "Simple Mode", click on it and select "KQL Mode" <br/>
Type in the following: <strong>SecurityEvent</strong> <br/>
Click the blue "Run" button <br/>
You should see a bunch of logs pop up <br/>
<img src="https://i.imgur.com/TpIzCkx.png" height="80%" width="80%" alt="AzureCloudSOC"/>
<br/>
<br/>

Now let's find our logs where there where failed login attempts (Note: may have to create more failed login attempts since Sentinel wasn't setup before) <br/>
Type the following command in: <br/>
```
SecurityEvent
| where EventID == 4625
```
<br/>

You should now see all of the attempts you generated for failed logins <br/>
<img src="https://i.imgur.com/JErhHxU.png" height="80%" width="80%" alt="AzureCloudSOC"/>
<br/>
<br/>

You can click the arrow to expand information about the generated log <br/>
<img src="https://i.imgur.com/UvoxcvX.png" height="80%" width="80%" alt="AzureCloudSOC"/>
<br/>
<br/>

Congrats on setting up your Azure cloud SOC lab! <br/>

<h2>Creating Alerts</h2>

Navigate to your Microsoft Sentinel resource <br/>
On the left side click "Analyitics" <br/>
Sometimes the page may say it has been moved and to click the link to go to the Defender portal, if it does just click it <br/>
<img src="https://i.imgur.com/UvoxcvX.png" height="80%" width="80%" alt="AzureCloudSOC"/>
<br/>
<br/>

In "Analyitics" Click the "Create" button at the top <br/>
Here you can choose a "scheduled rule or a "NRT rule" (Near-real-time) <br/>
Choose what you think is best, I'll choose scheduled for now <br/> 
<img src="https://i.imgur.com/9hu1Yth.png" height="80%" width="80%" alt="AzureCloudSOC"/>
<br/>
<br/>

Let's begin creating the rule <br/>
First enter a name for the rule you are going to create, im doing one for failed logins so I named mine "InvalidLogin" <br/>
You can also create a description for the alert if you want <br/>
The Severity will be up to you, I leave mine at Medium <br/>
You can also add the type of MITRE ATT&CK, in my case I would add "Intial Access" and  under that section add "Valid account" <br/>
<img src="https://i.imgur.com/BOGYajx.png" height="80%" width="80%" alt="AzureCloudSOC"/>
<br/>
<br/>

Click "Next" <br/>
In "Rule Query" we must write the rule like we did previously when we did the command: SecurityEvent <br/>
In this case I want failed logins and specifically for my user so I'll enter the following: <br/>
```
SecurityEvent
| where EventID == 4625 
| where Account contains "SOCanalyst"
```
<br/>

You can replace the "SOCanalyst" with whatever username you created <br/>
If you click the blue "View Query Results" link, it will take you to a page where you can run you query to see if it works! <br/>
<img src="https://i.imgur.com/GpYmksd.png" height="80%" width="80%" alt="AzureCloudSOC"/>
<br/>
<br/>

Under "Query Scheduling" we can set how often we want these alerts to get ran to try and detect something <br/>
For the purposes of testing we will make "Run Query Every" section to 5 Minutes <br/>
We will then go to the "Lookup data from the last" and make this 1 day <br/>
<img src="https://i.imgur.com/GpYmksd.png" height="80%" width="80%" alt="AzureCloudSOC"/>
<br/>
<br/>

The "Alert Threshold" I will leave as 0 but you can also set it to 1 (this is if the alert appears more than once) <br/>
Click "Next" until you see the blue "save" button <br/>
Click it <br/>
You should now see your Alert created <br/>
<img src="https://i.imgur.com/mNWkTmd.png" height="80%" width="80%" alt="AzureCloudSOC"/>
<br/>
<br/>

<h2>Testing the Alert</h2>

On the left side Click on "Investigation and Response" -> then on "Incidents and Alerts" <br/>
Click on "Incidents" <br/>
Generate some more invalid logins attempts <br/>
Wait a couple of minutes and click the "refresh" button in incidents <br/>
You can see I generated some <br/>
<img src="https://i.imgur.com/XzNJpzj.png" height="80%" width="80%" alt="AzureCloudSOC"/>
<br/>
<br/>

If we click on it we can view all the details including the query that was ran which will give us things such as time generated <br/>
<img src="https://i.imgur.com/LPcRoW7.png" height="80%" width="80%" alt="AzureCloudSOC"/>
<br/>
<br/>

Congrats you have created and tested an alert! <br/>

</p>
