The Nautilus DevOps team needs to set up a new Virtual Machine (VM) on the Azure cloud that can be accessed securely from their landing host (`azure-client`). Follow the steps below to complete this task:

1. **Create an SSH Key**: On the `azure-client` host, check if an SSH key already exists. If id doesn't exist, create a new SSH key on the `azure-client` host that will be used for password-less SSH access.
2. **Create a Virtual Machine**: Use the Azure Portal or Azure CLI to create a new Virtual Machine named `datacenter-vm` in the `westus` region. Set the VM size to **Standard_B1s** and configure the VM with **SSH access** for the `azureuser` account using the newly created SSH key.
3. **Configure SSH Access**: Ensure that the SSH key from the `azure-client` host is added to the `azureuser` account on `datacenter-vm`, enabling secure, password-less SSH access from the `azure-client` host.
4. **Verify Connectivity**: Test the connection from `azure-client` to `datacenter-vm` using SSH to confirm that password-less access has been set up correctly.

Complete these tasks entirely within the Azure Portal or Azure CLI. 

### Answer using Azure CLI

1. Check if any existing SSH keys are on the host by checking the `.ssh` directory.
2. If no existing keys, generated a key using the following command;

```
ssh-keygen -t rsa -b 4096
```

- `-t` = type 
- `-b`= bits

3. Use `az group list` to see if a resource group already exist that is need to create the VM. If one exist copy the name to use in the next step. 
4. Create the VM with the specific configuration using the follow command;

```
az vm create \
--resource-group kml_rg_main-******** \
--name datacenter-vm \
--location westus \
--image Ubuntu2204 \
--size Standard_B1s \
--storage-sku Standard_LRS \
--data-disk-sizes-gb 30 \
--admin-username azureuser \
--ssh-key-values ~/.ssh/id_rsa.pub
```

Key parameters for this VM.
- `--location` = `westus`
- `--ssh-key-values` = Ensure the file points to the public key (`.pub`)

5. Note down the public IP in the output of the VM creation and then verify you can SSH into the VM using the following command;

```
ssh azureuser@<public_ip>
```

6. If login is successful without password, click check to end the task. 
