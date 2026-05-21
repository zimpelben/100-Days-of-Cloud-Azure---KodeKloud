The Nautilus DevOps Team is working on setting up a new virtual machine (VM) to host a web server for a critical application. The Team lead has requested you to create an Azure VM that will server as a web server using Nginx. This VM will be part of the initial infrastructure setup for the Nautilus project. Ensuring that the server is correctly configured and accessible from the internet is crucial for the upcoming deployment phase.

As a member of the Nautilus DevOps Team, your task is to create a VM using Azure CLI with the following specifications:

**Instance Name**: The VM must be named `devops-vm`.

**Custom Script Extension/User Data**: Configure the VM to run a custom script during its launch. This script should:

- Install the Nginx package.
- Start the Nginx service.

**Network Security Group (NSG)**: Ensure that the VM allows HTTP traffic on port `80` from the internet.

**Instructions**:

1. Use Azure CLI commands to set up the VM in the specified configuration. 
2. Ensure the VM is accessible form the internet on port 80.
3. The Nginx service should be running after setup.

Use the Azure CLI commands to complete the task.

`Notes`:
- Create the resources only in the `East US` region.
- You may use the default resource group or create a new one if needed. 

### Answer using Azure CLI

1. Run `az group list` to get the resource group.
2. Copy the resource group name as this is needed for the VM creation.
3. Next create a `cloud-init.txt` file to host the script. This will make it easier to add to the VM creation in later on.
4. Create `cloud-init.txt` with your preferred editor.

```
#cloud-config
package_update: true
packages:
  - nginx

runcmd:
  - systemctl enable nginx
  - systemctl start nginx
```

5. Next create the VM with the following specifications. 

```
az vm create \
--resource-group kml_rg_main-******** \
--name devops-vm \
--image Ubuntu2204 \
--size Standard_B1s \
--storage-sku Standard_LRS \
--data-disk-sizes-gb 30 \
--admin-username azureuser \
--generate-ssh-keys \
--user-data /root/config-init.txt
```

It should default to use the `East US` location on creation but if not, include `--location eastus` in the command.

6. Copy the public IP address form the output for later. 
7. Next open the port `80` to allow traffic from the internet to reach the Nginx server.

```
az vm open-port \
--resource-group kml_rg_main-******** \
--name devops-vm \
--port 80 \
--priority 1010
```

Added the `--priority` as not to have a conflict with the SSH rule. 

8. In your browser go to the `<public-ip>:80` to see if the Welcome to nginx! server message is up. If so the server is running. 
9. Click check to complete the task. 
