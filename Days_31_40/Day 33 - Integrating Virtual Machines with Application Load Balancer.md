The Nautilus DevOps team is currently working on setting up a simple application on the Azure cloud. They aim to establish an Azure Load Balancer in front of a Virtual Machine (VM) where an Nginx server is currently running. While the Nginx server currently serves a sample page, the team plans to deploy the actual application later.

1. Set up an Azure Load Balancer named `devops-lb`.
2. Configure the Load Balancer’s frontend IP configuration with the name `devops-lb-ip` and assign a public IP address with the same name (`devops-lb-ip`).
3. Create a backend pool named `devops-backend-pool` and add the VM running Nginx to this pool.
4. Create a health probe named `devops-health-probe` on port `80` to check the VM's health.
5. Set up a load balancer rule named `devops-lb-rule` to route traffic on port `80` to the backend pool on port `80`.
6. Add an inbound rule to the existing NSG of the VM to allow HTTP traffic on port 80.

`Notes:`

- Create the resources only in the `eastus` region.

### Answer using Azure Portal

1. Search for load balancer in the top search bar.
2. Click on Load Balancers.
3. On the overview page click Create and choose Load Balancer.
4. Assign the default resource group.
5. For name use `devops-lb`.
6. Region choose `East US`.
7. For SKU select Standard.
8. For Type switch to Public.
9. Tier leave as Regional.
10. Click Front IP configuration.
11. Add a frontend IP configuration.
12. For name use `devops-lb-ip`.
13. For Public IP address, click create new and use the same name `devops-lb-ip` and press save.
14. Click on Backend Pools and add a backend pool.
15. Use the name `devops-backend-pool` and then chose the existing Virtual Network.
16. Under IP configurations click add and choose the `devops-vm` and click add. 
17. Press Save and continue to Inbound rule.
18. Add a load balancing rule and name it `devops-lb-rule`.
19. Choose `devops-lb-ip` for Frontend IP address and `devops-backend-pool` for Backend pool.
20. For Port and Backend Port, set both to 80.
21. For health probe click create new and name the probe `devops-health-probe.
22. Ensure the port is set to 80 and press save.
23. Click on Review and Create and create the load balancer.
24. Once the load balancer is created, take note of the public IP.
25. If visit the IP it will not work as we still need to create the HTTP-allow rule on the existing NSG.
26. Go to the NSG and add an inbound rule with the follow settings.
	- Source = Any
	- Source port ranges = *
	- Destination = Any
	- Service = HTTP
	- Destination port ranges = 80
	- Protocol = TCP
	- Actoin = Allow
	- Priority = 1010
	- Name = Allow-HTTP
27. Once the rule is created, we can check the public IP and will see the Welcome to nginx! page.
28. Click check to complete the task.
