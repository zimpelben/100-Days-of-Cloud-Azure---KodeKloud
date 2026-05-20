The datacenter DevOps team is migrating services to Azure. They are breaking down tasks to ensure better control and optimization. You are tasked with attaching an existing network interface (NIC) to a virtual machine (VM).

An existing VM named `datacenter-vm` and a network interface named `datacenter-nic` already exist in the `southcentralus` region.

- Attach the network interface `datacenter-nic` to the VM `datacenter-vm`.
- Ensure the NIC's status is attached before submitting the task.

Make sure the virtual machine initialization has been completed before submitting this task. 

### Answer using Azure Portal

1. Sign into the Azure portal with the login credentials provided.
2. At the top, in the search bar, type vm. 
3. Click on Virtual Machines.
4. Click on the existing VM. 
5. In the Overview panel, click on Stop VM. (A network interface cannot be attached to a running VM).
6. Once the VM is stopped, click on Networking and Network settings on the left hand side. 
7. Click on Attach network interface.
8. From the drop down menu select the existing NIC `datacenter-nic`.
9. Press Ok and wait for the NIC to attach. 
10. Go back to Overview and Start the VM. 
11. Wait for the Status of the VM to update to Running before submitting task.

### Answer using Azure CLI

1. Run the command az group list to get the resource group name.
2. Copy the name as this is needed in the next step. 
3. The VM needs to be stopped for the NIC can be attached, therefore run the following command to stop the VM.

```
az vm stop \
--resource-group kml_rg_main-******** \
--name datacenter-vm
```

4. Once the VM has stopped, run the deallocate command  as the status of the VM has to be deallocated before adding the NIC.

```
az vm deallocate \
--resource-group kml_rg_main-******** \
--name datacenter-vm
```

5. After deallocation, add the NIC to the VM.

```
az vm nic add \ 
--nics datacenter-nic \ 
--vm-name datacenter-vm \
--resource-group kml_rg_main-******** 
```

5. Start the VM again with the following command.

```
az vm start \
--resource-group kml_rg_main-******** \
--name datacenter-vm
```

5. Once the command has executed there will be no output. Run the command `az vm list`, to view the VM details including attached disks..
6. Click on Check to complete the task. 

