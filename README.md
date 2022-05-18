# The Azure Sentinel SIEM that I created using a completely exploitable honeypot Azure VM:


This is my first real security project involving Azure that I'm excited to share. A lot of components went into creating this SIEM and monitoring real-time brute-force attacks coming in from all over the globe against my poor little defenseless honeypot VM was nothing short of exhilirating!

The first step on this perilous journey was to create a poorly defended VM in Azure. 

![honeypot-vm properties](https://user-images.githubusercontent.com/105020710/168947818-59d2afd4-6613-42fb-abaa-9074d3222853.jpg)

It was important during the VM creation process to create a network security group that provided a totally porous ingress that would allow for a barrage of attacks against my honeypot VM. As you can see, I created an inbound firewall rule named DANGER_ANY_IN that was set to allow ANY inbound network traffic.

![NSG-honeypot](https://user-images.githubusercontent.com/105020710/168949174-0da2ca09-6ac0-404b-bd00-958760330ef9.jpg)

After a few more tweaks, my honeypot resource group was complete.

![honeypot-rg overview](https://user-images.githubusercontent.com/105020710/168949778-08d6347f-94df-44c9-9176-5e521005b691.jpg)

In order for me to be able to monitor the attacks coming in against my honeypot VM, I would next need to create a Log Analytics workspace.
