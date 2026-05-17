The Nautilus DevOps team has already set up a virtual machine and allocated a public IP address. The final task is to attach this public IP to the VM's network interface card (NIC).

An existing VM named `datacenter-vm-pip` and a public IP address named `datacenter-pip` already exist.

Attach the public IP `datacenter-pip` to the network interface of the VM `datacenter-vm-pip`.

Make sure the VM is properly assigned the public IP. 

### Answer using Azure Portal

1. Sign into the Azure portal with the login credentials provided.
2. At the top, in the search bar, type ip. 
3. Click on `datacenter-pip` under resources.
4. In the IP overview page, click on the Associate. 
5. In the Associate panel, for Resource type select Network interface.
6. For Network interface select the `datacenter-vm-pipVMNIC`.
7. In the overview page under Virtual machine, `datacenter-vm-pip` will be displayed.
8. Click on `datacenter-vm-pip` and in the overview page of the VM, a public IP will be listed under Networking.
9. Click Check to submit the task.

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
