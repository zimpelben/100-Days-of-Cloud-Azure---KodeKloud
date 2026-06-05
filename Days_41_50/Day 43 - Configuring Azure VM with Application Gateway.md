The Nautilus Development Team needs to set up a new Azure Virtual Machine (VM) and configure it to run a web server. This VM should be part of an Azure Application Gateway (AGW) setup to ensure high availability and better traffic management. The task involves creating a VM, setting up an AGW, configuring a backend pool, and ensuring the web server is accessible via the AGW public IP.

Create a Network Security Group (NSG): Create an NSG named `devops-nsg` and add an inbound security rule `Allow-HTTP` to allow `TCP` traffic on port `80`.

Create a Virtual Machine: Create a VM named `devops-vm` using any available Ubuntu image. Configure the instance with the following settings:
- Size: Choose a lightweight VM size (e.g., `Standard_B1s`).
- Authentication: Use `SSH public key` authentication. (Please select `use existing public key` option, create public-key locally and paste contents of `~/.ssh/id_rsa.pub`).
- OS Disk: Use a `Standard HDD`.
- Networking: Under the Advanced section, attach an existing NSG (e.g., `devops-nsg`).

Additionally, configure the instance to run a user data script during that:
- Install the Nginx package.
- Start the Nginx service.

Set up an Application Gateway: Set up an Azure Application Gateway named `devops-agw` with the following:
- Create and Associate it with a public IP address named `devops-agw-ip`.
- Attach the backend pool: `devops-backendpool` to the VM `devops-vm`.
- Select a subnet for the Application Gateway (you can create a new one if needed).

Configure HTTP Settings: Create an HTTP setting named `devops-http-settings` on port `80`.

Route Traffic: Add a listener named `devops-listener` and a routing rule named `devops-routing-rule` to route traffic from the AGW frontend to the backend pool:
- Listener: Frontend IP = public IP, Frontend Port = 80, Protocol = HTTP.
- Routing rule: Connects `devops-listener` to `devops-backendpool` using `devops-http-settings`.

NSG Adjustments: Make sure the NSG attached to the VM allows inbound TCP traffic on port 80, so the Nginx server running on `devops-vm` is accessible via the Application Gateway public IP.

**Note**: Wait for the Application Gateway resource to be fully deployed before proceeding with the next steps. Deployment may take several minutes to complete. Create the resources only in the West US region. 

### Answer using Azure Portal

#### Creating NSG
1. Sign in with the credentials provided.
2. Search for NSG in the top search bar and click on Network Security Group.
3. On the overview page click Create.
4. Create NSG with the following details:
	- name = `devops-nsg`
	- Region = West US
5. Then click Review and Create.
6. Once the NSG is created, go to resource and add Inbound rule.
7. On the left hand side go to Settings and Inbound security rule.
8. Add the following rule:
	- Source = Any
	- Source port ranges = *
	- Destination = Any
	- Service = HTTP
	- Destination port ranges = 80
	- Protocol = TCP
	- Actoin = Allow
	- Priority = 100
	- Name = Allow-HTTP
9. Press Save and move onto creating VM.

#### Create public SSH key
1. Before we create the VM we need a public SSH key.
2. We create one by using the `azure-client`.
3. Use the ssh-keygen command to create one with default settings.
4. Copy the key to your clipboard and paste in when creating the VM.

#### Creating VM
1. Go to Home and then Virtual Machines.
2. Click Create and use the following details.
	1. Basics:
		- Virtual machine name = `devops-vm`
		- Region = West US
		- Security Type = Standard
		- Image = Ubuntu Server 24.04 LTS - x64 Gen2
		- Size = Standard_B1s
		- Authentication type = SSH public key
		- Username = azureuser
		- SSH public key source = Use existing public key
		- SSH public Key = paste SSH Public Key value
		- Public inbound ports = Allow selected ports
		- Selected inbound ports = SSH (22)
	2. Disks:
		- OS Disk type = Standard HDD
	3. Networking:
		- NIC network security group = Advanced
		- Configure network security group = `devops-nsg`
	4. Advanced:
		- Enable user data
		- User data = 
```
#cloud-config
packages_update: true
packages:
  - nginx
    
runcmd:
  - systemctl enable nginx
  - systemctl start nginx
```

3. Press review and Create and wait for VM to be created before moving on.

#### Create Application Gateway
1. At the top search for application gateway and click on it.
2. Click Create.
	1. Basics:
		- Application Gateway name = `devops-agw`
		- Region = West US
		- Tier = Basic
		- Virtual network = devops-vm-vnet
		- Subnet = Create new one
			- Use the default settings provided for new subnet.
	2. Frontends:
		- Press Add New for Public IP and use name `devops-agw-ip`.
		- Click Next: Backends.
	3. Backends:
		- Click Add a Backend Pool.
		- Name = `devops-backendpool`
		- Select target VM = `devops-vm`.
		- Click Save and continue.
	4. Configuration:
		- Add a routing rule.
		- Name = `devops-routing-rule`
		- Priority = 100
		- Listener name = `devops-listener`
		- Click on Backend Targets.
		- Backend target = `devops-backendpool`
		- Backend settings click add new
			- Backend settings name = `devops-http-settings`
			- backend port 80
			- click add
			- click add and then Review and create.

#### Validating Configuration
1. Use the public IP of the application gateway and enter it in the browser
2. If configuration is correct, the Welcome to Nginx! web page will load.
3. Click check to complete the task. 
