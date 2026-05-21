The Nautilus DevOps team needs to expand the storage capacity of an existing virtual machine and add an additional data disk to support increased workloads. This task requires resizing the existing VM disk and mounting a new data disk to the VM. 

As a member of the team, perform the following steps:

1. Expand the existing VM `xfusion-vm` disk from `32Gi` to `64Gi`.
2. Also create new standard HDD data disk named `xfusion-disk` of `64Gi` and mount the disk to VM `xfusion-vm` at location `/mnt/xfusion-disk`.

### Answer using Azure CLI

1. Run `az group list` to get the name of the resource group.
2. Next we have to deallocate the VM so the current disk attached, will be come unattached and can be resized. Run the following command;

```
az vm deallocate \
--resource-group kml_rg_main-******** \
--name xfuions-vm
```

3. To identify the disk we can use the following query.

```
az disk list \
--resource-group kml_rg_main-******** \
--query '[*].{Name:name,size:diskSizeGB,Tier:sku.tier}' \
--output table
```

It is also possible just to run `az disk list` or `az disk list --output table`. Both will work but the query method provides the cleanest output and is useful if multiple disks are in one resource group. 

4. Now the disk has been identified, we can resize it with the following command;

```
az disk update \
--resource-group kml_rg_main-******** \
--name xfusion-vm_OsDisk_1_************************* \
--size-gb 64
```

5. Once the disk has been resized the VM can be started again using the command;

```
az vm start \
--resource-group kml_rg_main-******** \
--name xfusion-vm
```

6. Create a new data disk and attach it using the following command;

```
az vm disk attach \
--resource-group kml_rg_main-******** \
--vm-name xfusion-vm \
--name xfusion-disk \
--sku Standard_LRS \
--size-gb 64 \
--new
```

7. Once the disk is attached, find out the public IP of the VM so we can SSH into it;

```
az network public-ip list \
--resource-group kml_rg_main-******** 
```

8. Use the public IP to SSH into the VM.

```
ssh azureuser@<public_ip>
```

8. Inside the identify the disk name by using `lsblk`.
9. We can now create a partition and filesystem on the disk.

```
# Create a single partition spanning the entire disk
sudo parted /dev/sdc --script mklabel gpt mkpart primary ext4 0% 100%

# Create a filesystem on the disk
sudo mkfs.ext4 /dev/sdc1
```

10. Lastly we create the mounting point `/mnt/xfusion-disk` and mount the disk to the location;

```
# Create mounting point
sudo mkdir -p /mnt/xfusion-disk

# Mount disk to location
sudo mount /dev/sdc1 /mnt/xfusion-disk
```

11. We can double check with `lsblk` that the disk is mounted and then click check to complete the task. 
