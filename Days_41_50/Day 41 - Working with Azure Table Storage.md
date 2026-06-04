The Nautilus DevOps team is developing a simple 'To-Do' application using Azure Table Storage to store and manage tasks efficiently. The team needs to create an Azure Table to hold tasks, each identified by a unique `taskId`. Each task will have a description and a status, which indicates the progress of the task (e.g., 'completed' or 'in-progress').

Your task is to:

1. Create an Azure Storage Account named `devopstablest18557` with a Table Storage table called `tasks`.
2. Insert the following tasks into the table:
	- Task 1: PartitionKey: 'tasks', RowKey: '1', description: 'Learn Table Storage', status: 'completed'.
	- Task 2: PartitionKey: 'tasks', RowKey: '2', description: 'Build To-Do App', status: 'in-progress'.
3. Verify that Task 1 ha a status of 'completed' and Task 2 has a status of 'in-progress'.

**Note**: 
- Use the Azure CLI to insert these tasks into the table.
- Create the resources only in the `eastus` region.

### Answer using Azure CLI

#### 1. Create storage account
1. Run `az group list -o table` to get the resource group.
2. Create a new storage account with the following command.

```
az storage account create \
--name devopstablest18557 \
--resource-group kml_rg_main-********* \
--location eastus \
--sku Standard_LRS
```

3. Next we create the table as follows;

```
az storage table create \
--name tasks \
--account-name devopstablest18557
```

4. We need to get the account key to be able to insert data into the table;

```
az storage account keys list \
--account-name devopstablest18557 \
--query '[0].value' \
-o tsv
```

4. Copy the access key value from the output above.
5. Next we have to insert the following tasks as such;

```
az storage entity insert \
--account-name devopstablest18557 \
--account-key <Access-Key> \
--table-name tasks \
--entity PartitionKey=tasks RowKey=1 Description="Learn Table Storage" Status=completed

az storage entity insert \
--account-name devopstablest18557 \
--account-key <Access-Key> \
--table-name tasks \
--entity PartitionKey=tasks RowKey=2 Description="Build To-Do App" Status=in-progress
```

5. To verify that tasks have the correct status run;

```
az storage entity query \
--account-name devopstablest18557 \
--account-key <Access-Key> \
--table-name tasks \
-o table
```

- `-o table` - formats the output in a easy to read table.

6. If the statuses are correct. Click check to complete the task.
