The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the Azure cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. To achieve this, they have segmented large tasks into smaller, more manageable units. This granular approach enables the team to execute the migration in gradual phases, ensuring smoother implementation and minimizing disruption to ongoing operations. By breaking down the migration into smaller tasks, the Nautilus DevOps team can systematically progress through each stage, allowing for better control, risk mitigation, and optimization of resources throughout the migration process. 

Create a managed disk with the following requirements:

- Name of the disk should be `datacenter-disk`.
- Disk `type` must be `Standard_LRS`.
- Disk `size` must be `2 GiB`.

### Answer using Azure Portal

1. Sign into the Azure portal with the login credentials provided.
2. At the top, in the search bar, type disks. 
3. Click on `disk` under services.
4. In the Storage center overview page, click on the + Create. 
5. Assign the existing resource group and for Disk name use `datacenter-disk`
6. Leave the remaining fields as is but for Size click on Change size.
7. Change the size to `Standard HDD` in the drop down menu and at the bottom change the size to `2 GiB1`. 
8. Click on Review + Create and create the disk.
9. Click Check to submit the task once the disk is been successfully created.

### Answer using Azure CLI

1. Run the command az group list to get the resource group name.
2. Copy the name as this is needed in the next step. 
3. To create the disk using the following command.

```
az disk create \
--resource-group kml_rg_main-******** \
--name datacenter-disk \
--sku Standard_LRS \
--size-gb 2 
```

4. Once the disk has been created, check the output to confirm the configuration. Alternative, we can login to the portal and check the disk there. 
5. Click on Check to complete the task. 
