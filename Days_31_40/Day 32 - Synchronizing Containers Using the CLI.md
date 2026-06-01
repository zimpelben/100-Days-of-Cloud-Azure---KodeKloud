As part of a data migration project, the team lead has tasked the team with migrating data from an existing Azure Blob container to a new Blob container. The existing container contains a substantial amount of data that must be accurately transferred to the new container. The team is responsible for creating the new Blob container and ensuring that all data from the existing container is copied or synced to the new container completely and accurately. It is imperative to perform thorough verification steps to confirm that all data has been successfully transferred to the new container without any loss or corruption.

As a member of the Nautilus DevOps Team, your task is to perform the following:

**Create a New Private Azure Blob Container:** Name the container `datacenter-dest-24300` under the storage account `datacenterst3878`.

**Data Migration:** Migrate the file `datacenter.txt` from the existing `datacenter-source-18866` container to the new `datacenter-dest-24300` container.

**Ensure Data Consistency:** Ensure that both containers have the file `datacenter.txt` and confirm the file content is identical in both containers.

**Use Azure CLI:** Use the Azure CLI to perform the creation and data migration tasks.

### Answer using Azure CLI

1. Create the storage container with the following command;

```
az storage container create \
--name datacenter-dest-24300 \
--account-name datacenterst3878
```

2. To verify container creation use the following command to list all containers;

```
az storage container list --account-name datacenterst3878 -o table
```

- `-o table` = puts the output in a easily readable table

3. Use the following command to copy the file between containers;

```
az storage blob copy start \
--account-name datacenterst3878 \
--destination-container datacenter-dest-24300 \
--destination-blob datacenter.txt \
--source-container datacenter-source-18866 \
--source-blob datacenter.txt
```

4. Once the transfer is complete we can check the details of the file in each container;

```
az storage blob show \
--account-name datacenterst3878 \
-container-name datacenter-source-18866 \
-name datacenter.txt \
-o table

az storage blob show \
--account-name datacenterst3878 \
-container-name datacenter-dest-24300 \
-name datacenter.txt \
-o table
```

5. Check for the content length for example. 
6. We can also check by downloading the file with the command below;

```
az storage blob download \
--account-name datacenterst3878 \
-container-name datacenter-dest-24300 \
-name datacenter.txt
```

7. Click check to complete the task.
