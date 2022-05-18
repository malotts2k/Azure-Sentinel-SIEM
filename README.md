# The Azure Sentinel SIEM that I created using a completely exploitable honeypot Azure VM:


This is my first real security project involving Azure that I'm excited to share. A lot of components went into creating this SIEM and monitoring real-time brute-force attacks coming in from all over the globe against my poor little defenseless honeypot VM was nothing short of exhilirating!

The first step on this perilous journey was to create a poorly defended VM in Azure. 

![honeypot-vm properties](https://user-images.githubusercontent.com/105020710/168947818-59d2afd4-6613-42fb-abaa-9074d3222853.jpg)

It was important during the VM creation process to create a network security group that provided a totally porous ingress that would allow for a barrage of attacks against my honeypot VM. As you can see, I created an inbound firewall rule named DANGER_ANY_IN that was set to allow ANY inbound network traffic.

![NSG-honeypot](https://user-images.githubusercontent.com/105020710/168949174-0da2ca09-6ac0-404b-bd00-958760330ef9.jpg)

After a few more tweaks, my honeypot resource group was complete.

![honeypot-rg overview](https://user-images.githubusercontent.com/105020710/168949778-08d6347f-94df-44c9-9176-5e521005b691.jpg)

In order for me to be able to monitor the attacks coming in against my honeypot VM, I would next need to create a Log Analytics workspace. The documentation for this process can be found on the [Microsoft Docs](https://docs.microsoft.com/en-us/azure/azure-monitor/logs/quick-create-workspace) site if you aren't already familiar with this process.

In order to enable the ability to collect logs from my honeypot VM and connect them to the Log Analytics workspace, the next step was to ensure that Microsoft Defender for Cloud (previously Security Center) had the Azure Defender setting enabled. At the same time I also RDP'ed into the honeypot and turned off the Windows Defender Firewall for the Domain, Private and Public profiles.

![defender and firewall](https://user-images.githubusercontent.com/105020710/168951191-e4b4960b-e2a6-4352-9454-920134afca17.jpg)

Back in Azure, in the Data Collection screen, I made sure to enable log collection for All Events.

![data collection setting - defender](https://user-images.githubusercontent.com/105020710/168951297-ae89da5a-f39b-4c84-b267-658cf7fcc086.jpg)

After, in the Log Analytics workspace screen I connected the honeypot VM to the Log Analytics workspace.

![connect vm to law](https://user-images.githubusercontent.com/105020710/168951818-584fb13c-70fb-40fc-bb89-2daceb97b266.png)

