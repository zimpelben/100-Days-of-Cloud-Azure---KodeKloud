The Nautilus DevOps tea has been tasked with creating an internal information portal for public access. As part of this project, they need to host a static website on Azure using an Azure Storage account. The Storage account must be configured for public access to allow external users to access the static website directly via the Azure Storage URL.

Task Requirements:
1. Create an Azure Storage account named `nautiluswebst25752` in an existing resource group.
2. Configure the Storage account for a static website hosting with `index.html` as the index document.
3. Allow public access to the static website so that the website is publicly accessible. 
4. Upload the `index.html` file from the `/root/` directory of the Azure client host to the Storage account's `$web` container.
5. Verify that the website is accessible directly through the Azure storage static website URL. 

**Notes**
- Create the resources only in the `East US` region.
- Use the Azure Storage account's `$web` container to host the static website files.

### Answer using Azure Portal

1. Sign into the Azure portal with the login credentials provided.
2. At the top, in the search bar, type storage. 
3. Click on `Storage accounts` under services.
4. In the Storage center overview page, click on the + Create. 
5. Assign the existing resource group and for Storage account name use `nautiluswebst25752`.
6. For Preferred storage type choose `Azure Blob Storage or Azure Data Lake Storage`.
7. Under Networking ensure Public Network Access is Enabled.
8. Click on Review + Create and create the Storage Account.
9. Once the Storage account is created successfully, click on go to resource.
10. In the overview page go to Data Management in the left hand panel.
11. Select Static Website.
12. Enable and add under Index document name `index.html`.
13. Press save and will see a `$web` container has been create under the enabled option.
14. Make a note of the primary endpoint URL, we can test the URL but will give us a `The request content does not exist`.
15. On the overview page of the Storage account, take note of the resource group and return to the `azure-client` to upload the `index.html` file.
16. To upload the `index.html` file, use the following command;

```
az storage blob upload \
-f /root/index.html \
--container-name '$web' \
--account-name nautiluswebst25752
```

17. Once the file is uploaded, use the URL under Data Management - Static Website, to check the website.
18. If successful, the website will load and say `Welcome to KKE labs!`.
19. Click Check to complete the task.
