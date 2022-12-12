# Module #1 - Introducing Azure CLI

- Checking version:
az --version

- Login
az login

- Show details of the selected subcription:
az account show

 - Show details of all the subscriptions:
az account list

- Select an specific subscription:
az account set -s "subscription name" 
az account set -s "subscription GUID"

- List of available commands:
az

- Help:
az "command" -h

- Interactive mode:
az interactive

- Format output (table):
az "command" -o table
az "command" -o tsv

- Format output (highlighted JSON):
az "command" -o jsonc

- Filter output with queries:
*Properties of a single resource:*
az "command" --query "property"
az "command" --query "[property1,property2]"
az "command" --query "{Property1:property1,Property2:property2}"

*Properties or 2+ resources:*
az "command" --query "[].property"
az "command" --query "[].{Property1:property1,Property2:property2}"
az "command" --query "[0].{Property1:property1,Property2:property2}"
