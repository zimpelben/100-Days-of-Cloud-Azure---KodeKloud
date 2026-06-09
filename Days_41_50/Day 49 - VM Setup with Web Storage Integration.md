The Nautilus DevOps team is tasked with setting up an environment to host a static web application. The application will serve static content from an Azure Storage Account, and a Virtual Machine (VM) will be configured to fetch and display this content using Nginx. The Azure Storage Account is used as a secure, centralized location for storing the `index.html` file. The team intentionally keeps this file outside the main source code repository, since that repository contains additional internal application code that should not be exposed to or accessed by the VM. By placing only the required static file in the Storage Account, the team can distribute this asset safely and independently of the full codebase.

The VM should securely download the `index.html` blob directly from the designated container (e.g., using Azure CLI, SAS URL, or REST API) and place it in Nginx’s web root directory so that it is served locally by Nginx. The Storage Account is not mounted, and the Static Website feature is not used. The VM retrieves the file during deployment and may re-fetch it whenever updates are needed. The resources must follow best practices for security, performance, and accessibility.

### Task Details:

1) **Create a Virtual Network (VNet) and Subnet**:

- Create a VNet named `devops-vnet` in the **East US** region.
- Create a subnet named `devops-subnet` within the VNet for the VM.

2) **Create an Azure Storage Account**:

- Create a storage account named `devopsstor29413` in the **East US** region with **Locally-redundant storage (LRS)**.
- Create a Blob container named `devops-container` in the storage account.
- Upload the `index.html` file located at `/root` on the **client host** to the container `devops-container`.
- **Ensure the Storage Account is private and not publicly accessible** by disabling public access for the storage account.

3) **Create a Virtual Machine (VM)**:

- Create a VM named `devops-vm` in the **East US** region.
- Use the **devops-vnet** and subnet **devops-subnet** for the VM.
- Authentication: Use `SSH public key` authentication. (Please select `use existing public key` option, create public-key locally and paste contents of `~/.ssh/id_rsa.pub`)
- Install **Nginx** on the VM.
- Download the `index.html` file using a command such as:
    `sudo az storage blob download --account-name devopsstor29413 --account-key xxxxx --container-name devops-container --name index.html --file /var/www/html/index.html`
- Ensure Nginx is configured to serve the file from `/var/www/html/index.html`.


4) **Verify Setup**:
- Verify that the Nginx web server on the **client host** serves the `index.html` file correctly when accessing the VM's public IP address.

### Answer

**Create a Virtual Network (VNet) and Subnet**:

1. Login to the Azure Portal with the credentials provided.
2. Search for vnet in the top search bar and click on Virtual networks.
3. Create a VNet with the following configuration;
	- Vnet name = `devops-vnet`
	- Region = `East US`
	- Subnet = `devops-subnet` (rename the default subnet)
4. Click Review + Create to create the VNet.

**Create an Azure Storage Account**:

1. Go to Storage account.
2. Create new account with the following configuration;
	- Name = `devopsstor29413`
	- Region = East US
	- Redundancy = Locally-redundant storage (LRS)
3. Click Review + Create to create the account.
4. Next, go to resource and click on Data Storage and Containers.
5. Create a new container `devops-container` and ensure its Private.
6. Go to the `azure-client` and upload the `index.html` file to the container;

```
az storage blob upload \
-f /root/index.html \
--container-name devops-container \
--account-name devopsstor29413 \
-n index.html
```

**Create a Virtual Machine (VM)**:

1. Create a SSH key with the `ssh-keygen` command in the `azure-client`.
2. List the resource groups using `az group list -o table`.
3. Copy the resource group name as its need to create the VM.
4. Create the VM using the following configuration;

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
--ssh-key-values .ssh/id_rsa.pub
```

5. Once the VM has been created, note down the public IP.
6. We also have to open the port 80 which the python app will be listening on. 

```
az vm open-port \
--name devops-vm \
--port 80 \
--resource-group kml_rg_main-******
```

9. SSH into the VM.
10. Install the Nginx server;

```
sudo apt update
sudo apt install nginx -y
sudo systemctl start nginx
sudo systemctl enable nginx
sudo systemctl status nginx # check nginx is operational
```

11. Install the Azure CLI;

```
curl -fsSL 'https://azurecliprod.blob.core.windows.net/$root/deb_install.sh' | sudo bash
```

12. Sign into the Azure CLI and download the `index.html` file.

```
az login

sudo az storage blob download \
--account-name devopsstor29413 \
--account-key xxxxx \ # Get the Access Key from the storage account in the Portal
--container-name devops-container \
--name index.html \
--file /var/www/html/index.html`
```

13. Check the file is downloaded to `/var/www/html/`.
14. Enter the VM public IP into the browser and the website should display Welcome to KKE Azure Labs.
15. Click check to complete the task.
