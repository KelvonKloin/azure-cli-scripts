# Deploying Resources with ARM

- Deploying Resources with ARM
*Create the RG*:
resourceGroup="ARMDemo"
location="EastUS2"
az group create -n $resourceGroup -l $location
templateUri="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/application-workloads/wordpress/docker-wordpress-mysql/azuredeploy.json"

- Deploying an Online ARM Template
az deployment group create --name TestDeployment --resource-group $resourceGroup --template-uri $templateUri --parameters 'newStorageAccountName=myevoarmdemo' 'mysqlPassword=!Kelv0n!' 'adminUsername=KelvonKloin' 'adminPasswordOrKey=!Kelv0nKl0in!' 'dnsNameForPublicIP=myevoip2910'

- Creating ARM Templates
*Exporting an ARM template from an existing RG*"
az group export -n MyResourceGroupName

- Incremental Deployments 
az deployment group create 
--name TestDeployment 
--resource-group $resourceGroup  *Target RG*
--template-file MyTemplate.json *Location of the ARM template (--template-uri)*
--parameters @MyParameters.json *Parameters file*
--parameters UserName=Kelvon *Additional parameters overrride*
--Mode Incremental / Complete *Deployment mode*