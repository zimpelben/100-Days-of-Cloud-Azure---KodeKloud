You are tasked with modifying an ARM template for deploying a virtual network. The current template is located in the `/root/arm-templates` directory under the filename `vnet-deployment-template.json`. You need to make the following changes to the template:

1. Change the name and `displayName` tag of the virtual network to `arm-vnet-devops`.
2. Update the `addressPrefixes` to `192.168.0.0/16`.
3. Add one more tag named `Environment` with value `KKE-devops`.

After making these changes, you need to deploy the ARM template using the Azure CLI. 

Use the following command to find out the resource group to use:

`as group list --query '[].name --output table | grep 'kml'`

### Answer using the Azure CLI

1. Run the command above to get the resource group.
2. Use your editor of choice, and open the file `vnet-deployment-template.json` in the `/root/arm-templates` directory.
3. Make the following changes to the ARM template;
	- "name" = "arm-vnet-devops"
	- "displayName" = "arm-vnet-devops"
	- Add under "tags" - "Environment" = "KKE-devops"
	- "addressPrefixes" = "192.168.0.0/16"
	Make sure to add a , at the end of the displayName line as you are adding a second tag.
4. Save the file when done.  
5. Next run the following command to deploy the ARM template

```
az deployment group create \
--resource-group kml_rg_main-******** \
--template-file /root/arm-templates/vnet-deployment-template.json
```

6. Once the output is displayed, the vnet is created. 
7. We can double check the vnet with the command `az network vnet list`.
8. Click check to complete the task. 
