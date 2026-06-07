The Nautilus DevOps Team want to integrate an Azure Virtual Machine with Azure Event Hubs and Azure Blob Storage for centralized log collection and backup. Follow these steps to complete the task:

1. Create Azure Event Hubs Namespace:
	- Create an Event Hubs namespace named `devops-namespace` in the East US region.
	- Select the Standard pricing tier. Make sure to enable `Enable Auto-inflate`.

2. Create an Event Hub:
	- Within the namespace, create an Event Hub named `devops-hub`.

3. Set Up Azure Blob Storage for Log Backup:
	- Create a Storage account named `devopsst195759` in the East US region.
	- Create a container named `devops-backup-19413` within the Storage Account.
	- Ensure the container is publicly accessible for read operations.

4. Verify the Virtual Machine Configuration:
	- The client host already has a python script named `send_logs.py` located under `/root`. This script is used to send logs to the Event Hub.
	- Create a Virtual Machine named `devops-vm` in the East US region.
	- Copy the `send_logs.py` script form the client host to the `/home/azureuser` directory of the `devops-vm`.
	- Modify the script on the VM to also back up the logs to the `devops-backup-19413` container in the Azure Blob storage account.

5. Verify Logs:
	- Ensure the logs are successfully send to the event hub by checking the event hubs metrics in Azure portal.
	- Verify that the logs are backed up to the `devops-backup-19413` container in the Azure Blob storage.

**Notes**: 
- Create the resources only in the `East US` region.
- Use the existing client host to copy the script to the VM.
- Verify both the event hubs metrics and the blob storage container for successful log ingestion and backup.

### Answer using Azure Portal

1. Creating Azure Event Hubs Namespace:
	1. Login to the Azure Portal with the credentials provided.
	2. Search for event hub in the top search bar and go Event Hubs.
	3. Click create on the overview page.
	4. Use the following config;
		- Namespace name = `devops-namespace`
		- Region = `East US`
		- Pricing Tier = Standard
		- Enable Auto-Inflate = true
	5. Click on Review and Create and create namespace.

2. Creating an Event Hub:
	1. Got to resource once the namespace as been deployed.
	2. Under entities click on Event Hubs.
	3. Click + Event Hub
	4. Enter the name = `devops-hub` and click Review + Create.
	5. Create the hub and ensure Status is active.

3. Creating Azure Blob Storage
	1. Search for storage accounts in the top search bar and click on storage accounts.
	2. Click + Create.
	3. Use the following configs for storage account;
		- Storage account name = `devopsst19759`
		- Region = `East US`
		- Preferred Storage type = Azure Blob Storage or Azure Data Lake Storage
		- Ensure Anonymous access is enabled.
		- Click Review + Create to create the storage account.
	4. Once the account has been created, click on go to resource.
	5. Click on Data storage and Containers.
	6. Click + Add container.
		- Name = `devops-backup-19413`
		- Anonymous access level = Blob (anonymous read access for blobs only)
			- If the option is not available, go to Storage account Settings - Configurations and enable `Allow Blobl anonymous access`.
	7. Once the container is created we can move on to the VM creation.

4. VM creation and configuration. 
	1. As we need to SSH into the VM to copy the script, it will be easier to create the VM via the `azure-client`.
	2. Copy the resource group from the portal or use the `az group list -o table` to see it in the `azure-client`.
	3. Use the following config to create the vm;

```
az vm create \
--name devops-vm \
--resource-group kml_rg_main-****** \
--location eastus \
--image Ubuntu2204 \
--size Standard_B2s \
--data-disk-sizes-gb 30 \
--storage-sku Standard_LRS \
--admin-user azureuser \
--generate-ssh-keys
```

5. Once the VM has been created, note down the public IP from the VM output and copy the script using the following command onto the VM;

```
scp send_logs.py azureuser@<public-ip-vm>:/home/azureuser/send_logs.py
```

6. SSH into VM and configuring script:
	1. SSH into VM and locate script. 
	2. Next open the script with any editor to modify the script. 
	3. Go back to the Event Hub namespace in Azure Portal and go to Settings and Shared access Policies.
	4. Copy the Primary connection string and paste it into the script under `eventhub_conn_str`.
	5. Go to the Storage Account in the Portal and go to Security + networking and Access Keys, and copy the Connection string under key 1. 
	6. Paste the value in the script under `blob_conn_str`.
	7. Save and quiet the editor.
	8. Run the script with `python3 send_logs.py`

**Notes**:
	- If encountering any errors, it could be that azure-storage-blob and azure-eventhub python modules are not installed.
	- If that is the case install using the following commands

```
sudo apt install python3-pip

python3 -m pip install azure-storage-blob

python3 -m pip install azure-eventhub
```

7. Verify logs:
	1. If the script runs successful, the output message of the script will be shown.
	2. Check the Azure Portal, if the Metrics under the event hub show any logs.
	3. If logs are shown, double check the container to see if any log files have been stored. 
	4. If metrics and log files are backup, the task is complete and check can be clicked. 
