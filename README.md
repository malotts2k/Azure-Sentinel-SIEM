# The Azure Sentinel SIEM that I created using a completely exploitable honeypot Azure VM:


This is my first real security project involving Azure that I'm excited to share. A lot of components went into creating this SIEM and monitoring real-time brute-force attacks coming in from all over the globe against my poor little defenseless honeypot VM was nothing short of exhilirating!

Here's a visualization of what this process looks like. Not really much to look at yet but the interesting stuff is always found under the hood anyway, right?

![Honeypot-rg Visualizer](https://user-images.githubusercontent.com/105020710/168952475-019a92bb-d2a3-4d76-9453-d0f3e9f13150.jpg)


The first step on this perilous journey was to create a poorly defended VM in Azure. 

![VM overview](https://user-images.githubusercontent.com/105020710/168951948-b96a3659-209c-4de3-a982-60f73633b7f5.png)


It was important during the VM creation process to create a network security group that provided a totally porous ingress that would allow for a barrage of attacks against my honeypot VM. As you can see, I created an inbound firewall rule named DANGER_ANY_IN that was set to allow ANY inbound network traffic.

![nsg](https://user-images.githubusercontent.com/105020710/168952065-db1b6ba8-664d-45f0-9f82-0b0aa5a592ea.png)

After a few more tweaks, my honeypot resource group was complete.

![honeypot-rg overview](https://user-images.githubusercontent.com/105020710/168949778-08d6347f-94df-44c9-9176-5e521005b691.jpg)

In order for me to be able to monitor the attacks coming in against my honeypot VM, I would next need to create a Log Analytics workspace. The documentation for this process can be found on the [Microsoft Docs](https://docs.microsoft.com/en-us/azure/azure-monitor/logs/quick-create-workspace) site if you aren't already familiar with this process.

In order to enable the ability to collect logs from my honeypot VM and connect them to the Log Analytics workspace, the next step was to ensure that Microsoft Defender for Cloud (previously Security Center) had the Azure Defender setting enabled. At the same time I also RDP'ed into the honeypot and turned off the Windows Defender Firewall for the Domain, Private and Public profiles.

![defender and firewall](https://user-images.githubusercontent.com/105020710/168951191-e4b4960b-e2a6-4352-9454-920134afca17.jpg)

Back in Azure, in the Data Collection screen, I made sure to enable log collection for All Events.

![data collection setting - defender](https://user-images.githubusercontent.com/105020710/168951297-ae89da5a-f39b-4c84-b267-658cf7fcc086.jpg)

After, in the Log Analytics workspace screen I connected the honeypot VM to the Log Analytics workspace.

![connect vm to law](https://user-images.githubusercontent.com/105020710/168951818-584fb13c-70fb-40fc-bb89-2daceb97b266.png)

It's now time to set up the Azure Sentinel SIEM. Things are starting to get interesting! [Here's a great article](https://docs.microsoft.com/en-us/azure/sentinel/quickstart-onboard) if you want to set this up yourself.

I found a nice ipgeolocation API that tracked the coordinates of where the attacks were coming from by plugging in my honeypot's IP address into its site.

![ipgeo api](https://user-images.githubusercontent.com/105020710/168953082-72dab264-95ea-47f2-9fe4-54b7c8b2987d.jpg)

