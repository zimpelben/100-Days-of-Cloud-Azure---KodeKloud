The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the Azure cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. To achieve this, they have segmented large tasks into smaller, more manageable units. This granular approach enables the team to execute the migration in gradual phases, ensuring smoother implementation and minimizing disruption to ongoing operations. By breaking down the migration into smaller tasks, the Nautilus DevOps team can systematically progress through each stage, allowing for better control, risk mitigation, and optimization of resources throughout the migration process. 

For this task, create an SSH key pair with the following requirements:

- The name of the SSH key pair should be `datacenter-kp`.
- The key pair `type` must be `rsa`.

### Answer: Using Azure Portal 

1. Login into the Azure Portal with the login details provided.
2. Inside the Azure Portal, use the search bar at the top and type in `ssh key`.
3. Click Create New.
4. Select Resource group (kml_rg_main****** ).
5. Under Key pair name, enter the value - `datacenter-kp`.
6. SSH Key Type, ensure RSA is selected. 
7. Click on Review + create in the bottom left corner. 
8. Review details and select Create in the bottom left corner.
9. Download the private key and create resource.
10. Click refresh if the key does not appear on the SSH keys section.
11. You can double check if the key is created in the terminal using the command `az sshkey list`

### Answer: Using Azure CLI

1. List the existing resource group with the command `az group list`.
2. Make a note of the location and name of the resource group as both values are required to generate the new ssh key pair. 
3. Create a new SSH key pair using the command `az sshkey create --location "<resource-group-location" --resource-group "<resource-group-name" --name "datacenter-kp"
4. The key pair will be created and you can double check using the `az sshkey list` command.


Finish the task by clicking check. 
