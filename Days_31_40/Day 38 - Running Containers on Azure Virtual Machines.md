The Nautilus DevOps Team needs to set up an Azure Virtual Machine (VM) to interact with an Azure Blob Storage container for storing and retrieving data. The team must create a private storage account, configure Blob Storage, and test the fucntionality.

Task:

1. Azure Virtual Machine Setup:

	- The VM named `xfusion-vm` already exists in the East US region.

2. Create a Private Storage Account and Blob Container:

	- Create a storage account named `xfusionstor28932` in the East US region with Locally-redundant storage (LRS).
	- Create a private Blob container named `xfusion-container28932`.

3. Retrieve Storage Account Key:

	- Ge the storage account's access key to configure access for the application.

4. Create a Test File:

	- SSH into the VM and create a file named `testfile.txt` in the `/home/azureuser` directory with content: "this is a test file".

5. Upload the File to Blob Storage:

	- Upload the `testfile.txt` file to the Blob container `xfusion-container28932` using the Azure CLI command:
```
az storage blob upload --account-name xfusionstor28932 --account-key <access-key> --container-name xfusion-container28932 --name testfile.txt --file /home/azureuser/testfile/txt
```

**Notes**:
- Create the resources only in the `East US` region.
- Use the Azure Portal or Azure CLI for resource creation.
- Ensure the storage account is private and secure. 

### Answer using Azure CLI

1. Run the command az group list to get the resource group name.
2. Copy the name as this is needed in the next step. 
3. To create the storage account use the following command.

```
az storage account create \
--resource-group kml_rg_main-******** \
--name xfusionstor28932 \
--location eastus \
--sku Standard_LRS \
--allow-blob-public-access false
```

4. Once the storage account has been created, use the following command to create the blob container;

```
az storage container create \
--name xfusion-container28932 \
--account-name xfusionstor28932 \
--resource-group kml_rg_main-********
```

By default the container is private ("off") therefore we do not need to specify it in the command.

5. To list the account access-key use the following command;

```
az storage account keys list \
--account-name xfusionstor28932 \
--resource-group kml_rg_main-********
```

6. Copy the value and ssh into the `xfusion-vm` to create the test file.

```
# List the public IP of the VM
az network public-ip list -o table

# SSH into VM
ssh azureuser@<public-ip>

# Create the file with the content
echo '"this is a test file"' > testfile.txt

# Upload the test file
az storage blob upload --account-name xfusionstor28932 --account-key <access-key> --container-name xfusion-container28932 --name testfile.txt --file /home/azureuser/testfile/txt

# Verify upload was successful
az storageblos list \
--account-name xfusionstor28932 \
--container-name xfusion-container28932 \
--account-key <access-key> \
-o table
```

7. If file is successfully upload, click Check to complete task. 
