The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the Azure cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. To achieve this, they have segmented large tasks into smaller, more manageable units. This granular approach enables the team to execute the migration in gradual phases, ensuring smoother implementation and minimizing disruption to ongoing operations. By breaking down the migration into smaller tasks, the Nautilus DevOps team can systematically progress through each stage, allowing for better control, risk mitigation, and optimization of resources throughout the migration process. 

For this task, create a network security group (NSG) with the following requirements:

- Name of the NSG should be `datacenter-nsg`.
- Add an inbound security rule named `Allow-HTTP` for `HTTP` service on port 80, with the source CIDR range of `0.0.0.0/0`.
- Add another inbound security rule named `Allow-SSH` for `SSH` service on port `22`, with the source CIDR range of `0.0.0.0/0`.

### Answer using Azure Portal

1. Sign into the Azure portal with the login credentials provided.
2. At the top, in the search bar, type network security group. 
3. Click on `Network security groups` under services.
4. In the Network foundation overview page, click on the + Create. 
5. Assign the existing resource group and for NSG name use `datacenter-nsg`.
6. Click on Review + Create and create the NSG.
7. Once the NSG is created successfully, click on go to resource.
8. In the NSG overview page, click on Settings on the left hand side.
9. Click on `Inbound security rules` and then click on + Add.
10. For the new rule use the following settings;
	- Source = IP Addresses
	- Source IP addresses/CIDR ranges = 0.0.0.0/0
	- Source port range = *
	- Destination = Any
	- Service = HTTP
	- Destination port ranges = 80
	- Protocol = TCP
	- Action = Allow
	- Priority = 1000
	- Name = Allow-HTTP
11. Add the rule and ensure it is showing at the top of the table once created. 
12. Now repeat the process by adding another rule to allow for SSH;
	- Source = IP Addresses
	- Source IP addresses/CIDR ranges = 0.0.0.0/0
	- Source port range = *
	- Destination = Any
	- Service = HTTP
	- Destination port ranges = 22
	- Protocol = TCP
	- Action = Allow
	- Priority = 1010
	- Name = Allow-SSH
13. Once both rules are created click Check to submit the task.

### Answer using Azure CLI

1. Run the command az group list to get the resource group name.
2. Copy the name as this is needed in the next step. 
3. To create the NSG using the following command.

```
az network nsg create \
--resource-group kml_rg_main-******** \
--name datacenter-nsg \
```

4. Once the NSG has been created, use the following commands to create the inbound rules;

```
az network nsg rule create \
--name Allow-HTTP \
--nsg-name datacenter-nsg \
--priority 1000 \
--resource-group kml_rg_main-******** \
--access Allow \
--source-address-prefixes 0.0.0.0/0 \
--destination-address-prefixes "*" \
--destination-port-ranges 80
--protocol Tcp
```

```
az network nsg rule create \
--name Allow-SSH \
--nsg-name datacenter-nsg \
--priority 1010 \
--resource-group kml_rg_main-******** \
--access Allow \
--source-address-prefixes 0.0.0.0/0 \
--destination-address-prefixes "*" \
--destination-port-ranges 22
--protocol Tcp
```

4. Click on Check to complete the task. 
