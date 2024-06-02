<h3><p align="center"><b>Site-to-site VPN connection in the Azure portal</b></p></h3><br />


<p align="center" width="100%">
    <img width="70%" src="https://github.com/stahir131/Site-to-site-VPN-connection-in-the-Azure-portal/assets/64047385/6bc7a838-17c3-49cc-bc4c-a134e98d9df8">
</p>
<h3>Environments Used</h3>

- Azure subscription
- Windows 10/11 client
- Windows server 2019
  
Here are 10 easy steps to create a S2S VPN between on-premises network and the Azure resources.<br />

1.  Resource group - S2SiteVPNRg
2.  Create Azure virtual network
3.  Create Local network Gateway: 
4.  Azure Public IP: Used by the virtual network gateway
5.  Virtual Network gateway: Used to link to the on-prem environment through the VPN connection.
6. VPN connection: IKEv2/ipsec connection
7.  Test VM in Azure
8.  Configure RRAS on on-premises server 2019
9.  Add static route in Azure - 10.0.0.0
10.  Test RDP connection

<b>Step 1</b>: Login to Azure here to create Resource group - S2SVPNRg

<b>Step 2: Create Azure virtual network: Search for virtual network and create as shown below</b>
<p align="center" width="100%">
    <img width="70%" src="https://github.com/stahir131/Site-to-site-VPN-connection-in-the-Azure-portal/assets/64047385/f9239aae-5c7d-4eda-abbb-05eb495558b8">
</p>

Click next on to IP addresses, create a virtual network with address space of 10.0.0.0/24(256 IPs) and a subnet s2s-subnet of 10.0.0.0/26(64 IPs).
<p align="center" width="100%">
    <img width="70%" src="https://github.com/stahir131/Site-to-site-VPN-connection-in-the-Azure-portal/assets/64047385/7e2c6e41-dc17-4785-9e07-b295b2c62b94">
</p>

Creating a Gateway subnet in the S2Site_Vnet with 10.0.0./27.This subnet will be used in creating a Virtual network gateway in step 5.
<p align="center" width="100%">
    <img width="70%" src="https://github.com/stahir131/Site-to-site-VPN-connection-in-the-Azure-portal/assets/64047385/985f45bd-8a77-4280-86c2-809efe3f51d5">
</p><br />


<b>Step 3</b>: Local Network Gateway: Search for resource name and create. <br />The name is Site2SiteLocalNetGtway and the IP address will be my public facing IP address. 21.21.21.21<br />
Address spaces will be my local on-premise IP CIDR
<p align="center" width="100%">
    <img width="70%" src="https://github.com/stahir131/Site-to-site-VPN-connection-in-the-Azure-portal/assets/64047385/769596ed-af12-4c5c-a562-ba2e0ed0df1a">
</p>
<p align="center" width="100%">
    <img width="70%" src="https://github.com/stahir131/Site-to-site-VPN-connection-in-the-Azure-portal/assets/64047385/16efa187-7e9f-4dbd-9374-b1d3df05c5ae">
</p>


<b>Step 4</b>: Create azure public IP address: 20.120.111.124 (AzurePublicIP): Would be used when creating RRAS on the Windows server.

<b>Step 5: Virtual Network gateway:</b>  So I got an error below that the gateway subnet must be /27 or larger.<br />
Disable active-active mode<br />
Disable BGP
<p align="center" width="100%">
    <img width="70%" src="https://github.com/stahir131/Site-to-site-VPN-connection-in-the-Azure-portal/assets/64047385/7fb4e092-b271-48b6-8136-c80f0cdeec6b">
</p>
I’m changing my gateway subnet to 10.0.0.64/27. Deployment took about 15 mins.
<p align="center" width="100%">
    <img width="70%" src="https://github.com/stahir131/Site-to-site-VPN-connection-in-the-Azure-portal/assets/64047385/1ec97361-5d5e-40f5-967b-caa05aebcf8e">
</p>

The virtual network gateway was created successfully.
<p align="center" width="100%">
    <img width="70%" src="https://github.com/stahir131/Site-to-site-VPN-connection-in-the-Azure-portal/assets/64047385/e82314b5-bc3c-4ce7-8f5c-3473d7aed4f4">
</p>

<b>Step 6:</b>  VPN connection: IKEv2/ipsec connection<br />
Search for ‘Connection’ and create the VPN connection:<br /> 
Name: S2SVPN-OnPrem<br />
Connection type: site-to-site(IPsec)<br />
PreShared key: ABCDE123 (anything you want)

<p align="center" width="100%">
    <img width="70%" src="https://github.com/stahir131/Site-to-site-VPN-connection-in-the-Azure-portal/assets/64047385/1c149ab4-7819-44ac-8726-cc7071d1a98a">
