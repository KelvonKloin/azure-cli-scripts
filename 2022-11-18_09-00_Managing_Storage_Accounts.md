# Module 3 - Managing Storage Accounts

- Creating an Storage Account:

*Declaring variables*
resourceGroup="CLIStorageTest"
location="EastUS2"
storageAccount="evoblobtest"

*Creating the RG*
az group create -n $resourceGroup -l $location

*Creating the Storage Account*:
az storage account create -n $storageAccount -g $resourceGroup -l $location --sku Standard_LRS

*Getting the connection string of the SA*:
az storage account show-connection-string -n $storageAccount -g $resourceGroup --query connectionString -o tsv

*Getting the connection string into the varible*:
connectionString="DefaultEndpointsProtocol=https;EndpointSuffix=core.windows.net;AccountName=evoblobtest;AccountKey=UjSgX8QkmHdMg5v5ZPgXVspURVHBCbZWoPq6D3mxSaaewpsVXVoHMNRLN1XRF+CDYpxO/Zj3WWgt+AStPKroog=="

- Working with Blob Storage:

*Creating a public storage container*:
az storage container create -n "evopublictest" --public-access blob --connection-string $connectionString

*Creating an enviroment variable*:
$env:AZURE_STORAGE_CONNECTION_STRING = $connectionString

*Creating a private storage container*:
az storage container create -n "evoprivatetest" --public-access off 

*Creating a test file*:
echo "Hello World" > example.txt

*Uploading a file into the public container*:
az storage blob upload -c "evopublictest" -f "example.txt" -n "test.txt"

*Getting the URL of the public blob*:
az storage blob url -c "evopublictest" -n "test.txt" -o tsv

*Uploading a file into the private container*:
az storage blob upload -c "evoprivatetest" -f "example.txt" -n "test.txt"

*Getting the URL of the private blob*:
az storage blob url -c "evoprivatetest" -n "test.txt" -o tsv

*Creating the SAS Token for the private blob*:
az storage blob generate-sas -c "evoprivatetest" -n "test.txt" --permissions r -o tsv --expiry 2022-11-21T11:50Z

- Working with Queues:

*Creating the queue*:
queueName="evoqueue"
az storage queue create -n $queueName

*Submitting messages into the queue*:
az storage message put --content "Hello from PowerShell" -q $queueName
az storage message put --content "This is just a test" -q $queueName

*Read messages from the queue*:
az storage message get -q $queueName --visibility-timeout 120

*Update the time of the message*:
az storage message update

*Delete a message from the queue*:
az storage message delete --id "XXX" --pop-receipt "XXX" -q $queueName

- Working with Tables:
*Creating the table*:
tableName="evotabletest"
az storage table create -n $tableName

*Insert into the table*:
az storage entity insert -t $tableName -e PartitionKey="Settings" RowKey="Timeout" Value=10 Description="Timeout in seconds"

*Call back the rows of the table*:
az storage entity query -t $tableName
az storage entity query -t $tableName --filter "PartitionKey eq 'Settings'"
az storage entity query -t $tableName --filter 'Value eq 10'

*Update the row*:
az storage entity replace -t $tableName -e PartitionKey="Settings" RowKey="MaxRetries" Value=5 Description="Maximum Retries"
az storage entity merge -t $tableName -e PartitionKey="Settings" RowKey="MaxRetries" Value=6

*Show an specific value*:
az storage entity show -t $tableName --partition-key "Settings" --row-key "MaxRetries"

- Working with Files Shares:
*Creating the file share*:
fileShare="evofilesharetest"
az storage share create -n $fileShare --quota 2

*Upload a file to file share*:
az storage file upload -s $fileShare --source "example.txt"

*List all the files in the file share*:
az storage file list -s $fileShare

- Delete the Resource Group:
az group delete -n $resourceGroup --yes