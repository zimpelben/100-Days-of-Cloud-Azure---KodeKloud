The Nautilus DevOps team needs to set up an Azure Application Gateway to manage traffic for a backend pool of virtual machines. The gateway will serve as a load balancer, distributing traffic across the VMs.

### Task:

1) **Azure Virtual Network and Subnet**:

- Create a Virtual Network (VNet) named `xfusion-vnet` in the **East US** region.
- Create a Subnet named `xfusion-subnet` within the VNet for the VMs.
- Create a Subnet named `xfusion-apgw-subnet` within the VNet for the Application Gateway.

2) **Azure Virtual Machines**:

- Create two VMs named `xfusion-vm1` and `xfusion-vm2` in the **East US** region.
- Install Nginx on both VMs.
- Configure `index.html` on VM1 to display "Welcome to KKE Labs:Version 1".
- Configure `index.html` on VM2 to display "Welcome to KKE Labs:Version 2".

3) **Azure Application Gateway**:

- Create an Application Gateway named `xfusion-apgw` in the **East US** region.
- Assign the `xfusion-apgw-subnet` to the Application Gateway.
- Create a frontend IP configuration named `xfusion-apgw-ip`.
- Add the VMs `xfusion-vm1` and `xfusion-vm2` to the backend pool.
- Configure a basic routing rule to distribute traffic between the VMs.

4) **Validation**:

- Verify that the Application Gateway distributes traffic to both VMs.
- Ensure that accessing the Application Gateway URL displays either "Welcome to KKE Labs:Version 1" or "Welcome to KKE Labs:Version 2" depending on the load balancing.

`Notes:`

- Create all resources in the `East US` region.
- Use the Azure Portal or Azure CLI for resource creation.
- Ensure proper routing and traffic distribution through the Application Gateway.

### Answer

**Azure Virtual Network and Subnet**:

1. Sign into the Azure portal with the login credentials provided.
2. At the top, in the search bar, type virtual network. 
3. Click on virtual networks.
4. Click + Create.
5. Ensure a resource group is selected, use the existing resource group.
6. Under Virtual network name enter `xfusion-vnet.
7. Ensure (US) EAST US is selected under Region. 
8. Click Next twice, until you are on Address space.
9. Ensure an IPv4 Address space is added, by default it should already have `10.0.0.0/16` pre-configured.
10. Rename the default subnet to `xfusion-subnet`.
11. Add a second subnet named `xfusion-apgw-subnet`.
12. Press next twice until on the Review + Create section. 
13. Review the setup and then press Create at the bottom. 
14. Wait until Deployment is complete and then click on Go to resource to view the VNet.

**Azure Virtual Machines**:

1. Create a SSH key with the `ssh-keygen` command in the `azure-client`.
2. List the resource groups using `az group list -o table`.
3. Copy the resource group name as its need to create the VM.
4. Create the VM using the following configuration;

```
az vm create \
--name xfusion-vm1 \
--resource-group kml_rg_main-****** \
--location eastus \
--image Ubuntu2204 \
--size Standard_B2s \
--data-disk-sizes-gb 30 \
--storage-sku Standard_LRS \
--admin-user azureuser \
--ssh-key-values .ssh/id_rsa.pub
```

5. Once the VM has been created, note down the public IP.
6. We also have to open the port 80 which the python app will be listening on. 

```
az vm open-port \
--name devops-vm \
--port 80 \
--resource-group kml_rg_main-******
```

9. SSH into the VM.
10. Install the Nginx server;

```
sudo apt update
sudo apt install nginx -y
echo "Welcome to KKE Labs:Version 1" | sudo tee /var/www/html/index.html
sudo systemctl restart nginx
```

11. Go to public IP of the VM in the browser and ensure the text has changed.
12. Repeat steps to create `xfusion-vm2` and changing text to "Welcome to KKE Labs: Versoin 2".


**Azure Application Gateway**:

1. At the top search for application gateway and click on it.
2. Click Create.
	1. Basics:
		- Application Gateway name = `xfusion-apgw
		- Region = East US
		- Tier = Basic
		- Virtual network = `xfusion-vnet`
		- Subnet = `xfusion-apgw-subnet`
	2. Frontends:
		- Press Add New for Public IP and use name `xfusion-apgw-ip.
		- Click Next: Backends.
	3. Backends:
		- Click Add a Backend Pool.
		- Name = `xfusion-backendpool`
		- Select target VM = `xfusion-vm1` and `xfusion-vm2`
		- Click Save and continue.
	4. Configuration:
		- Add a routing rule.
		- Name = `xfusion-routing-rule`
		- Priority = 100
		- Listener name = `xfusion-listener`
		- Click on Backend Targets.
		- Backend target = `devops-backendpool`
		- Backend settings click add new
			- Backend settings name = `xfusion-http-settings`
			- backend port 80
			- click add
			- click add and then Review and create.
- Ensure `cookie-based-affinity` is disabled.

**Validation**:

1. Go to the Application Gateway public IP in your browser.
2. Refresh the page multiple times and it should switch between version 1 and 2. 
