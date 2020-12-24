# Udacity-Bertelsmann-Scholarship-WebApp-with-Azure-storage
This repository contains steps to deploy a simple web application, running the SQL database and Azure Blob storage on Azure Cloud Platform. Follow the following steps to run this app on the machine, with database and  on the Azure Cloud Platform. This app can be run on local and virtual machine. However, steps to run this app on a Azure virtual machine and creating that machine are not covered in this repository.

## Requirements to run the scripts on your local machine, to create resources on Azure Cloud Platform.
Here are some of the requirements, to run these scripts on your local machine:
1. Download a text editor like Microsoft Visual Studio Code. Link to download Microsoft Visual Studio Code: https://code.visualstudio.com/
2. Set up a Microsoft Azure account. Here's link to create your Azure account: https://azure.microsoft.com/en-gb/
3. Set Azure Command Line Interface on your device. Download Azure CLI tool using the link: https://docs.microsoft.com/en-us/cli/azure/
4. Use the following command in your command prompt or terminal of your VS Code to log-in to your Azure account.
`
$ az login
`

## Steps to create a Azure Resource group
Run this script on your command prompt or terminal of your VS Code to create a resource group:
`
$ az group create -l westus -n resource-group-west
`

## Steps to create the SQL Server with Database on Azure cloud platform
Here are steps to create the SQL Server and Database, on Microsoft Azure:
1. Run this script on your command prompt or terminal of your VS Code to create the SQL server:
`
$ az sql server create 
--admin-user uadmin 
--admin-password p@ssword1234 
--name hello-world-server 
--resource-group resource-group-west 
--location westus2 
--enable-public-network true 
--verbose
`

2. Run this scripts on your command prompt or terminal of your VS Code to create outbound firewall rule for your SQL server:
`
$ az sql server firewall-rule create 
-g resource-group-west 
-s hello-world-server 
-n azureaccess 
--start-ip-address 0.0.0.0 
--end-ip-address 0.0.0.0 
--verbose
`

3. Now, determine the IP of your computer, and copy that to add that in step 4. You can determine your IP by running the following command:
For windows command prompt:
`
$ ipconfig /all 
`
For mac:
`
$ curl ifconfig.me
`

4. Now, add your IP address into the following command, to create a firewall rule to connect to your SQL server:
`
$ az sql server firewall-rule create 
-g resource-group-west 
-s hello-world-server 
-n azureaccess 
--start-ip-address [ADD YOUR IP ADDRESS] 
--end-ip-address [ADD YOUR IP ADDRESS] 
--verbose
`

5. Now, execute following command to create the Database in your SQL server:
`
$ az sql db create 
--name hello-world-db 
--resource-group resource-group-west 
--server hello-world-server 
--tier Basic 
--verbose
`

## Steps to create the Azure Blob storage on Azure cloud platform
Here are steps to create he Azure Blob storage on Microsoft Azure. This will store all the images of the web app.
1. Execute following command to create a storage account for your blob storage:
`
$ az storage account create 
--name helloworld12345 
--resource-group resource-group-west 
--location westus2
`

2. Now, we need to add a Blob Container. For this, you will need to access your Azure Account from online portal. And search about Azure Storage Accounts. And follow the steps accordingly:
    a. Once the Storage account is deployed, click on its name to access it (you may need to go back to the main "Storage accounts" page in Azure).
    b. Click on the "Containers" button in the storage account's "Overview" page.
    c. Click "+ Container", then add the name of images.
    d. Set "Public access level" to Container, and click "Create". 
   
## Steps to run the Web Application 
For running the application, you will need to copy following information of your resources, from your Azure Portal. Then update this information into some of the files, and then run the application.
1. To connect an app to the storage we've set up, we need a few things from each storage service. From the SQL server and database:
SQL Server server name (the name of the sql server with .database.windows.net appended to it), Admin username, Admin password ,SQL Database name. From blob storage: Storage account name, A storage account access key (From the Azure portal, navigate to the storage account, and then under "Setting", click on "Access keys". Copy and paste the "Key" under "key1", including the double equal signs ("==") at the end), Container name.
2. Now, within [config.py](https://github.com/Karansingh005/Udacity-Bertelsmann-Scholarship-WebApp-with-Azure-storage/blob/main/config.py), update all the values, that we noted down in previous steps.
3. Now, setup the terminal, go to the directory of the application, and run the [application.py](https://github.com/Karansingh005/Udacity-Bertelsmann-Scholarship-WebApp-with-Azure-storage/blob/main/application.py) file
