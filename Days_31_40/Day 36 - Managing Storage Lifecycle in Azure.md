The Nautilus DevOps team needs to optimize data retention costs by automating the deletion of old blobs. They plan to implement Blob Lifecycle Management for a specific container in Azure Storage. 

Task:

1. Create a Storage Account:
	- Name the storage account `devopsstor21117`.
	- Set the region to `East US`.
	- Use Locally-redundant storage (LRS) as the redundancy option.

2. Create Blob container:
	- Name the container `devops-container21117`.

3. Upload a File to the Container:
	- Upload the file named `tempfile.txt` to the container. The file is present under `/root` of the client host.

4. Configure Blob Lifecycle Management:
	- Apply a Lifecycle Management rule named `devops-del-rule` to the container `devops-container21117` to delete blobs after `7` days of last modification. 

5. Validation:
	- Verify that the Lifecycle Management rule named `devops-del-rule` is correctly applied. 

**Notes**:
- Create the resources only in the `East US` region.
- Use the Azure Portal or Azure CLI for resource creation.
- Ensure the storage account and container are properly configured.

### Answer using Azure CLI

1. Get the name of the existing resource group with the following command;

```
az group list -o table
```

2. Create the storage account with the following command;

```
az storage account create \
--name devopsstor21117 \
--resource-group kml_rg_main-******** \ # use resource group from command above
--location eastus \
--sku Standard_LRS
```

3. Create blob container next with the command;

```
az storage container create \
--name devops-container21117 \
--account-name devopsstor21117 \
--resource-group kml_rg_main-******** \ # use resource group from command above
```

4. Once the container is created, we upload the file `tempfile.txt` via the command below;

```
az storage blob upload \
-f /root/tempfile.txt \
-c devops-container21117 \
--account-name devopsstor21117 \
-n tempfile.txt
```

5. To create a lifecycle managment rule, we first need to specify the rule in a json file. The rule will be as follows;

```
# Use your editor of choice

{
  "rules": [
    {
      "name": "devops-del-rule",
      "enabled": true,
      "type": "Lifecycle",
      "definition": {
        "filters": {
          "blobTypes": [ "blockBlob" ],
          "prefixMatch": [ "devops-container21117/" ]
        },
        "actions": {
          "baseBlob": {
            "delete": { "daysAfterModificationGreaterThan": 7 }
          }
        }
      }
    }
  ]
}
```

- Ensure to use "prefixMatch" as the rule is only supposed to be applied to the specific container and not the entire Storage Account. 

6. Apply the rule with the following command;

```
az storage account management-policy create \
--account-name devopsstor21117 \
--resource-group kml_rg_main-******** \
--policy lifecycle-policy.json
```

7. We can verify the policy by viewing it with the following command;

```
az storage account management-policy show \
--account-name devopsstor21117 \
--resource-group kml_rg_main-******** 
```

8. Once the rule is verified we can click check to complete the task. 
