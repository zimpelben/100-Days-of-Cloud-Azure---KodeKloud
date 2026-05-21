The Nautilus DevOps Team has received a new request from the Development Team to set up a new Azure Virtual Machine (VM). This VM will be used to host a new application that requires a stable public IP address. To ensure that the VM has a consistent public IP, a Static Public IP address needs to be associated with it. The VM will be named `xfusion-vm`, and the Static Public IP will be named `xfusion-pip`. This setup will help the Development Team to have a reliable and consistent access point for their application. 

1. Create an Azure VM named `xfusion-vm` using any available Ubuntu image, with the VM size `Standard_B1s`.
2. Generate an SSH public key on the `azure-client` host and associate it with the VM for SSH access. 
3. Associate a Static Public IP address named `xfusion-pip` with this vm.
4. Ensure the VM is accessible via SSH using the generated public key. 

### Answer using Azure CLI

1. Run `az group list` to get the resource group.
2. Create the Public IP first using the following command;

```
az network public-ip create \
--name xfusion-pip \
--resource-group kml_rg_main-******** \
--allocation-method Static
```

3. Next create the VM and associate the public IP under the `--public-ip-address`;

```
az vm create \
--resource-group kml_rg_main-******** \
--name xfusion-vm \
--image Ubuntu2204 \
--size Standard_B1s \
--storage-sku Standard_LRS \
--data-disk-sizes-gb 30 \
--admin-username azureuser \
--generate-ssh-keys \
--public-ip-address xfusion-pip
```

4. Note the public IP address of the VM from the output and attempt to SSH into the VM to verify the SSH key working.

```
ssh azureuser@<public ip>
```

5. If successful you will be logged into the VM without a password and now can click check to complete the task.
