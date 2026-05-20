The Nautilus DevOps team is migrating a portion of its infrastructure to Azure. During the migration, they have created several virtual machines (VMs) in different regions. The team identified one VM that is experiencing increased workload demands and requires additional compute resources to maintain optimal performance. 

1. Change the VM size from `Standard_B1s` to `Standard_B2s` for the virtual machine named `xfusion-vm`.
2. Ensure the VM is in the `running` state after the size change is complete.

**Notes**:
- Create the resources only in `eastus` region.
- Make sure the VM is in the `Running` state after resizing. 

### Answer using Azure Portal 

1. Login to the console using the login details provided.
2. Click on Virtual machines. 
3. Click on the `xfusion-vm`.
4. Stop the VM and wait for the status to be stopped (deallocated). 
5. Expand Availability + scale in the left hand panel and click on Size.
6. Change the size from `Standard_B1s` to `Standard_B2s` and click Resize at the bottom left corner.
7. Go back to overview and start the VM again and wait until the status is running before click on check task. 

**Note**: 
VMs can be resized without stopping but resizing them will they are running will cause them to restart. 

When stopping the VM and deallocating the VM, the VM will release any dynamic IP addresses assigned. 

In both instances, resizing a VM should be considered a disruptive operation and can impact other workloads which should be taken into considerations. 
