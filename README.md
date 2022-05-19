# My Custom Configured Azure SIEM


This is my first real security project involving Azure that I'm excited to share. A lot of components went into creating this SIEM and monitoring real-time brute-force attacks coming in from all over the globe against my poor little defenseless honeypot VM was nothing short of exhilarating!

Here's a visualization of what this process looks like. Not really much to look at yet but the interesting stuff is always found under the hood anyway, right?

![visualizer](https://user-images.githubusercontent.com/105020710/169176840-615e4402-d9a8-4e03-9a73-59345eeaa14e.png)



The first step on this perilous journey was to create a poorly defended VM in Azure. 

![VM overview](https://user-images.githubusercontent.com/105020710/169175169-df3576ee-7265-48fc-bcde-6f4acab1f1be.png)


It was important during the VM creation process to create a network security group that provided a totally porous ingress that would allow for a barrage of attacks against my honeypot VM. As you can see, I created an inbound firewall rule named DANGER_ANY_IN that was set to allow ANY inbound network traffic.

![nsg](https://user-images.githubusercontent.com/105020710/169175045-b7f76f29-c5e7-48fb-b1cb-2da18250acf2.png)


After a few more tweaks, my honeypot resource group was complete.

![honeypot-RG-overview](https://user-images.githubusercontent.com/105020710/169175975-c107cfff-426b-4b3f-8612-d70414c15b73.png)


In order for me to be able to monitor the attacks coming in against my honeypot VM, I would next need to create a Log Analytics workspace. The documentation for this process can be found on the [Microsoft Docs](https://docs.microsoft.com/en-us/azure/azure-monitor/logs/quick-create-workspace) site if you aren't already familiar with this process.

To enable the ability to collect logs from my honeypot VM and connect them to the Log Analytics workspace, the next step was to ensure that Microsoft Defender for Cloud (previously Security Center) had the Azure Defender setting enabled. 

Note: At this point, I remoted into the honeypot VM and turned off the Windows Defender Firewall for the Domain, Private and Public profiles which allowed attackers immediate access to start launching brute force attacks against it.

![defender on](https://user-images.githubusercontent.com/105020710/169176188-e0a9759a-14a8-4e0b-abd7-2ac6d650735e.png)


Back in Azure, in the Data Collection screen, I made sure to enable log collection for All Events.

![defender-data-collection](https://user-images.githubusercontent.com/105020710/169176207-77c301df-aaaa-48ad-aad3-dfda146ffa9a.png)


After, in the Log Analytics workspace screen I connected the honeypot VM to the Log Analytics workspace.

![connect vm to law](https://user-images.githubusercontent.com/105020710/168951818-584fb13c-70fb-40fc-bb89-2daceb97b266.png)

It's now time to set up the Azure Sentinel SIEM. Things are starting to get interesting! [Here's a great article](https://docs.microsoft.com/en-us/azure/sentinel/quickstart-onboard) if you want to set this up yourself.

I found a nice ipgeolocation API that tracked the coordinates of where the attacks were coming from by plugging in my honeypot's IP address into its site.

![ipgeo api](https://user-images.githubusercontent.com/105020710/168953082-72dab264-95ea-47f2-9fe4-54b7c8b2987d.jpg)

Inside the honeypot VM, I ran a custom security log exporter PowerShell script that uses the API key from the geolocation site to view the geolocation data from each attacker, while also combing the event viewer Security logs to display all the failed log-in events in real-time!

![PowerShell log extraction code](https://user-images.githubusercontent.com/105020710/168953538-ef923632-ace3-4b17-a0c4-3b753d54bb65.jpg)

Once I kicked off the PowerShell script, it would chronologically output the scraped data from failed attackers including their location, source IP, and even the username that they tried brute forcing my VM with.

![Incoming attacks PowerShell](https://user-images.githubusercontent.com/105020710/168953910-6301e6fb-79da-4cbe-9648-1df40745f549.jpg)

It dumps those failed log-in attempts to a .txt file in the ProgramData folder on the client VM and outputs the following:

![failed logon txt dumps](https://user-images.githubusercontent.com/105020710/168954247-c352e290-0385-4cf6-a277-bb0cd5e3e094.jpg)

Well, we're officially able to see brute force attackers from all over the globe in real-time attacking our poor honeypot. Now it's time to set up a custom log script that will allow that data to be imported into the Log Analytics Workspace. A simple query referencing the column data from the previous PowerShell script should do it.


![query](https://user-images.githubusercontent.com/105020710/169176705-c3b100a2-c1ac-4e5a-b57d-f632afb4842a.png)


After a few custom field extractions, the data started pouring into the Log Analytics workspace that matched the Security logs from the honeypot VM. Finally, I get to see the security logs directly from the Azure UI!

![Azure Honeypot logs](https://user-images.githubusercontent.com/105020710/168954879-7b97917b-883c-4186-b3c6-d1856794263e.jpg)

The Azure Sentinel dashboard is lighting up as well!


![sentinel](https://user-images.githubusercontent.com/105020710/169176723-67a6c626-cc39-4b8f-8c13-d0e114064b16.png)

The most exciting part about this process (besides getting to see attacks being launched from all over the globe against one of my Azure resources in real-time) was the beautiful visuals that were created. By creating a new workbook in Azure Sentinel and matching the map settings to the geographic data generated in the security logs in LAW, I was able to create an actual heat map of where the attackers were coming from.

I let the honeypot sit for about 24 hours before coming back to check on the activity that occurred against my enticing VM. 

What I discovered was astounding...

![Azure Honeypot Attacks - Microsoft Azure](https://user-images.githubusercontent.com/105020710/168955271-00c717aa-325f-4de7-8bd1-eefa21937522.jpg)

In only 24 hours from the moment I set up this honeypot and SIEM to monitor the attacks against it, I was able to observe about 16,000 malicious brute-force attacks from all over the world.

This is only the start of a very long and arduous journey into my Azure Security experience and I'm glad I was able to share it here.
