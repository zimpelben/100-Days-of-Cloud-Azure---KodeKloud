The Nautilus DevOps team is currently engaged in a cleanup process, focusing on removing unnecessary data and services from their Azure environment. As part of the migration process, several resources were created for one-time use only, necessitating a cleanup effort to optimize their Azure environment.

A private blob container named `datacenter-blob-7033` already exists in the `westus` region under storage account `datacenterst14016`.

1. Copy the content of the `datacenter-blob-7033` blob container to the `/opt` directory on the `azure-client` host (the landing host once you load this lab).
2. Delete the blob container `datacenter-blob-7033` from the storage account.

### Answer using Azure CLI

1. We need to get the account key of the storage account which we can find with the command below;

```
az storage account keys list \
--account-name datacenterst14016 \
--query '[0].value' \
-o tsv
```

2. We will list all of the blobs stored in the container;

```
az storage blob list \
--account-name datacenterst14016 \
--account-key <Access-Key> \
--container-name datacenter-blob-7033 \
-o table
```

3. In this instance there is only one blob stored in the container.
4. We have two option, just to download the file or we can use batch-download, which would download all files;

```
# Download specific file
az storage blob download \
-f /opt/datacenter.txt \ # patch to where file should be stored
--account-name datacenterst14016 \
--account-key <Access-Key> \
--container-name datacenter-blob-7033 \
--name datacenter.txt # name of blob as shown in blob list command.

# Download all files from container
az storage blob download-batch \
--account-name datacenterst14016 \
--source datacenter-blob-7033 \
--destination /opt/ \
--account-key <Access-Key> 
```

5. Now the contents has been downloaded, we can delete the container;

```
az storage container delete \
--name datacenter-blob-7033 \
--account-key <Access-Key> \
--account-name datacenterst14016
```

6. Verify container is deleted with;

```
az storage container list \
--account-key <Access-Key> \
--account-name datacenterst14016 \
-o table
```

7. If the container is delete click Check to complete task. 
