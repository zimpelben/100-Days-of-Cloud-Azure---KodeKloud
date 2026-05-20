The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the Azure cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. 

For this task, create a Virtual Network (VNet) named `datacenter-vnet` and one subnet named `datacenter-subnet` within the VNet in the `westus` region. Make sure the `IPv4 address range` is `10.0.0.0/16`.

### Answer using Azure Portal

1. Sign into the Azure portal with the login credentials provided.
2. At the top, in the search bar, type virtual network. 
3. Click on virtual networks.
4. Click + Create.
5. Ensure a resource group is selected, use the existing resource group.
6. Under Virtual network name enter `datacenter-vnet`.
7. Ensure (US) West US is selected under Region. 
8. Click Next twice, until you are on Address space.
9. Ensure an IPv4 Address space is added, by default it should already have `10.0.0.0/16` pre-configured.
10. Under Subnets press the pencil icon to edit the default subnet. 
11. Change the name to `datacenter-subnet`.
12. Leave all other settings as they are and press save. 
13. Press next twice until on the Review + Create section. 
14. Review the setup and then press Create at the bottom. 
15. Wait until Deployment is complete and then click on Go to resource to view the VNet.
16. Click on Check to complete the task.

### Answer using Azure CLI

1. Run the command az group list to get the resource group name.
2. Copy the name as this is needed in the next step. 
3. Run the following command to create the VNet;

```
az network vnet create \ 
--name datacenter-vnet \ 
--resource-group kml_rg_main-********\ 
--address-prefixes 10.0.0.0/16 \ 
--subnet-name datacenter-subnet \ 
--subnet-prefixes 10.0.0.0/24
--location westus 
```

4. Once the VNet is created you can list is using `az network net list`, alternatively, login to the Azure portal and check under virtual networks if it is listed.
5. Click on Check to complete the task. 

