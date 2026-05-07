Advanced Real-World Azure Networking Lab** simulating a company environment with Production and Management networks

 🎯 Scenario
A company wants to:
- Separate **Production** and **Management** environments
- Allow secure communication using **VNet Peering**
- Protect servers using **NSG + ASG**
- Allow management access (RDP) only from the Management network

 🏗️ Architecture
 ./Architecture/main diagram.png
 
 <img width="410" height="322" alt="image" src="https://github.com/user-attachments/assets/57dff98c-792f-48e6-9425-58f694728123" />

🛠️ What You Will Implement
- Resource Group
- Two VNets + Subnets
- Virtual Machines (Web, App, Management)
- Application Security Groups (ASG)
- Network Security Group (NSG) with custom rules
- VNet Peering (Bidirectional)

 📋 Lab Steps
→ [Full Step-by-Step Guide](./Step-by-Step-Guide.md)

 🧪 Testing
→ [Verification & Testing](./Verification.md)

 📁 Repository Structure
- **screenshots/** → All lab screenshots (organized by component)
- **Step-by-Step-Guide.md** → Detailed walkthrough
- **Verification.md** → How to test the lab

🧹 Cleanup
Delete the resource group `rg-network-project` to avoid unnecessary costs.

