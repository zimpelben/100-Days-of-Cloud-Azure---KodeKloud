The Nautilus DevOps team is presently immersed in data migrations, transferring data from on-premise storage systems to Azure Blob containers. They have recently received some data that they intend to copy to one of the Blob containers. 

A Blob container named `xfusion-blob-6581` already exists in the `eastus` region under the storage account `xfusionst10563`. Copy the file `/tmp/xfusion.txt` to the Blob container `xfusion-blob-6581`.

### Answer using Azure CLI

1. Use the following command to upload the file from the host to the blob container;

```
az storage blob upload -f /tmp/xfusion.txt \
--container-name xfusion-blob-6581 \ 
--account-name xfusionst10563
```

2. Once the upload is complete press check to complete the task. 
