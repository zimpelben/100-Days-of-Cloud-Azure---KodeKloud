The Nautilus DevOps Team is working on setting up a new virtual machine (VM) to host a web server for a critical application. The team lead has requested you to create an Azure VM that will serve as a web server using Nginx. This VM will be part of the initial infrastructure setup for the Nautilus project. Ensuring that the server is correctly configured and accessible from the internet is crucial for the upcoming deployment phase. 

As a member of the Nautilus DevOps team, you task is to create a VM with the following specifications:

**Instance Name**: The VM must be named `devops-vm`.

**Image**: Use any available Ubuntu image to create this VM.

**Custom Script Extension/User Data**: Configure the VM to run a custom script during its launch. This script should:

- Install the Nginx package.
- Start the Nginx service. 

**Network Security Group (NSG)**: Ensure that the VM allows HTTP traffic on port `80` from the internet.

### Answer: Using Azure Portal 

1. Login in with the credential provided.
2. Go to Virtual Machines.
3. Click + Create.
4. Choose Virtual machine.
5. Select Resource group (kml_rg_main****** ).
6. Virtual machine name = `devops-vm`.
7. Availability options choose No infrastructure redundancy required
8. For Image choose `Ubuntu Server 24.04 LTS`.
9. For Size click on the drop down and then See all sizes.
10. Select Class B and then `Standard_B1s`.
11. For Authentication type select SSH public key.
12. Use the `azureuser` for username (this is automatically generated).
13. Ensure RS SSH Format is selected.
14. Use the auto-generated Key pair name.
15. For Public inbound ports select Allow selected ports
16. Ensure SSH (22) and HTTP (80) is selected for Select inbound ports.
17. Click on Next: Disk.
18. For OS disk size ensure `Image Default (30GiB)` is selected.
19. OS Disk type, select `Standard HDD`.
20. Click on Next:Networking.
21. Ensure the auto-generated network values are corrected and that SSH (22) and HTTP (80) port is allowed. 
22. Click through to Advanced.
23. In Advanced under User data, Enable user data and provide the following in the text box below;

```
#cloud-config
packages_update: true
packages:
  - nginx
    
runcmd:
  - systemctl enable nginx
  - systemctl start nginx
```

24. Click on Review and Create.
25. Download the private key and create resource.
26. Ensure the deployment is complete and then click go to resource.
27. Copy the `Public IP` and enter it in you brownser.
28. You will see Welcome to nginx!, which confirms the nginx install and launch was successful. 
29. Then check the connection via SSH. 
