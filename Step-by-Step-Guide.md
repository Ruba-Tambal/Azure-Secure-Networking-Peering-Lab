Advanced Azure Networking Lab – Step-by-Step Professional Guide
Scenario: Real company environment with separate Production and Management networks.

STEP 1 — Create Resource Group

Go to the Azure Portal
Search for Resource groups → Click + Create
Settings:
Name: rg-network-project
Region: UAE North

Click Review + create → Create


STEP 2 — Create Production VNet

Search for Virtual networks → + Create
Basics tab:
Resource group: rg-network-project
Name: vnet-production
Region: UAE North

IP Addresses tab:
IPv4 address space: 10.0.0.0/16
Delete the default subnet

Click Review + create → Create


STEP 3 — Create Production Subnet

Open vnet-production
Go to Subnets (left menu) → + Subnet
Settings:
Name: subnet-app
Address range: 10.0.1.0/24

Click Save


STEP 4 — Create Management VNet

Create another Virtual Network:
Name: vnet-management
Address space: 10.1.0.0/16
Region: UAE North



STEP 5 — Create Management Subnet
Inside vnet-management:

Name: subnet-management
Address range: 10.1.1.0/24


STEP 6 — Create Virtual Machines
VM 1: Web Server

Name: vm-web
Resource group: rg-network-project
Region: UAE North
Image: Windows Server 2022 Datacenter (or Ubuntu Server)
Size: Standard_D2s_v3
Virtual network: vnet-production
Subnet: subnet-app
Public IP: None (recommended for security)

VM 2: App Server

Name: vm-app
Same VNet and Subnet as vm-web

VM 3: Management VM (Jumpbox)

Name: vm-management
Virtual network: vnet-management
Subnet: subnet-management


STEP 7 — Create Application Security Groups (ASG)

Search for Application security groups → + Create
Create two ASGs:
Name: asg-web
Name: asg-app



STEP 8 — Associate ASGs to VMs

Open vm-web → Networking → Click on the Network Interface → Application security groups → Add asg-web
Open vm-app → Add asg-app


STEP 9 — Create Network Security Group (NSG)

Search for Network security groups → + Create
Name: nsg-production
Region: UAE North
Click Review + create → Create


STEP 10 — Configure NSG Rules
Go to nsg-production → Inbound security rules → + Add
Rule 1: Allow RDP from Management Network

Source: IP Addresses
Source IP addresses: 10.1.0.0/16
Destination: Any
Destination port ranges: 3389
Protocol: TCP
Action: Allow
Priority: 100
Name: Allow-RDP-Management

Rule 2: Allow Web to App Communication

Source: Application security group
Source ASG: asg-web
Destination: Application security group
Destination ASG: asg-app
Destination port ranges: 80, 443
Protocol: TCP
Action: Allow
Priority: 110
Name: Allow-Web-to-App

Leave the default DenyAllInbound rule as is (lowest priority).

STEP 11 — Associate NSG to Subnet

Open nsg-production
Go to Subnets → + Associate
Select:
Virtual network: vnet-production
Subnet: subnet-app

Click OK


STEP 12 — Configure VNet Peering
Peering 1: From Production to Management

Open vnet-production → Peerings → + Add
Settings:
Peering link name: prod-to-mgmt
Virtual network: vnet-management
Allow virtual network access: Enabled
Allow forwarded traffic: Enabled


Peering 2: From Management to Production

Open vnet-management → Peerings → + Add
Settings:
Peering link name: mgmt-to-prod
Virtual network: vnet-production
Allow virtual network access: Enabled



STEP 13 — Testing

From vm-management → RDP to vm-web → Should work
From vm-web → Connect to vm-app on port 80 → Should work
From vm-management → Try to connect to vm-app → Should be blocked
Check both peerings status = Connected
