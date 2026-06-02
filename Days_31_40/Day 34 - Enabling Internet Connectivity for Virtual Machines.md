The Nautilus DevOps team has encountered an issue with an Azure VM named `devops-vm`. They are unable to install any packages on this VM due to connectivity issues. The team needs to identify the root cause of the problem and resolve it to restore normal operations.

1. Investigate the connectivity issue preventing package installation on the Azure VM `devops-vm`.
2. Implement a solution to resolve the connectivity issue and restore package installation capabilities on the VM. 

**Note**: The SSH key required to access the Azure VM is already created and added to the VM's authorized keys. You can find the SSH key at `/root/.ssh/id_rsa` on the `azure-client` host.

**Notes**: 
- Create the resources only in the `East US` region.

### Answer using Azure Portal

1. Login into the portal with the credentials provided.
2. On the home page click on virtual machines.
3. Click on the `devops-vm`.
4. On the VM overview page, click on Networking and Network settings on the left hand side.
5. Expand the Outbound port rules.
6. In this instance there is a `Block-All-Outbound` rule with a priority of `200` which is the causing the issues.
7. By deleting this rule, this will resolve the issue and allow the VM in future to connect to external resource. 
8. To verify the issue is resolved, let's SSH into the VM and attempt to update the package list. 
9. Make a note of the public IP and use the following command to SSH into the VM;

```
ssh azureuzer@<public_ip>
```

10. Once inside the VM, lets attempt to update package list;

```
sudo apt update
```

11. Alternative, we can attempt to ping the Google DNS server.

```
ping -c 3 8.8.8.8
```

12. To complete the task just click Check.
