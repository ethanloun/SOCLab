<h1>SOC Lab</h1>

<h2>Description</h2>
In this lab, I create a simple home Security Operations Center in Azure. I set up a Windows virtual machine as a honeypot, expose it to the public internet to attract genuine attack traffic, and then send the resulting logs to a central Log Analytics Workspace. These logs are then integrated with Microsoft Sentinel for analysis, allowing me to query and visualize attack data, as well as create a real-time attack map that displays attacker positions. The project is designed for practical experience in log analysis, threat detection, and SOC operations in a real-world cloud environment. I used this video for reference - https://www.youtube.com/watch?v=g5JL2RIbThM by Josh Madakor.
<br />

<h2>Languages and Utilities Used</h2>

- <b>Microsoft Azure</b> 

<h2>Environments Used </h2>

- <b>Windows 10 Pro</b> (22H2)

<h2>Overview:</h2>

![Image](https://github.com/user-attachments/assets/bb93aa18-f894-4f5a-844f-a282d07cd092)

<h2>Walk-through:</h2>

<p align="center">
Creating Resource Group: <br/>

<p align="center">
  <img width="700" height="900" src="https://github.com/user-attachments/assets/756aa4c1-3872-4c10-a0ee-ccf668ac3299">
</p>

<p align="center">
Creating Virtual Network: <br/>

<p align="center">
  <img width="700" height="900" src="https://github.com/user-attachments/assets/6b6bb127-43ac-498a-853c-bcade58df13b">
</p>

<p align="center">
Creating Virtual Machine: <br/>

<p align="center">
  <img width="900" height="900" src="https://github.com/user-attachments/assets/c8f6a709-3e9d-4987-ae4f-8e26b567e6bb">
</p>

<p align="center">
Creating Inbound Security Rule to Let Anyone In: <br/>

Removing Remote Desktop Protocol. Literally letting ANY traffic inbound. 

![Image](https://github.com/user-attachments/assets/cca36417-137f-4319-a93f-4c67e0c3068e)

<p align="center">
Connect to Virtual Machine: <br/>

Connecting to Virtual Machine with the IP Address of the VM I created. Pasting that IP into Remote Desktop Connection. 

![Image](https://github.com/user-attachments/assets/c7a23df0-3076-4ba0-900e-3ef3d5600dd7)

<p align="center">
Turn Off the Windows Firewall in the Virtual Machine: <br/>

Turning off the firewalls to allow inbound connections.

![Image](https://github.com/user-attachments/assets/ae4c3da7-087b-4f0e-ac3e-6e6fc012b72c)

<p align="center">
Checking Connection to Public IP: <br/>

Ping Virtual Machine from local computer to see if we can reach it over the internet. If this works then that means the attackers can do the same.

![Image](https://github.com/user-attachments/assets/59ab7ba4-2db2-4d82-8438-0545cde14023)

<p align="center">
Intentionally Failing to Login to Virtual Machine: <br/>

Using the username "employ" to fail logins so that we can see this event in the local logs. 

<p align="center">
<img width="495" height="600" src="https://github.com/user-attachments/assets/1b99fb62-8b5b-4870-9208-09fb102703e6">
</p>

<p align="center">
Viewing Security Events: <br/>

![Image](https://github.com/user-attachments/assets/80f34fcf-664f-4bce-8799-d869df9ea08f)

<p align="center">
See Logs where there were Failed Login Attempts: <br/>

By searching the EventID 4625 I can see the failed logins from before, by the username "employ".

![Image](https://github.com/user-attachments/assets/7d52d606-7c35-48e2-a772-5a502e74122b)

<p align="center">
Creating Log Analytics Workspace to View Logs: <br/>

Create a log repository.

<p align="center">
<img width="600" height="800" src="https://github.com/user-attachments/assets/61b8e3e0-f362-49a6-94f9-6447319bd8a0">
</p>

<p align="center">
Add Microsoft Sentinel to Workspace: <br/>

Linking log repo to SIEM. 

![Image](https://github.com/user-attachments/assets/976d1ac7-d37d-48d9-a2ff-4309a9287664)

<p align="center">
Install Windows Security Event: <br/>

This connects the Virtual Machine to the Log Analytics Workspace.

![Image](https://github.com/user-attachments/assets/14beffba-9009-4ee1-9882-d0392f4a6211)

<p align="center">
Forward Logs into Log Analytics Workspace: <br/>

![Image](https://github.com/user-attachments/assets/4919aa7e-b365-4148-b1af-3d3202b1d437)

<p align="center">
Create Data Collection Rule: <br/>

<p align="center">
<img width="800" height="900" src="https://github.com/user-attachments/assets/d6473132-5c79-408e-80eb-0af795081dfb">
</p>

<p align="center">
Viewing Logs: <br/>

This is to check the windows logs with query "SecurityEvent".

![Image](https://github.com/user-attachments/assets/ee31de51-9644-4bb7-a7f6-39b73b9b2aaa)

<p align="center">
Using this File to Locate Where the IPs Originate from: <br/>

Speadsheet of IP mapping ranges and where they are in the world.

<p align="center">
<img width="600" height="900" src="https://github.com/user-attachments/assets/d48ea81f-3e8d-4de6-ac56-c54af3dd7206">
</p>

<p align="center">
Putting Geoip Into Watchlist: <br/>

![Image](https://github.com/user-attachments/assets/c1dfd8e4-c43c-459e-9006-2a04bcb81cb8)

<p align="center">
Wait for Geoip to Upload: <br/>

This took some time because there are over 55k records. 

![Image](https://github.com/user-attachments/assets/e2cf8ec9-d5ef-4a3d-83d4-0ed86c3cb46b)

<p align="center">
All Items Uploaded: <br/>

![Image](https://github.com/user-attachments/assets/541f7faf-683b-4449-be9e-4eb01b94e0cb)

<p align="center">
Watchlist Uploaded into Log Analytics Workspace: <br/>

![Image](https://github.com/user-attachments/assets/44811019-0bf1-40e0-9bc1-b02f7f7b9551)

<p align="center">
Using Event ID 4625 to Identfy Failed Logins and Grab IP: <br/>

![Image](https://github.com/user-attachments/assets/6e0418d0-1e62-4c98-bcc2-f02f2b211d84)

<p align="center">
Query to Find Location with Longitude and Latitude: <br/>

![Image](https://github.com/user-attachments/assets/e3a6e3b5-f44a-46ae-aa68-0d60c9cb250d)

<p align="center">
Adding JSON to Workboook: <br/>

This JSON creates a map where the attacks can be mapped. Then I can see where the attacks are coming from.

![Image](https://github.com/user-attachments/assets/41a581fa-86ea-472e-a8a4-080cdea3003c)

![Image](https://github.com/user-attachments/assets/5a2bc8a4-d8a8-4884-b81e-eea8805ce3bc)

<p align="center">
Map of Attacks: <br/>

![Image](https://github.com/user-attachments/assets/37811764-45de-47a4-95c4-1d842650e216)

<p align="center">
After a Few Hours: <br/>

![Image](https://github.com/user-attachments/assets/f67e23b7-4338-4471-8129-9b4f4cbe184d)

<p align="center">
After 24 Hours: <br/>

![Image](https://github.com/user-attachments/assets/417878b0-c80d-42b2-8958-21c54cf2ce14)
