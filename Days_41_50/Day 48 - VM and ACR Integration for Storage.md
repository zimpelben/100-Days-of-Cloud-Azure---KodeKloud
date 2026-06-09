The Nautilus DevOps team needs to set up an Azure Virtual Machine (VM) to interact with an Azure Blob Storage container for storing and retrieving data. The team must create a private storage account, configure Blob Storage, and test the functionality.

### Task:

1) **Azure Virtual Machine Setup**:

- Create a VM named `devops-vm` in the **East US** region.
- Authentication: Use `SSH public key` authentication. (Please select `use existing public key` option, create public-key locally and paste contents of `~/.ssh/id_rsa.pub`).
- Install Docker and Azure CLI on the VM.
- Pull the Docker image from the ACR and run it on the VM, ensuring it listens on port 80.

2) **Azure Container Registry (ACR) Setup**:

- Create an ACR named `devopsacr3223` in the **East US** region.
- The repository name should be `devops/python-app`.
- Build the Docker image using the Dockerfile already given under `/root/pyapp`.
- Push the Docker image to the ACR with the tag `latest`.

3) **Create a Storage Account and Blob Container**:

- Create a storage account named `devopsstor3223` in the **East US** region with **Locally-redundant storage (LRS)**.
- Create a Blob container named `devops-config`.
- Upload a file named `config.json` (available under `/root/`) to the container.

4) **Validation**:

- Confirm that the application can be accessed on the browser.

**Notes:**

- Create all resources in the `East US` region.
- Use the Azure CLI or Azure Portal for resource creation.
- The Dockerfile is already given under `/root/pyapp`. The image tag must be `latest`.
- The repository name should be `devops/python-app`.

### Answer

**Azure Virtual Machine Setup**:
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

5. Copy the config.json file also into the VM as this will be needed for the python-app later.

```
scp azureuser@<public-ip-vm>:/home/azureuser/config.json
```

5. SSH into the VM, update the packages and install Docker and the Azure CLI.

```
# Udpate package list
sudo apt update 

# Install docker
sudo apt update
sudo apt install docker.io -y
sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -aG docker azureuser

# Install Azure CLI
curl -fsSL 'https://azurecliprod.blob.core.windows.net/$root/deb_install.sh' | sudo bash
```

7. Exit the VM when completed.

**Azure Container Registry (ACR) Setup**:

8. Login into the Azure Portal with the credentials provided.
9. Search for container registry and click on container registries.
10. Click create and use the following configuration;
	- ACR name = `devopsacr3223` 
	- Region = **East US** 
11. Once the ACR is created, copy the server address as its needed to push the image to the registry. 
12. Return to the `azure-client` and exit the VM, if still connected to the VM.
13. Navigate to `pyapp` directory and build the docker image.

```
cd pyapp

# Build docker image from Dockerfile
docker build . -t python-app:latest
```

7. Once the image is build, we can verify with the `docker image list` command.
8. Next we need to tag the image before uploaded it to the ACR using the following command;

```
# Tag docker imager
docker tag python-app:latest devopsacr3223.azurecr.io/devops/python-app:latest

# Push image to ACR
docker push devopsacr3223.azurecr.io/devops/python-app:latest
```

**Note** 
- The Server URL is followed by the repository name `/devops/python-app` as specified in the instructions above.

9. Once the upload is complete, double check the image is uploaded and the tag is latest in the Azure Portal. 

**Create a Storage Account and Blob Container**

1. Go back to the Azure portal and search for storage account in the top bar.
2. Click Create and use this configuration.
	- Storage account name = `devopsstor3223`
	- Region = East US
	- Storage type = Locally-redundant storage (LRS)
3. Click Review + Create, and create the account.
4. Once the account is created, click go to resource.
5. On the left hand side under Data storage, click on Containers.
6. Click Create and create a container with the name `devops-config`.
7. Click save and go back to the `azure-client`.
8. Upload the `config.json` file to the blob container.

```
az storage blob upload \
-f /root/config.json \
--container-name devops-config \
--account-name devopsstor3223 \
-n config.json
```

9. Once the upload is complete, double check in the Azure Portal that the file is in the container.

**Validation**

1. SSH into the VM.
2. Sign into the Azure CLI and the ACR.

```
az login

az acr login --name devopsacr3223
```

**Note**
- The acr login will require a username and password.
- Go to the Azure Portal and under Settings - Access key, tick the admin access box.
- This will show a admin username and a password which we can during the `az acr login`


3. Once that is complete we can pull the image from the ACR.

```
docker run -d \
-p 80:80 \
-v /home/azureuser/config.json:/app/config.json \
--name python-app \
devopsacr3223.azurecr.io/devops/python-app:latest
```

4. Once the image is running we can check if it works by going to the public IP of the VM in our browser. 
5. Click check to complete the task.
