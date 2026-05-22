The Nautilus DevOps Team has received a request from the Networking Team to set up a new public VNet to support a set of public-facing services. This VNet will host various resources that need to be accessible over the internet. As part of this setup, you need to ensure the VNet has public subnets with with automatic public IP assignment for resources. Additionally, a new VM will be launched within this VNet to host public applications that require SSH access. This setup will enable the Networking Team to deploy and manage public-facing applications. 

Create a public VNet named `nautilus-pub-vnet`, and a subnet named `nautilus-pub-subnet` under the same, make sure public IP is being auto-assigned to resources under this subnet. Further, create a VM named `nautilus-pub-vm` under this VNet. Make sure SSH port `22` is open for this instance and accessible over the Internet. Use the Azure portal to complete the task and ensure that SSH access is configured correctly. 

`Notes`:

- Create the resources only in the `East US` region

### Answer using Azure Portal

1. Sign into Azure Portal with the credentials provided.
2. Type in `vnet` in the search bar at the top and click on virtual networks in the options.
3. Click on + Create in the overview page.
4. Ensure a resource group is selected and enter the name `nautilus-pub-vnet`.
5. Ensure Region is `East US`.
6. Click next until we get to Networks.
7. Change the default network name to `nautilus-pub-subnet` and keep the rest as it is and press save.
8. Click on Review + Create.
9. Once the VNet and Subnet have been created, search at the top for vm and click on Virtual Machines.
10.  Click + Create.
11. Choose Virtual machine.
12. Select Resource group (kml_rg_main****** ).
13. Virtual machine name = `nautilus-pub-vm`.
14. Click on Region and choose `East US`.
15. For Image choose `Ubuntu Pro 24.04 LTS`.
16. For Size click on the drop down and then See all sizes.
17. Select Class B and then `Standard_B1s`.
18. For Authentication type select SSH public key.
19. Use the `azureuser` for username (this is automatically generated).
20. Ensure RS SSH Format is selected.
21. Use the auto-generated Key pair name.
22. For Public inbound ports select Allow selected ports
23. Ensure SSH (22) is selected for Select inbound ports (should be selected by default).
24. Click on Next: Disk.
25. For OS disk size ensure `Image Default (30GiB)` is selected.
26. OS Disk type, select `Standard HDD`.
27. Click on Next.
28. Under Network Section, ensure the VNet is set to `nautilus-pub-vnet` and the subnet is `nautilus-pub-subnet`.
29. Under Public IP, ensure a new public IP is being created, if not create one.
30. Ensure the auto-generated network values are corrected and that SSH (22) port is allowed. 
31. Click on Review + Create.
32. Once the VM is launched we can SSH into it to ensure all is working.
33. Then click Check to complete the task. 
