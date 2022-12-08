az account list-locations 
az group create  --name storage-resource-group  --location northeurope
Check if Im registered
az provider list --output table
az provider register --namespace 'Microsoft.Storage'
