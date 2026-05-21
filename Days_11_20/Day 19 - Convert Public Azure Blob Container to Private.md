The Nautilus DevOps team has been using Azure Blob Storage to manage their data. Recently, they realized that one of their containers, currently public, needs to be restricted for internal use only. Your task is to convert a public Azure Blob container to private.

Two blob containers named `devops-container-12750` and `devops-priv-2027` are available in the `southcentralus` region within the storage account `devopsst25029`. The `devops-container-12750` is currently public, and `devops-priv-2027` is private. 

1. Convert the blob container `devops-container-12750` from public to private while leaving `devops-priv-2027`.
2. Make sure the access level for `devops-container-12750` is set to `private` with no public access. 

### Answer using Azure Portal

1. Sign into the Azure portal with the login credentials provided.
2. At the top, in the search bar, type storage. 
3. Click on `Storage accounts` under services.
4. In the Storage center overview page, click on the `devopsst25029'. 
5. In the storage account, click on Data storage and Containers in the left hand panel.
6. Two containers will be listed, click on the three dots on the right hand side for the container named `devops-container-12750`.
7. Under the options click on Change access level.
8. Change Anonymous access level to `Private (no anonymous access` and press OK. 
9. Wait for the table to update and the anonymous access level to state private.
10. Click Check to submit the task.

### Answer using Azure CLI

1. Run the follow command to change the access level of the blob container;

```
az storage container set-permission \
--name devops-container-12750 \
--account-name devopsst25029 \
--public-access off
```

3. You can check if all worked by using the following command;

```
az storage container show-permission \
--name devops-container-12750 \
--account-name devopsst25029 \
```

4. The output should show 

```
{
  "publicAccess": "off"
}
```

4. Click on Check to complete the task. 