</p>

Got another error below but was fixed this by changing my Virtual network IPs to a 192.168.0.0
<p align="center" width="100%">
    <img width="70%" src="https://github.com/stahir131/Site-to-site-VPN-connection-in-the-Azure-portal/assets/64047385/1038ca12-320b-40b7-af67-6991bee8642f">
</p>

Connection was created successfully as shown below.
<p align="center" width="100%">
    <img width="70%" src="https://github.com/stahir131/Site-to-site-VPN-connection-in-the-Azure-portal/assets/64047385/f4667411-f254-4660-903a-bba21439a69c">
</p>

<b>Step 7.</b> Created a test Azure vm<br /> 
Private IP:192.168.0.4<br /> 
Public IP: 20.55.64.190<br /> 
Username: surry<br /> 
Pw: Welcome12345

<b>Step 8:Configure RRAS on the on-prem server 2019</b>
<p align="center" width="100%">
    <img width="70%" src="https://github.com/stahir131/Backup-on-Premises-Windows-Server-to-Azure/assets/64047385/748b4a3b-f7ee-41f5-943a-5c06e2b49959">
</p>
Click Next<br />

<p align="center" width="100%">
    <img width="70%" src="https://github.com/stahir131/Site-to-site-VPN-connection-in-the-Azure-portal/assets/64047385/1298c1df-8555-4fb8-8f41-272cfc0b5473">
</p>
Click Next<br />

   
<p align="center" width="100%">
    <img width="70%" src="https://github.com/stahir131/Site-to-site-VPN-connection-in-the-Azure-portal/assets/64047385/92b17911-094f-4db3-9513-22c37d37ee11">
</p>
Click Next<br />

<p align="center" width="100%">
    <img width="70%" src="https://github.com/stahir131/Site-to-site-VPN-connection-in-the-Azure-portal/assets/64047385/5a955c4b-446d-48bd-8f47-d0c1f88ba5c7">
</p>
Click Next<br />


<p align="center" width="100%">
    <img width="70%" src="https://github.com/stahir131/Site-to-site-VPN-connection-in-the-Azure-portal/assets/64047385/9b4b5841-e735-49c5-9b20-40bb4d84fc41">
</p>

Choose Deploy VPN Only


<p align="center" width="100%">
    <img width="70%" src="https://github.com/stahir131/Site-to-site-VPN-connection-in-the-Azure-portal/assets/64047385/59b8fc96-d3fb-45d9-9637-e889473c096b">
</p>

Right-click on server name, click Configure and enable routing and remote access and click Next


<p align="center" width="100%">
    <img width="70%" src="https://github.com/stahir131/Site-to-site-VPN-connection-in-the-Azure-portal/assets/64047385/5b0aae68-ec48-4a7c-a021-65df39a6bd77">
</p>

<p align="center" width="100%">
    <img width="70%" src="https://github.com/stahir131/Site-to-site-VPN-connection-in-the-Azure-portal/assets/64047385/d1daf4aa-1f93-480d-a31f-2ba04dd40d3c">
</p>

Take default and Finish.

<p align="center" width="100%">
    <img width="70%" src="https://github.com/stahir131/Site-to-site-VPN-connection-in-the-Azure-portal/assets/64047385/1a3585b4-3b75-448a-86ba-0b4d6cfd80e4">
</p>

<p align="center" width="100%">
    <img width="70%" src="https://github.com/stahir131/Site-to-site-VPN-connection-in-the-Azure-portal/assets/64047385/8f0eb6ab-f21a-4ab9-ad4b-20cd0baad143">
</p>

Name: S2SVPNtoAzure<br />
VPN type: IKEv2<br />
Destination address: Azure public address  - 20.120.111.124

<p align="center" width="100%">
    <img width="70%" src="https://github.com/stahir131/Site-to-site-VPN-connection-in-the-Azure-portal/assets/64047385/6cad5ccb-b2ee-4b8f-a354-21a45d093485">
</p>

<b>Step 9</b>. Add static route in Azure - 10.0.0.0

Put Azure virtual network address space 198.168.0.0 in static route
Next find the Network interface on the left, right-click to properties to include the Preshared key from Azure. Surry123

<p align="center" width="100%">
    <img width="70%" src="https://github.com/stahir131/Site-to-site-VPN-connection-in-the-Azure-portal/assets/64047385/b567d198-5e16-4fa2-8051-e11c43858ba5">
</p>

Test VPN connection

RDP to Azure irtual machine<br />
Create a network drive on the on-prem server<br />
Map the drive to the Azure vm.

