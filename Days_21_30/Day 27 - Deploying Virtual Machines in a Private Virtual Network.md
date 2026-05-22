The Nautilus DevOps team is expanding their Azure infrastructure and requires the setup of a private Virtual Network (VNet) along with a subnet. This VNet and subnet configuration will ensure that resources deployed within them remain isolated from external networks and can only communicate within the VNet. Additionally, the team needs to provision a Virtual Machine (VM) under the newly created private VNet. This VM should be accessible over SSH from within the VNet only, allowing for secure communication and resource management within the Azure environment.

The name of the VNet must be `nautilus-priv-vnet`, create a subnet named `nautilus-priv-subnet` under the same. Further, create a Virtual Machine named `nautilus-priv-vm` under this VNet. Additionally, create a Network Security Group (NSG) named `nautilus-priv-nsg`, and ensure that the NSG rules for the VM allow access only from within the VNet's CIDR block. Ensure all resources are created in the `Central US` region.

`Notes:`

- Create the resources **only** in the `Central US` region.
- Use the VNet CIDR `10.0.0.0/16` for nautilus-priv-vnet in Central US.
- Set up an explicit NSG inbound SSH rule on nautilus-priv-nsg with the following parameters:
    - Source: 10.0.0.0/16
    - Destination: 10.0.0.0/16
    - TCP Port: 22
    - Action: Allow.
### Answer using Azure Portal

1. Sign into Azure Portal with the credentials provided.
2. Type in `vnet` in the search bar at the top and click on virtual networks in the options.
3. Click on + Create in the overview page.
4. Ensure a resource group is selected and enter the name `nautilus-priv-vnet`.
5. Ensure Region is `Central US`.
6. Click next until we get to Networks.
7. Change the default network name to `nautilus-priv-subnet` and ensure the default for Private subnet is checked.
8. In the Subnet also create a new Network Security Group called `nautilus-priv-nsg`.
9. Then click next.
10. Click on Review + Create.
11. Once the VNet and Subnet have been created, search at the top for vm and click on Virtual Machines.
12.  Click + Create.
13. Choose Virtual machine.
14. Select Resource group (kml_rg_main****** ).
15. Virtual machine name = `nautilus-pub-vm`.
16. Click on Region and choose `Central US`.
17. For Image choose `Ubuntu Pro 24.04 LTS`.
18. For Size click on the drop down and then See all sizes.
19. Select Class B and then `Standard_B1s`.
20. For Authentication type select SSH public key.
21. Use the `azureuser` for username (this is automatically generated).
22. Ensure RS SSH Format is selected.
23. Use the auto-generated Key pair name.
24. For Public inbound ports select Allow selected ports
25. Ensure SSH (22) is selected for Select inbound ports (should be selected by default).
26. Click on Next: Disk.
27. For OS disk size ensure `Image Default (30GiB)` is selected.
28. OS Disk type, select `Standard HDD`.
29. Click on Next.
30. Under Network Section, ensure the VNet is set to `nautilus-priv-vnet` and the subnet is `nautilus-priv-subnet`.
31. Under Network Security Group, create a new one with the name `nautilus-priv-nsg`.
32. Ensure  that SSH (22) port is allowed. 
33. Click on Review + Create.
34. Once the VM is is created we need to set the inbound rule.
35. Search for nsg in the top seach bar and click on network security groups. (The NSG can also be navigated to under Networking in the VM or from the VNet as well.)
36. Select the group `nautilus-priv-nsg` and create a new inboud rule with the following parameters.
	- Source: 10.0.0.0/16
    - Destination: 10.0.0.0/16
    - TCP Port: 22
    - Action: Allow.
    - Priority: 1000
37. Once the rule has been created, we can double check by going back into the VM and double check that under networking the rule has been applied. 
38. Then click Check to complete the task. 
