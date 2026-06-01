The Nautilus DevOps team is tasked with deploying a Python-based web application on Azure. You need to create a web app using the following specifications:

1) The **Web App** name should be `xfusion-webapp`.  
2) It should be created in the **east US** region under the **default resource group**.  
3) The **publish** option should be set to `Code`.  
4) The **Runtime Stack** should be `Python` with `Linux` as the operating system.  
5) Create a new **App Service Plan** named `xfusion-learn-python` with the SKU `Basic B1`. 
6) **Application Insights** should be disabled.  
6) Add **tags**:
	- Name: WebAppLearning
	- Environment: Dev

Make sure the web app is in `Running` state after creation.

### Answer using Azure Portal

1. Search for web app in the top search bar.
2. Click on App Services.
3. On the overview page click Create and choose Web App.
4. Assign the existing Resource Group.
5. For the name use `xfusion-webapp`.
6. Ensure the publish option is set to `code`.
7. Choose the latest Python version for the Runtime Stack.
8. Ensure operating system is set to `Linux`.
9. Region choose `Central US`.
	- The task did not specify a region, but `East US` or the default `Central Canada` did not work.
10. For Linux Plan, select `Create New` and name the plan `xfusion-learn-python`.
11. For Pricing Plan, select the `Basic B1` SKU.
12. Leave all other options as default but under Monitor + Secure, ensure the Application Insight is switched to `No`.
13. Under Tags, create the tags;
	- Name: WebAppLearning
	- Environment: Dev
14. Click Review + Create and once the checks have completed, click Create.
15. Once the app is created, check the overview page to ensure its in a `Running` state.
16. Click check to complete the task. 
