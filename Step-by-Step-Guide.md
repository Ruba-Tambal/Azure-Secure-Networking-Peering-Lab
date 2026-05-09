
---

### STEP 1 — Create Resource Group
1. Go to **Resource groups** → **+ Create**
2. Settings:
   - **Name**: `rg-network-project`
   - **Region**: **UAE North**
3. Click **Review + create** → **Create**

### STEP 2 — Create Production VNet
1. Search for **Virtual networks** → **+ Create**
2. **Basics** tab:
   - Resource group: `rg-network-project`
   - Name: `vnet-production`
   - Region: **UAE North**
3. **IP Addresses** tab:
   - IPv4 address space: `10.0.0.0/16`
   - Delete the default subnet
4. Click **Review + create** → **Create**

### STEP 3 — Create Production Subnet
1. Open `vnet-production`
2. Go to **Subnets** → **+ Subnet**
3. Settings:
   - **Name**: `subnet-app`
   - **Address range**: `10.0.1.0/24`
4. Click **Save**

### STEP 4 — Create Management VNet
1. Create another Virtual Network:
   - Name: `vnet-management`
   - Address space: `10.1.0.0/16`
   - Region: **UAE North**

### STEP 5 — Create Management Subnet
Inside `vnet-management`:
- **Name**: `subnet-management`
- **Address range**: `10.1.1.0/24`

---

### STEP 6 — Create Virtual Machines

**VM 1: Web Server**
- Name: `vm-web`
- VNet: `vnet-production` | Subnet: `subnet-app`
- Public IP: **None** (recommended)

**VM 2: App Server**
- Name: `vm-app`
- Same VNet and Subnet as `vm-web`

**VM 3: Management VM (Jumpbox)**
- Name: `vm-management`
- VNet: `vnet-management` | Subnet: `subnet-management`

---

### STEP 7 — Create Application Security Groups (ASG)
1. Search **Application security groups** → **+ Create**
2. Create two ASGs:
   - `asg-web`
   - `asg-app`

### STEP 8 — Associate ASGs to VMs
- Open `vm-web` → **Networking** → Network Interface → **Application security groups** → Add `asg-web`
- Open `vm-app` → Add `asg-app`

### STEP 9 — Create Network Security Group (NSG)
1. Search **Network security groups** → **+ Create**
2. Name: `nsg-production`
3. Region: **UAE North**
4. Create

### STEP 10 — Configure NSG Rules

**Rule 1: Allow RDP from Management Network**
- Source: `10.1.0.0/16`
- Destination port ranges: `3389`
- Protocol: `TCP`
- Action: **Allow**
- Priority: `100`
- Name: `Allow-RDP-Management`

**Rule 2: Allow Web to App Communication**
- Source: Application security group → `asg-web`
- Destination: Application security group → `asg-app`
- Destination port ranges: `80, 443`
- Protocol: `TCP`
- Action: **Allow**
- Priority: `110`
- Name: `Allow-Web-to-App`

> Keep the default **DenyAllInbound** rule.

### STEP 11 — Associate NSG to Subnet
1. Open `nsg-production` → **Subnets** → **+ Associate**
2. Select:
   - Virtual network: `vnet-production`
   - Subnet: `subnet-app`

### STEP 12 — Configure VNet Peering

**Peering 1: Production → Management**
- Open `vnet-production` → **Peerings** → **+ Add**
- Peering link name: `prod-to-mgmt`
- Virtual network: `vnet-management`
- Allow virtual network access: **Enabled**

**Peering 2: Management → Production**
- Open `vnet-management` → **Peerings** → **+ Add**
- Peering link name: `mgmt-to-prod`
- Virtual network: `vnet-production`
- Allow virtual network access: **Enabled**

---

### STEP 13 — Testing

- From `vm-management` → RDP to `vm-web` → **Should work**
- From `vm-web` → Connect to `vm-app` on port 80/443 → **Should work**
- From `vm-management` → Try to connect to `vm-app` → **Should be blocked**
- Check both peerings status = **Connected**

**Lab Completed Successfully!** 🎉
