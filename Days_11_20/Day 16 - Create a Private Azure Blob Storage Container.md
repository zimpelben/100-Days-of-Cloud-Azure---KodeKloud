As part of the data migration process, the Nautilus DevOps team is actively creating several storage containers on Azure. They plan to utilize private Blob containers to store the relevant data. Given the ongoing migration of other infrastructure to Azure, it is logical to consolidate data storage within the Azure environment as well.

Create a new storage account named `xfusionst7140` and a `private` Blob container named `xfusion-blob-30345` within the storage account. 

### Answer using Azure Portal

1. Sign into the Azure portal with the login credentials provided.
2. At the top, in the search bar, type storage. 
3. Click on `Storage accounts` under services.
4. In the Storage center overview page, click on the + Create. 
5. Assign the existing resource group and for Storage account name use `xfusionst7140`.
6. For Preferred storage type choose `Azure Blob Storage or Azure Data Lake Storage`
7. Click on Review + Create and create the Storage Account.
8. Once the Storage account is created successfully, click on go to resource.
9. In the Storage account overview page, click on Data storage on the left hand side.
10. Click on `Containers` and then click on + Add.
11. For name use `xfusion-blob-30345` and ensure container is private.
12. Once container is created click Check to submit the task.

### Answer using Azure CLI

1. Run the command az group list to get the resource group name.
2. Copy the name as this is needed in the next step. 
3. To create the storage account use the following command.

```
az storage account create \
--resource-group kml_rg_main-******** \
--name xfusionst7140 \
```

4. Once the storage account has been created, use the following command to create the iblob container;

```
az storage container create \
--name xfusion-blob-30345 \
--account-name xfusionst7140
```

By default the container is private ("off") therefore we do not need to specify it in the command.

4. Click on Check to complete the task. 
