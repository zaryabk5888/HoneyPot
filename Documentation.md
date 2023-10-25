
<h1>Lets Start</h1>

<h2>Languages and Utilities Used</h2>
<b>-Azure Subscription: Access to an Azure subscription for resource creation.</b><br/>
<b>-IP Geolocation API: Access to an IP geolocation API for data collection.</b><br/>
<b>-PowerShell Environment: An environment for running PowerShell scripts.</b><br/>

<h2>Environments Used </h2>

- <b>Azure Cloud Service</b> (2023-oct)

<h2>Program walk-through:</h2>
<h2>Note : Make all resource and everything in one region and group them all into one resource else it wont work like choose a fix one. I used uk south</h2>
<p align="center">
Architecture: <br/>
<img src="https://i.imgur.com/vR2ep8o.png" height="80%" width="80%" />
<br />
<br />
Create Virtual Machine on Azure Cloud:  <br/>
<img src="https://imgur.com/w5kC6e8.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Allow traffic from all ports and protocols by doing this: <br/>
<img src="https://i.imgur.com/ptupfvq.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Now connect to your vm using native rdp or any other approach and open powershell ise in admin mode and paste the following code in it and also make sure to replace the api key variable with your own that you get from ipgeolocation site after creating account: <a href="https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqa3N6bmVlUDlsdjZidHk0TVdfckM1UkN4UDE2Z3xBQ3Jtc0ttOVpDb21URHBjVGpTWmdodDREa0JjNHQ1cXJ0NXhqRjg5Z2htM2dyRzlReTVTSmFkdzBfejIzOHAzamNoXzlPcnBHRzM2WXZCVUYydTNmVXJfV1BFVUNfNV9ELVZyRkNFbEJkb1VBWlcyOGdWOU8wVQ&q=https%3A%2F%2Fgithub.com%2Fjoshmadakor1%2FSentinel-Lab%2Fblob%2Fmain%2FCustom_Security_Log_Exporter.ps1&v=RoZeVbbZ0o0">click here for the code</a> <br/>
<img src="https://i.imgur.com/PUJfDwK.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Run the script using green button on the top and let it do its magic. Now copy the path where log file will be stored : C:\ProgramData\failed_rdp.log :  <br/>
<img src="https://i.imgur.com/MQ1by34.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Now go back to the host pc and dont stop the virtual machine and to log analatics workspace:  <br/>
<img src="https://i.imgur.com/1V3aUab.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
create data collection rule and it will ask for data collection endpoint so create it and the important part in data collection rule is this part and remember the table name as it will be used later:  <br/>
<img src="https://i.imgur.com/M2g51jw.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
 <br />
open the link https://learn.microsoft.com/en-us/azure/azure-monitor/logs/create-custom-table?tabs=azure-powershell-1%2Cazure-portal-2%2Cazure-portal-3 and then copy the powershell code and paste it in two pieces like $table part and then the funciton in azure cli like this and make sure to put the name of the table exactly and then execute the invoke function with also changing the parameter like workspace, subscription id etc<br/>
<img src="https://i.imgur.com/tGYuQN9.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
 <br />
Now if successful then go to log analatics workspace and to table and finding the table that was created or not<br/>
<img src="https://i.imgur.com/k1RTvnY.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
  <br />
After sometime logs will come in and can be seen in<br/>
<img src="https://i.imgur.com/z969ilG.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
  <br />
Now setup azure sentinel and then go to the workbook part and then to the new query and change the table name<br/>
AllenCustomSecurity3_CL
| extend d = parse_csv(RawData)
| extend Latitude = d[0]
| extend latitude = substring(Latitude, indexof(Latitude, ":") + 1)
| extend Longitude = d[1]
| extend longitude = substring(Longitude, indexof(Longitude, ":") + 1)
| extend Destinationhost = d[2]
| extend destinationhost = substring(Destinationhost, indexof(Destinationhost, ":") + 1)
| extend Username = d[3]
| extend username = substring(Username, indexof(Username, ":") + 1)
| extend Sourcehost = d[4]
| extend sourcehost = substring(Sourcehost, indexof(Sourcehost, ":") + 1)
| extend State = d[5]
| extend state = substring(State, indexof(State, ":") + 1)
| extend Country = d[6]
| extend country = substring(Country, indexof(Country, ":") + 1)
| extend Label = d[7]
| extend label = substring(Label, indexof(Label, ":") + 1)
| extend Timestamp = d[8]
| extend timestamp = substring(Timestamp, indexof(Timestamp, ":") + 1)
| project latitude,longitude,destinationhost,username,sourcehost,state,country,label,timestamp
| summarize event_count=count() by destinationhost, latitude, longitude, country, label, sourcehost
<img src="https://i.imgur.com/2rHr8hvl.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
  <br />
apply and observe attacker pop up<br/>
<img src="https://i.imgur.com/0mDf2Bfl.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
  <br />
save it<br/>
<img src="https://i.imgur.com/wXPO73ol.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

