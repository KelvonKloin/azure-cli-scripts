# Managing Web Apps and SQL Databases

- Creating a WebApp:
*Creating an App Service Plan*:
resourceGroup="EvoWebAppTest" *Creating the RG*
location="EastUs2"
az groupe create -n $resourceGroup -l $location
planName="EvoWebAppDemo" 
az appservice plan create -n $planName -g $resourceGroup --sku B1

*Creating the Web App*:
appName="evocisdemo"
az webapp create -n $appName -g $resourceGroup --plan $planName
az webapp show -n $appName -g $resourceGroup --query "defaultHostName" (*Getting the url*)

- Deploying Git from a repository:
*Deploying code from GitHub*:
gitrepo="https://github.com/markheath/azure-cli-snippets"
az webapp deployment source config -n $appName -g $resourceGroup --repo-url $gitrepo --branch master --manual-integration

*Sync changes from the repository*:
az webapp deployment source sync -n $appName -g $resourceGroup

- Creating a SQL DB:
*Creating an SQL Server*:
sqlServerName="evosqldemo"
sqlServerUsername="KelvonKloin"
sqlServerPassword="*Kelv0n!"
az sql server create -n $sqlServerName -g $resourceGroup -l $location -u $sqlServerUsername -p $sqlServerPassword

*Creating the database*:
databaseName="EvoCISDatabase"
az sql db list-editions -l $location -o table
az sql db create -g $resourceGroup -s $sqlServerName -n $databaseName --service-objective Basic

- Connecting a Web App to a SQL Dataase:
*Creating the firewall rule*:
az webapp show -n $appName -g $resourceGroup --query "outboundIpAddresses" -o tsv
az sql server firewall-rule create -g $resourceGroup -s $sqlServerName -n AllowWebApp1 --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0

*Creating the connection string*:
connectionString="Server=tcp:$sqlServerName.database.windows.net;Database=$databaseName;User ID=$sqlServerUsername@$sqlServerName;Password=$sqlServerPassword;Trusted_Connection=False;Encrypt=True;"

*Setting a connection string for the web app*:
az webapp config connection-string set -n $appName -g $resourceGroup --settings "SnippetsContext=$connectionString" --connection-string-type SQLAzure

- Backing up & Restoring a DB:
az sql db export -s MyServerName -d MyDBName -g MyRG -u Username -p Password -storage-key-type StorageAccessKey --storage-key MyStorageKey --storage-uri ByBacpacBlobUri