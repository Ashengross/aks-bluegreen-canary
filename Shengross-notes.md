az login --use-device-code
# Create a service principal for a resource group using a preferred name and role
az ad sp create-for-rbac --name myAKSClusterCanary --role contributor --scopes /subscriptions/7d2529f2-f8b2-4eed-beea-24db93eda831/resourceGroups/myResourceGroup --sdk-auth
{
  "clientId": "4f1fb63a-f7c9-4752-9926-29cc05ba21d0",
  "clientSecret": "SrSs0.7DxLXXLb3o0C_nMfiG-RPbfkP_1D",
  "subscriptionId": "7d2529f2-f8b2-4eed-beea-24db93eda831",
  "tenantId": "72f988bf-86f1-41af-91ab-2d7cd011db47",
  "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
  "resourceManagerEndpointUrl": "https://management.azure.com/",
  "activeDirectoryGraphResourceId": "https://graph.windows.net/",
  "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
  "galleryEndpointUrl": "https://gallery.azure.com/",
  "managementEndpointUrl": "https://management.core.windows.net/"
}

az aks create --resource-group myResourceGroup --name myAKSClusterCanary --node-count 1 --enable-addons monitoring --generate-ssh-keys
az aks get-credentials --resource-group myResourceGroup --name myAKSClusterCanary
az aks start --name myAKSClusterCanary --resource-group myResourceGroup
az aks stop --name myAKSClusterCanary --resource-group myResourceGroup

cd nginx-html
docker build -t shengrossdemoacr.azurecr.io/blue-nginx:1 .
docker build -t shengrossdemoacr.azurecr.io/green-nginx:1 .

docker push shengrossdemoacr.azurecr.io/blue-nginx:1
docker push shengrossdemoacr.azurecr.io/green-nginx:1



