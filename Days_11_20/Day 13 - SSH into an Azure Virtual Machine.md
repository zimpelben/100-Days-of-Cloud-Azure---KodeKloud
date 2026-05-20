The Nautilus DevOps team is working on setting up secure SSH access for their virtual machines in Azure. One of the requirements is to add the SSH public key of the root user from the Azure client host (landing host) to the `nautilus-vm` Azure VM's `authorized_keys` file. This ensures secure and password-less SSH access to the VM. 

Task Details:

1) VM Details:
	- The VM is named `nautilus-vm` and is running in the `eastus` region. The default SSH user is `azureuser` - use this user to connect to the VM.
	- You need to add the root user's SSH public key from the Azure client host to the `authorized_keys` file of the VM's root user. 
	- The SSH public key of the root user on the Azure client host is located at `/root/.ssh/id_rsa.pub`. 
2) Public Key Addition:
	- Copy the public key located at `/root/.ssh/id_rsa.pub` on the Azure client host to the `authorized_keys` file of the root user on `nautilus-vm`.
	- Ensure that the proper permissions for the `.ssh` folder and `authorized_keys` file are set on the VM. 
3) Verification: 
	- After adding the public key, make sure that you are able to SSH into the `nautilus-vm` VM as the `root` user from the Azure client host without needed a password. 

**Notes**:
- Ensure that the VM is up and running before attempting to SSH.
- You may need to adjust the firewall or security group rules for the VM to allow SSH access. 

### Answer using Azure CLI

1. Use the command `az network public-ip list` to find the public IP of the `nautilus-vm`.
2. Next copy the `id_rsa.pub` file onto the VM using the secure copy command.

```
scp /root/.ssh/id_rsa.pub azureuzer@public-ip:/tmp/
```

Note: the tmp directory gets cleared on every restart/shutdown in Linux. If we forgot to remove the file at least the file is deleted automatically should the VM restart or shutdown. 

From this point there are several ways of completing the task. 

```
ssh azureuser@public-ip # SSH back into the vm.

sudo -i # swith user to root (no password on root profile)

cat /tmp/id_rsa.pub > /root/.ssh/authorized_keys # add public to authorized keys file

rm /tmp/id_rsa.pub # clean up by removing public key file.

exit # logout of root user and then VM.

ssh root@public-ip # check if we can SSH into root user without password.
```

The second option is to do to it with in one command  using a shell script;

```
ssh -T azureuser@public-ip <<-EOF 
> sudo -i
> cat /tmp/id_rsa.pub > /root/.ssh/authorized_keys
> rm /tmp/id_rsa.pub
> EOF

ssh root@public-ip # check if we can SSH into root user without password.
```

Explanation:
- `ssh -T` this option tells SSH to connect without allocating a pseudo-terminal (tty).
- `<<-EOF` starts the here-document in this case for a shell script.
- We add all of the command into the script and then declare the end with `EOF`.
- We SSH into root to verify we can now login as root. 
