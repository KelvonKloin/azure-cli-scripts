# Module #2 - Managing Virtual Machines

- VMs commands subgroup:
az vm -h

- Show VMs available:
az vm image list *Most used images*
az vm image list --all *All images available*
az vm image list -f "flavor" *Specify an image*
az vm image list -s "SkuName" *Knowing the SkuName of the image*
av vm list-sizes --location "location" *Images available per zone or region*

- Create a resource group:
ResourceGroupName="CreateVMTest" *Variable containing the resource group name*
az group create --name $ResourceGroupName --location EastUS2 *Creating the resource group*

- Create a VM:
VMName="TestVM" *Variable containing the VM name*
AdminPassword="CISTestVM221116!" *Variable containing the VM admin password*
az vm create *Starting command*

- Parameters:
/ --resource-group $ResourceGroupName *Using the existing variable of the resource groupe*
/ --name $VMName 
/ --image Win2016Datacenter 
/ --admin-username CISUser 
/ --admin-password $AdminPassword
/ --size

- Check the resources in the RG:
az resource list -g $ResourceGroupName -o table

- Check the VM size of a VM:
az vm show -g $ResourceGroupName -n $VMName --query "hardwareProfile.vmSize"

- List the IP addresses in the RG:
az vm list-ip-addresses -n $VMName -g $ResourceGroupName -o table

# Customizing the VM: Turning the VM into a WebServer

- Open ports in the VM:
az vm open-port --port 80 --resource-group $ResourceGroupName --name $VMName

- Install the Custom Script Extension:
az vm extension set 
--publisher Microsoft.Compute 
--version 1.10 
--name CustomScriptExtension 
--vm-name $VMName 
--resource-group $ResourceGroupName 
--settings extensionSettings.json

- Checking the state of the VM:
az vm show -d --name "VMTest" --resource-group "CreateVMTest" --query "powerState" -o tsv

- Stopping a VM:
az vm deallocate --name "TestVM" --resource-group "CreateVMTest"

- Deleting a VM:
az vm delete --name "TestVM" --resource-group "CreateVMTest"

- Starting a VM:
az vm start --name "TestVM" --resource-group "CreateVMTest"
