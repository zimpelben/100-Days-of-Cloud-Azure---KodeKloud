The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to Azure. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. As part of this migration, they are focusing on setting up and managing Azure SQL Databases, implementing backup processes, and ensuring data recovery. Below are the tasks they require you to perform. 

**Task 1**: Create An Azure SQL Database

1. Create a publicly accessible Azure SQL Database instance with the following details:
	- Database name: `nautilus-sqldb`
	- Server Name: `nautilus-server-17556`
	- Location: `West US`
	- Backup Storage Redundancy: Locally-redundant backup storage.
	- Hardware Configuration: Basic (For less demanding workloads).
	- Admin Username: `nautilus-admin`
	- Admin Password: Set an appropriate password.
	- Database Size: Set to 2 GiB.
	- Keep all other configurations as `default`.
2. Ensure the database is in the `Ready` state.

**Task 2**: Create a Storage Account

1. Create a storage account named `nautilusst1900`.
2. Configure a Blob container named `nautilus-container-8535` with this storage account.

**Task 3**: Backup the Azure SQL Database

1. Take a backup of the Azure SQL database instance `nautilus-sqldb` and store it in the Blob container:
	- Storage account: `nautiluss1900`
	- Blob container: `nautilus-container-8538`
	- Backup File Name: nautilus-db-backup
2. Ensure the backup is full exported to the blob container.

**Task 4**: Download the backup

1. Download the backup file from the Blob container to the `/opt` directory on the `azure-client` host.
2. Ensure the file is accessible and properly named based on its extension.

**Requirements for Completion**:
- Ensure the SQL database is in the `Ready` state.
- Config the backup is stored in the specified blob container.
- Verify the backup file is successfully downloaded to the `/opt` directory on the client host.

### Answer

**Task 1**: Creating An Azure SQL Database

1. Login to the Azure Portal with the credentials provided.
2. Search for azure sql in the top search bar and, click on Azure SQL Database.
3. Click + Create.
4. Assign the existing resource group and then the following configuration.
	- Database name: `nautilus-sqldb`
	- Server Name: 
		- Click create new and enter the name `nautilus-server-17556`.
		- Change the location to `West US`.
		- Authentication method = Use SQL authentication
		- Server admin login = `nautilus-admin`
		- Password = Set an appropriate password
		- click OK.
	- Workload environment = Development.
	- Hardware Configuration: click Configure database
		- Service tier = Basic (For less demanding workloads).
		- Data max size (GB) = 2 Gib
	- Backup Storage Redundancy: Locally-redundant backup storage.
	- Network Connectivity
		- Connectivity method = Public endpoint
		- Allow Azure services and resources to access this server = `Yes`
	- Keep all other configurations as `default`.
	- Click Review + Create to create the database.

**Task 2**: Create a Storage Account

1. Once the SQL database is created, search for storage account in the top search bar. 
2. Click on Storage account.
3. Click + create.
4. Use the following configuration for the storage account;
	- Storage account name = `nautiluss1900`
	- Regions = `West US`
5. Click Review + Create to create the account.
6. Once the account is created go to resource. 
7. Got Data storage and Containers on the left hand side.
8. Click + Add container
	- Container name = `nautilus-container-8538`
	- Click Create.

**Task 3**: Backup the Azure SQL Database

1. Go to the `azure-client` and run the following command to backup/export the database to the storage container;

```
az sql db export \
--resource-group kml_rg_main-********* \
--server nautilus-server-17556 \
--name nautilus-sqldb \
--admin-user nautilus-admin\
--admin-password <password that was set> \
--storage-uri <URL of storage container> \ # ensure to add the file name at the end otherwise, it will error out <nautilus-sqldb-backup.bacpac
--storage-key <Storage account access key> \
--storage-key-type StorageAccessKey \ 
```

2. Once it has been completed, check the `nautilus-container-8538`, the portal to see if the file exist in the container.

**Task 4**: Download the backup

1. Once the file is in the container, we can download it to the `azure-client` with the following command;

```
az storage blob download \
--account-name nautiluss1900 \
--container-name nautilus-container-8538 \
--name nautilus-db-backup.bacpac \
--file /opt/nautilus-db-backup.bacpac \
--auth-mode login
```

2. Verify the file is downloaded to `/opt` dirctory.
3. Click check to complete the task.
