# Automating with Service Principals
	
- Creating a Service Principal
*Creating an AD Application*
az ad app create
--display-name MyApp
--homepage http://MyApp
--identifier-uris http://MyApp

*Creating a Service Principal*
az ad sp create-for-rbac
--name $appId
--password $password

*Create a role assigment*
az role assigment create
--assignee $spAppId
--role Contributor
--scope "/subscriptions/$subId/resourceGroups/$resGroup"

*Login in with a service principal*
az login --service-principal
-u $spAppId
--password $password
--tenant $tenantID