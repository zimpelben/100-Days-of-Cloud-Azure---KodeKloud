The Nautilus DevOps team is strategically planning the migration of a portion of their infrastructure to the Azure cloud. Acknowledging the magnitude of this endeavor, they have chosen to tackle the migration incrementally rather than as a single, massive transition. Their approach involves creating Virtual Networks (VNets) as the initial step, as they will be provisioning various services under different VNets.

Create a Virtual Network (VNet) named `xfusion-vnet` in the `southcentralus` region with `192.168.0.0/24` IPv4 CIDR.

### Answer using Azure Portal

1. Sign into the Azure portal with the login credentials provided.
2. At the top, in the search bar, type virtual network. 
3. Click on virtual networks.
4. Click + Create.
5. Ensure a resource group is selected, use the existing resource group.
6. Under Virtual network name enter `xfusion-vnet`.
7. Ensure (US) South Central US is selected under Region. 
8. Click Next twice, until you are on Address space.
9. Ensure an IPv4 Address space is added, by default it should already have `10.0.0.0/16` pre-configured.
10. Change the `10.0.0.0` to `192.168.0.0` and select `/24`. 
11. Press next twice until on the Review + Create section. 
12. Review the setup and then press Create at the bottom. 
13. Wait until Deployment is complete and then click on Go to resource to view the VNet.
14. Click on Check to complete the task.

### Answer using Azure CLI

1. Run the command az group list to get the resource group name.
2. Copy the name as this is needed in the next step. 
3. Run the following command to create the VNet;

```
az network vnet create \ 
--name xfusion-vnet \ 
--resource-group kml_rg_main-********\ 
--address-prefixes 192.168.0.0/24 \ 
--location southcentralus
```

4. Once the VNet is created you can list is using `az network net list`, alternatively, login to the Azure portal and check under virtual networks if it is listed.
5. Click on Check to complete the task. 
