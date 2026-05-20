The Nautilus DevOps team is migrating services to Azure. They are breaking down tasks to ensure better control and optimization. You are tasked with attaching an existing data disk to a virtual machine (VM).

An existing VM named `devops-vm` and a managed disk named `devops-disk` already exist in the `southcentralus` region.

- Attach the disk `devops-disk` to the VM `devops-vm` as a data disk.
- Ensure the disk is attached to the VM `devops-vm`.

Make sure that the virtual machine initialization has been completed before submitting this task. 
### Answer using Azure Portal

1. Sign into the Azure portal with the login credentials provided.
2. At the top, in the search bar, type public ip. 
3.
### Answer using Azure CLI

1. Run the command az group list to get the resource group name.
2. Copy the name as this is needed in the next step. 
3. Run the following command to attach the managed disk;

```
az vm disk attach \ 
--vm-name devops-vm \ 
--name devops-disk \
--resource-group kml_rg_main-********\ 
```

4. Once the command has run and there will be no output. Run the command `az vm list`, to view the VM details including attached disks..
5. Click on Check to complete the task. 
