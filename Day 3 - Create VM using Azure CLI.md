The Nautilus DevOps team is in the process of migrating some of their workloads to Azure. One of the tasks involves creating a new Virtual Machine (VM) using the Azure CLI. The team does not have access to the Azure portal but can manage Azure resources via the `azure-client` host (the landing host for this lab).

1. Create a new Azure Virtual Machine named `devops-vm` using the Azure CLI.
2. Use the `Ubuntu2204` image and set the VM size to `Standard_B2s`.
3. Make sure the admin username is set to `azureuser` and SSH keys are generated for the secure access.
4. Use `Standard_LRS` storage account, disk size must be `30GB` and ensure the VM `devops-vm` is in the `running` state after creation. 

### Answer

1. Run the `az group list` command and copy the name of the resource group.
2. Run the following command;
```
az vm create -n devops-vm --resource-group kml_rg_main-****** \
--image Ubuntu2204 \
--size Standard_B2s \
--data-disk-sizes-gb 30 \
--storage-sku Standard_LRS \
--admin-user azureuser \
--generate-ssh-keys
```

3. Once the VM is created, note down the public IP and SSH into the VM with the command `ssh azureuser@<Public IP>.
4. Once in the VM, exit and then press Check to complete the task. 
