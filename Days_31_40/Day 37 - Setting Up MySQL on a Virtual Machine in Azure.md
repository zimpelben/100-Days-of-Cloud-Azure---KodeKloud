The Nautilus DevOps team is tasked with integrating a PHP application hosted on an Azure VM with a MySQL database hosted on another Azure VM. This will validate the application's ability to connect to the database in the cloud. 

1. Create the MySQL VM:

	- Create a VM named `datacenter-mysql-vm` using the MySQL Jetware image from the Azure Marketplace.
	- Configure the VM in the Central US region.
	- Use `Password` as the authentication type.
	- Set the username as `datacenter_admin` and the password as `Namin@123456`.
	- Allow inbound traffic on port 3306 to enable MySQL access.

2. Setup the MySQL Database:

	- SSH into the `datacenter-mysql-vm`.
	- Use the `sudo /jet/enter mysql` command to access the MySQL shell.
	- Create a database named `datacenter-db`.
	- Create a MySQL user named `datacenter_user` with password `password123`.
	- Grant all privileges on the `datacenter-db` database to this user.

3. PHP VM Setup:

	- A VM named `datacenter-php-vm` already exist in the East US region.
	- This VM is hosting a PHP application and contains a pre-existing `db_test.php` file in the `/var/www/html` directory.

4. Database Connection Configuration:

	- Retrieve the public IP address of the `datacenter-mysql-vm`.
	- Update the database connection settings in the `db_test.php` file to use the MySQL credentials and public IP address of the `datacenter-mysql-vm`.

5. Validation:

	- Access the `db_test.php` file from the `datacenter-php-vm` using its public IP address.
	- Ensure the file displays the message `Connected successfully`, confirming the connection between the PHP application and the MySQL database.

**Notes**:
- Ensure the MySQL database allows inbound traffic on port 3306.
- Verify that the PHP application on the `datacenter-php-vm` successfully connects to the MySQL database on the `datacenter-mysql-vm`.

### Answer 

1. Create MySQL VM
	1. Login into Azure Portal with the credential provided.
	2. Go to Virtual machines and create a new machine.
	3. For the name use `datacenter-mysql-vm`.
	4. Region, ensure is set to `Central US`.
	5. Availability options choose No Infrastructure redundancy required.
	6. Security type choose Standard.
	7. Image, click on See all images and then search for mysql in the marketplace.
	8. Choose the Jetware image `Percona Server for MySQL`.
	9. Size use `Standard_B1s`.
	10. For Authentication Type switch to Password and using the following details;
		- Username = `datacenter_admin`
		- Password = `Namin@123456`
	11. For Selected inbound ports make sure SSH is enabled.
	12. Click Next: Disks and select Standard HDD for OS disk type.
	13. Click Next: Networking and change the NIC network security group to `Advanced`.
	14. Click on Create new for configure network security group and click on  + Add an inbound rule.
	15. Use the following setup for the inbound rule:
		- Source = Any
		- Source port ranges = *
		- Destination = Any
		- Service = MySQL
		- Destination port ranges = 3306
		- Protocol = TCP
		- Actoin = Allow
		- Priority = 1010
		- Name = Allow-MySQL
	16. Click add and then OK to save.
	17. Next Click on Review + Create to create the VM.
	18. Make a note of the public IP of the `datacenter-mysql-vm.
	19. Return to the `Azure-client` and SSH into the VM:

```
ssh datacenter-admin@<public-ip>
```

2. Setup the MySQL Database
	1. Inside the `datacenter-mysql-vm` use the following commands for the following;

```
# Start mysql command prompt
sudo /jet/enter mysql 

# Create database
CREATE DATABASE xfusion_db; 

# Create user
CREATE USER 'xfusion_user'@'datacenter-php-vm-public-ip' IDENTIFIED BY 'password123';

# Assign privileges to user
GRANT ALL PRIVILEGES ON xfusion_db.* TO 'xfusion_user'@'datacenter-php-vm-public-ip';

# Exit MySQL prompt
exit

# Exit SSH session
exit
```

3.  PHP and Database Configuration
	1. SSH into the `datacenter-php-vm`.
	2. Edit the file `/var/www/html/db_test.php` - make sure to use sudo.
	3. Edit the details and save. 

```
$servername = "datacenter-mysql-vm-public-up"
$username = "datacenter_user"
$password = "password123"
$dbname = "datacenter_db"
```

4. Validation
	1. Run the following command to check the connection. The output should be Connection Successful;

```
php -f /var/www/html/db_test.php
```

5. Click check to complete the task.
