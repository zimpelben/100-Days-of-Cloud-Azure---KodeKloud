The Nautilus DevOps Team attempted to deploy an Nginx server on an Azure VM in a public VNet named datacenter-vnet, but they were unable to complete the setup successfully, and the server remains inaccessible from the internet.

As a DevOps team member, complete the following tasks:

1. **Verify VNet Configuration**: Ensure `datacenter-vnet` allows internet access.
2. **Attach Public IP**: A public IP named `datacenter-pip` already exists. Attach this public IP to the VM `datacenter-vm` to make it accessible from the internet.
3. **Ensure Accessibility**: Confirm the VM `datacenter-vm` is accessible on port 80.
4. **Install and Configure Nginx**: Install Nginx on `datacenter-vm` and ensure the service is enabled and running, listening on port 80.

Use the provided Azure credentials to troubleshoot and resolve the issue.

`Notes:`

- Create resources only in the `West US` region.
- Ensure the Network Security Group (NSG) is attached to the VM's NIC or subnet and configured to allow HTTP traffic on port 80.

### Answer using the Azure Portal

#### 1. Verify VNet Configuration:

1. Sign into the Portal using the credentials provided.
2. Search for vnet in the top search bar and click on `datacenter-vnet`.
3. Next click on subnets either in the essentials panel or by going on settings and subnets. 
4. In Subnet panel, we can see that the is a routing table assigned. 
5. When looking up at the routing table it, it has blocked internet rule. 
6. Go to Settings and Routes on the left had site to edit the route.
7. Click on the 3 dots on the right and delete the Block-Internet route.
8. One the route is deleted create a new one with the following details;
	- name = Internet
	- IP Address = 0.0.0.0/0
	- Next Hop = Internet
9. Once the route is added, we can move onto attaching the public IP to the VM.

#### 2. Attach Public IP:

1. Search for ip in the top search bar and `datacenter-pip` should appear.
2. On the overview page click Associate and ensure the following options are set;
	- Resource type = Network Interface
	- Network Interface = `datacenter-vmVMNIC`
3. Click Ok.
4. Double check that under Associated to in the overview page, the `datacenter-vmVMNIC` appears. 

#### 3. Ensure Accessibility:

1. In the top search, type vm and click on the `datacenter-vm`.
2. In the overview page, go to network and network settings on the left hand side. 
3. Check if there is an inbound rule to allow SSH - in this case yes.
4. Check if there is an inbound rule to allow HTTP - in this case not.
5. Create a new inbound rule to allow HTTP access, use the following parameters;
	- Source = Any
	- Source port ranges = *
	- Destination = Any
	- Service = HTTP
	- Destination port range = 80
	- Protocol = TCP
	- Action = Allow
	- Priority = 1010
	- Name = Allow-HTTP
6. Once the rule is created, copy the public IP and enter it in the browser and check if the VM is reachable on port 80.  
7. If nothing appears then the Nginx server is not running. 

#### 4. Install and configure Nginx:

1. As the SSH is allowed, will attempt to SSH into the VM to see if we can connect.
2. First, we need to get the ssh key to get into the VM which we can do in the Azure CLI.
3. Run the command to see if any public keys are available

```
az sshkey list --resource-group kml_rg_main-*********
```

4. If not then we just use the following command to SSH into the VM.

```
ssh -i .ssh/id_rsa azureuser@<public-ip>
```

5. Once inside the VM we need to install Nginx using the following commands;

```
sudo apt update # update package list.

sudo apt install nginx -y # install nginx with the yes flag. 

sudo systemctl enable nginx # ensures Nginx starts on VM reboot.

sudo systemctl status nginx # check if Nginx is running.

sudo systemctl start nginx # To start the nginx should it not be running after install.
```

6. Once Nginx is installed and running, we can double check that the port `80` is open with the following command;

```
sudo ss -ltunp
```

The command shows all of the ports that are open and here is a breakdown of flags.

- **-l**: Only display sockets currently listening for connections.
- **-t**: Filter to show only TCP connections.
- **-u**: Include UDP connections.
- **-n**: Show numeric values (such as port numbers) instead of resolving service names.
- **-p**: Display the process using each socket (root privileges are required to view processes owned by root).

7. Finally, exit the VM using the command `exit` and then we use `curl <public-ip` to see if we can a html response back. Alternative, we can just go to the public IP in the browser and should get the page 'Welcome to nginx!'.
8. Click check to complete the task

**Note**:

We install the Nginx server within the VM manually and should the VM ever have to restart or be replaced, the server will dissappear as well. To avoid this we can add the following User data config under the Settings - Operating System. This would ensure that if the VM is restarted or goes down, the Nginx server is installed an started.
