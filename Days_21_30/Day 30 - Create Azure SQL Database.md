The Nautilus Devops team is strategizing the migration of a portion of their infrastructure to Azure. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. Recently, they started working on creating and configuring some database instances in Azure.

For this task, create one `publicly` accessible Azure SQL Database instance along with the following details:

1. The name of the Azure SQL Database must be `devops-sqldb`.
2. The server name must be `devops-server-5516` under `centralus`.
3. The compute + storage configuration should be **Basic** (for less demanding workloads).
4. The backup storage redundancy should be **Locally-redundant backup storage**.
5. Set the login admin username to `devops-admin` and the set an appropriate password.
6. Set the database size to **2 GiB**.
7. Keep the rest of the configuration as `default`. Finally, make sure the database is in the `Ready` state before submitting this task.

### Answer using Azure Portal

1. Search for Azure SQL Database in the top search bar.
2. On the overview page, click + Create.
3. Assign the existing resource group to the database.
4. For the database name use `devops-sqldb`.
5. For the Server, click on new and enter the server name as `devops-server-5516` and location choose `(US) Central US`.
6. For Authentication method choose `Use SQL authentication`.
7. For Server admin login use `devops-admin` and a password as you see fit.
8. Click OK and continue with the setup. 
9. For Compute + storage click on Configure database and choose **Basic** (for less demanding workloads) and check that the size is set to 2 GiB, then click OK.
10. For backup change to Locally-redundant backup storage.
11. For the rest of the settings keep them as `Default`.
12. Click Review + Create to deploy the database.
13. Once the database is deployed, ensure it is running before clicking check.
