Lab Verification & Testing

Test Cases

Test 1: Management Access**
- From `vm-management` → RDP to `vm-web`
- **Expected**: Success

Test 2: Application Communication**
- From `vm-web` → Access `vm-app` on port 80/443
- **Expected**: Success (because of ASG rule)

Test 3: Unauthorized Access**
- From `vm-management` → Try to access `vm-app` directly
- **Expected**: Blocked by NSG

Test 4: Peering Validation**
- Check both peerings show **Connected** status

 Useful Commands (Inside VMs)
```powershell
Test connection from vm-web to vm-app
Test-NetConnection -ComputerName <vm-app-private-ip> -Port 80

 Check effective NSG rules
Get-AzEffectiveNetworkSecurityGroup
