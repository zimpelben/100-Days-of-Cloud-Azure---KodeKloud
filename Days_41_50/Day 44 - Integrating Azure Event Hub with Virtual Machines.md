The Nautilus DevOps team wants to integrate an Azure Virtual Machine with Azure Event Hubs for centralized log collection. Follow these steps to complete the task:

1. Create Azure Event Hubs Namespace:
	- Create an Event Hubs namespace named `xfusion-namespace` in the East US region.
	- Select the Standard pricing tier. Make sure to enable `Enable Auto-inflate`.

2. Create an Event Hub:
	- Within the namespace, create an Event Hub named `xfusion-hub`.

3. Verify the Virtual Machine Configuration:
	- A VM named `xfusion-vm` already exists.
	- A Python script named `send_logs.py` already exists on the VM under `/home/azureuser`. This script is sued to send logs to the Event Hub. Make sure to execute this script multiple times.

4. Verify Logs:
	- Ensure the logs are successfully send to the Event Hub by checking the Event Hubs metrics in the Azure portal.

**Notes**:
- Create the resources only in the `East US` region.
- Use the existing VM `xfusion-vm` to send logs.
- Verify the Event Hubs metrics to confirm successful log ingestion.

### Answer using Azure Portal

1. Creating Azure Event Hubs Namespace
	1. Log in to the portal using the credentials provided.
	2. Search for event hubs in the top search bar and click on Event Hubs.
	3. Click + Create to create the namespace.
		- Namespace name = `xfusion-namespace`
		- Region = `East US`
		- Pricing tier = Standard
		- Throughput Units = 1
		- Enable Auto-Inflate = true
	4. Click Review + create to create the namespace.

2. Creating Event Hub:
	1. Go to the namespace resource and click + Event Hub.
		- Name = `xfusion-hub`
		- Leave the rest as default and click Review + Create.
	2. Go back to the namespace and go into Settings and Shared access policies.
	3. Copy the Primary connection string to your clipboard, as will need to enter this into the python code on the VM. 

3. Verify VM Config and execute script.
	1. Click on the resource group to see all resources.
	2. Click on `xfusion-vm` and VM is running and allows SSH traffic under Network settings.
	3. Go to the `azure-client` and SSH into the VM using the public IP.
	4. Navigate to `/home/azureuser` and edit the python code.
	5. Paste the connection string, copied in the previous step, under `connection_str`.
	6. Save and exit the script.
	7. Execute the script via `python3 send_logs.py` multiple times.

4. Verify Logs:
	1. Go back to the Azure Portal.
	2. On the overview page, under Request, we should have multiple request noted.
	3. We can also check under Entities - Event Hubs - `xfusion-hub` directly.
	4. Click check to complete the task.
