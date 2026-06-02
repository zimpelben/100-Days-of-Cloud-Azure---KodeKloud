The Nautilus DevOps team has been tasked with demonstrating the use of VNet Peering to enable communication between two VNets. One Vnet will be a private VNet that contains a private Azure VM, while the other will be a public VNet containing a publicly accessible Azure VM. 

1. Existing Azure Resources:
	- Public VM: `xfusion-pub-vm` is already in the public VNet.
	- Private VNet and VM: `xfusion-priv-vnet` and `xfusion-priv-vm` exist in the private VNet with its subnet: `xfusion-priv-subnet`.

2. Create VNet Peering:
	- Create a VNet Peering between the Public VNet and Private VNet.
	- VNet Peering Name: `xfusion-pub-to-priv-peering`.

3. Test the Connection:
	- SSH into the public VM and verify that you can ping the private VM. 

**Notes**:
- Create the resources only in the `East US` region.

### Answer using Azure Portal


#### Creating VNet Peering

1. Login with the credentials provided.
2. Search for virtual networks in the search bar and click on Virtual networks under services.
3. Click on the `xfusion-pub-vnet` and go to Settings and Peerings.
4. Click + Add to add a peering.
5. For peering link name use `xfusion-pub-to-priv-peering`.
6. For virtual network choose the `xfusion-priv-net`.
7. Enter the peering link name again under local virtual network summary.
8. Click Add to complete the peering.
9. Ensure Peering sync status and peering state have a green check mark before continuing.
10. Click on the `xfusion-priv-net` and check under Settings and Peerings that the peering is there as well. 

#### Checking connectivity

1. From VNet overview page, click on resource group to view all resources. 
2. Click on the `xfusion-priv-vm` and note down the private IP address. (10.1.1.4)
3. Click back on the resource group and click on `xfusion-pub-vm` to note down the public IP address. 
4. Now go back to the `azure-client` and SSH into the public VM.

```
ssh azureuzer@<public_ip>
```

5. Once inside the VM ping the private VM using the private IP address.

```
ping -c 3 <private_ip>
```

6. If the pings are successful, the setup is complete and we can click check to complete the task. 
