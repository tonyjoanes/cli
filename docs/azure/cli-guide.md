# Azure CLI (az) Guide

A comprehensive guide to working with the Azure Command Line Interface for managing Azure resources and services.

## Table of Contents
- [Getting Started](#getting-started)
- [Authentication and Account Management](#authentication-and-account-management)
- [Resource Groups](#resource-groups)
- [Virtual Machines](#virtual-machines)
- [App Service and Web Apps](#app-service-and-web-apps)
- [Storage Accounts](#storage-accounts)
- [Databases](#databases)
- [Networking](#networking)
- [Container Services](#container-services)
- [Azure Functions](#azure-functions)
- [Key Vault](#key-vault)
- [Monitoring and Diagnostics](#monitoring-and-diagnostics)
- [Common Workflows](#common-workflows)
- [Tips and Best Practices](#tips-and-best-practices)

---

## Getting Started

### Check Installation and Version
```bash
# Check Azure CLI version
az --version
az version

# Update Azure CLI
az upgrade

# Get help
az --help
az <command> --help
az vm --help
az webapp --help

# Interactive mode (easier autocomplete)
az interactive
```

### Installation
```bash
# Windows (via MSI installer or winget)
winget install -e --id Microsoft.AzureCLI

# macOS (via Homebrew)
brew update && brew install azure-cli

# Linux (Ubuntu/Debian)
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash

# Via Docker
docker run -it mcr.microsoft.com/azure-cli
```

### Configure CLI
```bash
# Configure defaults
az configure

# Set default location
az configure --defaults location=eastus

# Set default resource group
az configure --defaults group=myResourceGroup

# View current configuration
az configure --list-defaults

# Clear defaults
az configure --defaults location='' group=''
```

---

## Authentication and Account Management

### Login and Authentication

```bash
# Login interactively (opens browser)
az login

# Login with username and password
az login -u user@domain.com -p password

# Login with service principal
az login --service-principal -u <app-id> -p <password-or-cert> --tenant <tenant-id>

# Login with managed identity
az login --identity

# Login to specific tenant
az login --tenant <tenant-id>

# Check if logged in
az account show
```

### Account Management

```bash
# List all subscriptions
az account list
az account list --output table

# Show current subscription
az account show

# Set active subscription
az account set --subscription "Subscription Name"
az account set --subscription <subscription-id>

# List available locations
az account list-locations
az account list-locations --output table

# Get subscription details
az account show --output json
```

### Logout

```bash
# Logout from all accounts
az logout

# Clear token cache
az account clear
```

---

## Resource Groups

### Creating and Managing Resource Groups

```bash
# List all resource groups
az group list
az group list --output table

# Create a resource group
az group create --name myResourceGroup --location eastus

# Show resource group details
az group show --name myResourceGroup

# List resources in a resource group
az resource list --resource-group myResourceGroup
az resource list --resource-group myResourceGroup --output table

# Update resource group (add tags)
az group update --name myResourceGroup --tags Environment=Dev Project=MyApp

# Delete a resource group
az group delete --name myResourceGroup --yes --no-wait

# Check if resource group exists
az group exists --name myResourceGroup

# Export resource group template
az group export --name myResourceGroup > template.json
```

### Locking Resources

```bash
# Create a lock
az lock create --name DeleteLock --resource-group myResourceGroup --lock-type CanNotDelete

# List locks
az lock list --resource-group myResourceGroup

# Delete a lock
az lock delete --name DeleteLock --resource-group myResourceGroup
```

---

## Virtual Machines

### Creating and Managing VMs

```bash
# List all VMs
az vm list
az vm list --output table

# List VMs in a resource group
az vm list --resource-group myResourceGroup

# Create a VM (Ubuntu)
az vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --image Ubuntu2204 \
  --size Standard_B2s \
  --admin-username azureuser \
  --generate-ssh-keys

# Create a VM (Windows)
az vm create \
  --resource-group myResourceGroup \
  --name myWindowsVM \
  --image Win2022Datacenter \
  --admin-username azureuser \
  --admin-password MyP@ssw0rd123

# Create VM with public IP
az vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --image Ubuntu2204 \
  --public-ip-address myPublicIP \
  --public-ip-sku Standard

# List available VM images
az vm image list --output table
az vm image list --publisher Canonical --output table
az vm image list --offer Ubuntu --all --output table

# List VM sizes
az vm list-sizes --location eastus --output table
```

### VM Operations

```bash
# Start a VM
az vm start --resource-group myResourceGroup --name myVM

# Stop a VM (still billed)
az vm stop --resource-group myResourceGroup --name myVM

# Deallocate a VM (not billed for compute)
az vm deallocate --resource-group myResourceGroup --name myVM

# Restart a VM
az vm restart --resource-group myResourceGroup --name myVM

# Delete a VM
az vm delete --resource-group myResourceGroup --name myVM --yes

# Show VM details
az vm show --resource-group myResourceGroup --name myVM

# Get VM IP address
az vm show --resource-group myResourceGroup --name myVM --show-details --query publicIps -o tsv
az vm list-ip-addresses --resource-group myResourceGroup --name myVM
```

### VM Management

```bash
# Resize a VM
az vm resize --resource-group myResourceGroup --name myVM --size Standard_D2s_v3

# Open a port
az vm open-port --resource-group myResourceGroup --name myVM --port 80
az vm open-port --resource-group myResourceGroup --name myVM --port 443

# Run command on VM
az vm run-command invoke \
  --resource-group myResourceGroup \
  --name myVM \
  --command-id RunShellScript \
  --scripts "sudo apt-get update && sudo apt-get upgrade -y"

# Get VM extensions
az vm extension list --resource-group myResourceGroup --vm-name myVM

# Install VM extension
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name CustomScript \
  --publisher Microsoft.Azure.Extensions
```

---

## App Service and Web Apps

### Creating and Managing Web Apps

```bash
# List App Service plans
az appservice plan list --output table

# Create App Service plan
az appservice plan create \
  --name myAppServicePlan \
  --resource-group myResourceGroup \
  --location eastus \
  --sku B1 \
  --is-linux

# Create a web app
az webapp create \
  --resource-group myResourceGroup \
  --plan myAppServicePlan \
  --name myUniqueWebApp \
  --runtime "NODE:18-lts"

# List available runtimes
az webapp list-runtimes --os linux
az webapp list-runtimes --os windows

# Create web app with container
az webapp create \
  --resource-group myResourceGroup \
  --plan myAppServicePlan \
  --name myContainerApp \
  --deployment-container-image nginx:latest
```

### Web App Operations

```bash
# List web apps
az webapp list --output table
az webapp list --resource-group myResourceGroup

# Show web app details
az webapp show --name myUniqueWebApp --resource-group myResourceGroup

# Start web app
az webapp start --name myUniqueWebApp --resource-group myResourceGroup

# Stop web app
az webapp stop --name myUniqueWebApp --resource-group myResourceGroup

# Restart web app
az webapp restart --name myUniqueWebApp --resource-group myResourceGroup

# Delete web app
az webapp delete --name myUniqueWebApp --resource-group myResourceGroup

# Browse to web app
az webapp browse --name myUniqueWebApp --resource-group myResourceGroup
```

### Deployment

```bash
# Deploy from local Git
az webapp deployment source config-local-git \
  --name myUniqueWebApp \
  --resource-group myResourceGroup

# Deploy from GitHub
az webapp deployment source config \
  --name myUniqueWebApp \
  --resource-group myResourceGroup \
  --repo-url https://github.com/user/repo \
  --branch main \
  --manual-integration

# Deploy ZIP file
az webapp deploy \
  --resource-group myResourceGroup \
  --name myUniqueWebApp \
  --src-path app.zip \
  --type zip

# List deployment sources
az webapp deployment source show \
  --name myUniqueWebApp \
  --resource-group myResourceGroup

# Set up continuous deployment
az webapp deployment source config \
  --name myUniqueWebApp \
  --resource-group myResourceGroup \
  --repo-url <repo-url> \
  --branch main
```

### Configuration and Settings

```bash
# Set application settings
az webapp config appsettings set \
  --resource-group myResourceGroup \
  --name myUniqueWebApp \
  --settings KEY1=VALUE1 KEY2=VALUE2

# List application settings
az webapp config appsettings list \
  --resource-group myResourceGroup \
  --name myUniqueWebApp

# Delete application setting
az webapp config appsettings delete \
  --resource-group myResourceGroup \
  --name myUniqueWebApp \
  --setting-names KEY1

# Set connection string
az webapp config connection-string set \
  --resource-group myResourceGroup \
  --name myUniqueWebApp \
  --connection-string-type SQLAzure \
  --settings DefaultConnection='Server=...'

# Enable HTTPS only
az webapp update \
  --resource-group myResourceGroup \
  --name myUniqueWebApp \
  --https-only true

# Configure custom domain
az webapp config hostname add \
  --resource-group myResourceGroup \
  --webapp-name myUniqueWebApp \
  --hostname www.example.com
```

### Logs and Monitoring

```bash
# Enable logging
az webapp log config \
  --resource-group myResourceGroup \
  --name myUniqueWebApp \
  --application-logging filesystem \
  --web-server-logging filesystem

# Stream logs
az webapp log tail \
  --resource-group myResourceGroup \
  --name myUniqueWebApp

# Download logs
az webapp log download \
  --resource-group myResourceGroup \
  --name myUniqueWebApp \
  --log-file logs.zip
```

---

## Storage Accounts

### Creating and Managing Storage

```bash
# List storage accounts
az storage account list --output table

# Create storage account
az storage account create \
  --name mystorageaccount \
  --resource-group myResourceGroup \
  --location eastus \
  --sku Standard_LRS \
  --kind StorageV2

# Show storage account details
az storage account show \
  --name mystorageaccount \
  --resource-group myResourceGroup

# Get connection string
az storage account show-connection-string \
  --name mystorageaccount \
  --resource-group myResourceGroup

# Get access keys
az storage account keys list \
  --account-name mystorageaccount \
  --resource-group myResourceGroup

# Delete storage account
az storage account delete \
  --name mystorageaccount \
  --resource-group myResourceGroup
```

### Blob Storage

```bash
# Create a container
az storage container create \
  --name mycontainer \
  --account-name mystorageaccount

# List containers
az storage container list \
  --account-name mystorageaccount \
  --output table

# Upload a blob
az storage blob upload \
  --account-name mystorageaccount \
  --container-name mycontainer \
  --name myblob.txt \
  --file ./local-file.txt

# List blobs
az storage blob list \
  --account-name mystorageaccount \
  --container-name mycontainer \
  --output table

# Download a blob
az storage blob download \
  --account-name mystorageaccount \
  --container-name mycontainer \
  --name myblob.txt \
  --file ./downloaded-file.txt

# Delete a blob
az storage blob delete \
  --account-name mystorageaccount \
  --container-name mycontainer \
  --name myblob.txt

# Generate SAS token
az storage blob generate-sas \
  --account-name mystorageaccount \
  --container-name mycontainer \
  --name myblob.txt \
  --permissions r \
  --expiry 2024-12-31T23:59:59Z
```

### File Shares

```bash
# Create file share
az storage share create \
  --name myshare \
  --account-name mystorageaccount

# List file shares
az storage share list \
  --account-name mystorageaccount \
  --output table

# Upload file to share
az storage file upload \
  --account-name mystorageaccount \
  --share-name myshare \
  --source ./local-file.txt

# List files in share
az storage file list \
  --account-name mystorageaccount \
  --share-name myshare \
  --output table

# Delete file share
az storage share delete \
  --name myshare \
  --account-name mystorageaccount
```

---

## Databases

### Azure SQL Database

```bash
# Create SQL server
az sql server create \
  --name mysqlserver \
  --resource-group myResourceGroup \
  --location eastus \
  --admin-user sqladmin \
  --admin-password MyP@ssw0rd123

# List SQL servers
az sql server list --output table

# Create SQL database
az sql db create \
  --resource-group myResourceGroup \
  --server mysqlserver \
  --name mydatabase \
  --service-objective S0

# List databases
az sql db list \
  --resource-group myResourceGroup \
  --server mysqlserver \
  --output table

# Show database details
az sql db show \
  --resource-group myResourceGroup \
  --server mysqlserver \
  --name mydatabase

# Create firewall rule
az sql server firewall-rule create \
  --resource-group myResourceGroup \
  --server mysqlserver \
  --name AllowMyIP \
  --start-ip-address 1.2.3.4 \
  --end-ip-address 1.2.3.4

# Allow Azure services
az sql server firewall-rule create \
  --resource-group myResourceGroup \
  --server mysqlserver \
  --name AllowAzureServices \
  --start-ip-address 0.0.0.0 \
  --end-ip-address 0.0.0.0

# Get connection string
az sql db show-connection-string \
  --client ado.net \
  --name mydatabase \
  --server mysqlserver
```

### Azure Database for PostgreSQL/MySQL

```bash
# Create PostgreSQL server
az postgres flexible-server create \
  --name mypostgresserver \
  --resource-group myResourceGroup \
  --location eastus \
  --admin-user myadmin \
  --admin-password MyP@ssw0rd123 \
  --sku-name Standard_B1ms

# Create MySQL server
az mysql flexible-server create \
  --name mymysqlserver \
  --resource-group myResourceGroup \
  --location eastus \
  --admin-user myadmin \
  --admin-password MyP@ssw0rd123 \
  --sku-name Standard_B1ms

# List servers
az postgres flexible-server list --output table
az mysql flexible-server list --output table

# Create database
az postgres flexible-server db create \
  --resource-group myResourceGroup \
  --server-name mypostgresserver \
  --database-name mydatabase

# Configure firewall
az postgres flexible-server firewall-rule create \
  --resource-group myResourceGroup \
  --name mypostgresserver \
  --rule-name AllowMyIP \
  --start-ip-address 1.2.3.4 \
  --end-ip-address 1.2.3.4
```

### Cosmos DB

```bash
# Create Cosmos DB account
az cosmosdb create \
  --name mycosmosdb \
  --resource-group myResourceGroup \
  --locations regionName=eastus failoverPriority=0

# List Cosmos DB accounts
az cosmosdb list --output table

# Create database
az cosmosdb sql database create \
  --account-name mycosmosdb \
  --resource-group myResourceGroup \
  --name mydatabase

# Create container
az cosmosdb sql container create \
  --account-name mycosmosdb \
  --resource-group myResourceGroup \
  --database-name mydatabase \
  --name mycontainer \
  --partition-key-path "/id"

# Get connection string
az cosmosdb keys list \
  --name mycosmosdb \
  --resource-group myResourceGroup \
  --type connection-strings
```

---

## Networking

### Virtual Networks

```bash
# Create virtual network
az network vnet create \
  --resource-group myResourceGroup \
  --name myVNet \
  --address-prefix 10.0.0.0/16 \
  --subnet-name mySubnet \
  --subnet-prefix 10.0.1.0/24

# List virtual networks
az network vnet list --output table

# Create subnet
az network vnet subnet create \
  --resource-group myResourceGroup \
  --vnet-name myVNet \
  --name mySubnet2 \
  --address-prefix 10.0.2.0/24

# List subnets
az network vnet subnet list \
  --resource-group myResourceGroup \
  --vnet-name myVNet \
  --output table

# Delete virtual network
az network vnet delete \
  --resource-group myResourceGroup \
  --name myVNet
```

### Network Security Groups

```bash
# Create NSG
az network nsg create \
  --resource-group myResourceGroup \
  --name myNSG

# List NSGs
az network nsg list --output table

# Create NSG rule
az network nsg rule create \
  --resource-group myResourceGroup \
  --nsg-name myNSG \
  --name Allow-HTTP \
  --priority 100 \
  --source-address-prefixes '*' \
  --source-port-ranges '*' \
  --destination-address-prefixes '*' \
  --destination-port-ranges 80 \
  --access Allow \
  --protocol Tcp

# List NSG rules
az network nsg rule list \
  --resource-group myResourceGroup \
  --nsg-name myNSG \
  --output table

# Delete NSG rule
az network nsg rule delete \
  --resource-group myResourceGroup \
  --nsg-name myNSG \
  --name Allow-HTTP
```

### Public IP Addresses

```bash
# Create public IP
az network public-ip create \
  --resource-group myResourceGroup \
  --name myPublicIP \
  --allocation-method Static \
  --sku Standard

# List public IPs
az network public-ip list --output table

# Show public IP details
az network public-ip show \
  --resource-group myResourceGroup \
  --name myPublicIP

# Delete public IP
az network public-ip delete \
  --resource-group myResourceGroup \
  --name myPublicIP
```

### Load Balancers

```bash
# Create load balancer
az network lb create \
  --resource-group myResourceGroup \
  --name myLoadBalancer \
  --sku Standard \
  --public-ip-address myPublicIP

# List load balancers
az network lb list --output table

# Create backend pool
az network lb address-pool create \
  --resource-group myResourceGroup \
  --lb-name myLoadBalancer \
  --name myBackendPool

# Create health probe
az network lb probe create \
  --resource-group myResourceGroup \
  --lb-name myLoadBalancer \
  --name myHealthProbe \
  --protocol tcp \
  --port 80

# Create load balancing rule
az network lb rule create \
  --resource-group myResourceGroup \
  --lb-name myLoadBalancer \
  --name myHTTPRule \
  --protocol tcp \
  --frontend-port 80 \
  --backend-port 80 \
  --frontend-ip-name LoadBalancerFrontEnd \
  --backend-pool-name myBackendPool \
  --probe-name myHealthProbe
```

---

## Container Services

### Azure Container Instances (ACI)

```bash
# Create container instance
az container create \
  --resource-group myResourceGroup \
  --name mycontainer \
  --image mcr.microsoft.com/azuredocs/aci-helloworld \
  --dns-name-label mycontainer-dns \
  --ports 80

# List container instances
az container list --output table

# Show container details
az container show \
  --resource-group myResourceGroup \
  --name mycontainer

# Get container logs
az container logs \
  --resource-group myResourceGroup \
  --name mycontainer

# Execute command in container
az container exec \
  --resource-group myResourceGroup \
  --name mycontainer \
  --exec-command "/bin/bash"

# Delete container
az container delete \
  --resource-group myResourceGroup \
  --name mycontainer --yes
```

### Azure Container Registry (ACR)

```bash
# Create container registry
az acr create \
  --resource-group myResourceGroup \
  --name myregistry \
  --sku Basic

# List registries
az acr list --output table

# Login to registry
az acr login --name myregistry

# Build image in ACR
az acr build \
  --registry myregistry \
  --image myapp:v1 .

# List images in registry
az acr repository list --name myregistry --output table

# List tags for an image
az acr repository show-tags \
  --name myregistry \
  --repository myapp \
  --output table

# Delete image
az acr repository delete \
  --name myregistry \
  --image myapp:v1
```

### Azure Kubernetes Service (AKS)

```bash
# Create AKS cluster
az aks create \
  --resource-group myResourceGroup \
  --name myAKSCluster \
  --node-count 2 \
  --enable-addons monitoring \
  --generate-ssh-keys

# List AKS clusters
az aks list --output table

# Get AKS credentials
az aks get-credentials \
  --resource-group myResourceGroup \
  --name myAKSCluster

# Scale AKS cluster
az aks scale \
  --resource-group myResourceGroup \
  --name myAKSCluster \
  --node-count 3

# Upgrade AKS cluster
az aks upgrade \
  --resource-group myResourceGroup \
  --name myAKSCluster \
  --kubernetes-version 1.28.0

# Stop AKS cluster (save costs)
az aks stop \
  --resource-group myResourceGroup \
  --name myAKSCluster

# Start AKS cluster
az aks start \
  --resource-group myResourceGroup \
  --name myAKSCluster

# Delete AKS cluster
az aks delete \
  --resource-group myResourceGroup \
  --name myAKSCluster --yes
```

---

## Azure Functions

### Creating and Managing Functions

```bash
# Create storage account for functions
az storage account create \
  --name myfunctionsstorage \
  --location eastus \
  --resource-group myResourceGroup \
  --sku Standard_LRS

# Create function app
az functionapp create \
  --resource-group myResourceGroup \
  --name myFunctionApp \
  --storage-account myfunctionsstorage \
  --runtime node \
  --runtime-version 18 \
  --functions-version 4 \
  --os-type Linux

# List function apps
az functionapp list --output table

# Show function app details
az functionapp show \
  --name myFunctionApp \
  --resource-group myResourceGroup

# Configure function app settings
az functionapp config appsettings set \
  --name myFunctionApp \
  --resource-group myResourceGroup \
  --settings KEY=VALUE

# Deploy from ZIP
az functionapp deployment source config-zip \
  --resource-group myResourceGroup \
  --name myFunctionApp \
  --src function-app.zip

# List functions
az functionapp function list \
  --name myFunctionApp \
  --resource-group myResourceGroup

# Get function app URL
az functionapp show \
  --name myFunctionApp \
  --resource-group myResourceGroup \
  --query defaultHostName -o tsv
```

---

## Key Vault

### Managing Secrets

```bash
# Create Key Vault
az keyvault create \
  --name myKeyVault \
  --resource-group myResourceGroup \
  --location eastus

# List Key Vaults
az keyvault list --output table

# Set a secret
az keyvault secret set \
  --vault-name myKeyVault \
  --name mySecret \
  --value "SuperSecretValue"

# Get a secret
az keyvault secret show \
  --vault-name myKeyVault \
  --name mySecret

# List secrets
az keyvault secret list \
  --vault-name myKeyVault \
  --output table

# Delete a secret
az keyvault secret delete \
  --vault-name myKeyVault \
  --name mySecret

# Set access policy
az keyvault set-policy \
  --name myKeyVault \
  --upn user@domain.com \
  --secret-permissions get list set delete
```

### Managing Keys and Certificates

```bash
# Create a key
az keyvault key create \
  --vault-name myKeyVault \
  --name myKey \
  --protection software

# List keys
az keyvault key list \
  --vault-name myKeyVault \
  --output table

# Import certificate
az keyvault certificate import \
  --vault-name myKeyVault \
  --name myCert \
  --file certificate.pfx \
  --password certPassword

# List certificates
az keyvault certificate list \
  --vault-name myKeyVault \
  --output table
```

---

## Monitoring and Diagnostics

### Resource Monitoring

```bash
# Get activity log
az monitor activity-log list --output table

# Get activity log for resource group
az monitor activity-log list \
  --resource-group myResourceGroup \
  --output table

# Get metrics
az monitor metrics list \
  --resource <resource-id> \
  --metric "Percentage CPU" \
  --output table

# Create alert rule
az monitor metrics alert create \
  --name myAlert \
  --resource-group myResourceGroup \
  --scopes <resource-id> \
  --condition "avg Percentage CPU > 80" \
  --description "Alert when CPU exceeds 80%"

# List alerts
az monitor metrics alert list \
  --resource-group myResourceGroup \
  --output table
```

### Log Analytics

```bash
# Create Log Analytics workspace
az monitor log-analytics workspace create \
  --resource-group myResourceGroup \
  --workspace-name myWorkspace

# Query logs
az monitor log-analytics query \
  --workspace myWorkspace \
  --analytics-query "AzureActivity | take 10"
```

---

## Common Workflows

### Deploy a Web Application

```bash
# 1. Create resource group
az group create --name myWebAppRG --location eastus

# 2. Create App Service plan
az appservice plan create \
  --name myAppPlan \
  --resource-group myWebAppRG \
  --sku B1 \
  --is-linux

# 3. Create web app
az webapp create \
  --resource-group myWebAppRG \
  --plan myAppPlan \
  --name myUniqueWebApp123 \
  --runtime "NODE:18-lts"

# 4. Configure app settings
az webapp config appsettings set \
  --resource-group myWebAppRG \
  --name myUniqueWebApp123 \
  --settings NODE_ENV=production

# 5. Deploy code
az webapp deploy \
  --resource-group myWebAppRG \
  --name myUniqueWebApp123 \
  --src-path app.zip \
  --type zip

# 6. Browse to app
az webapp browse --resource-group myWebAppRG --name myUniqueWebApp123
```

### Set Up a Complete Development Environment

```bash
# 1. Create resource group
az group create --name myDevRG --location eastus

# 2. Create virtual network
az network vnet create \
  --resource-group myDevRG \
  --name myVNet \
  --address-prefix 10.0.0.0/16 \
  --subnet-name default \
  --subnet-prefix 10.0.1.0/24

# 3. Create VM for development
az vm create \
  --resource-group myDevRG \
  --name myDevVM \
  --image Ubuntu2204 \
  --vnet-name myVNet \
  --subnet default \
  --admin-username azureuser \
  --generate-ssh-keys

# 4. Create PostgreSQL database
az postgres flexible-server create \
  --name mydevdb \
  --resource-group myDevRG \
  --location eastus \
  --admin-user dbadmin \
  --admin-password MyP@ssw0rd123 \
  --sku-name Standard_B1ms \
  --tier Burstable \
  --storage-size 32

# 5. Create storage account
az storage account create \
  --name mydevstorage123 \
  --resource-group myDevRG \
  --location eastus \
  --sku Standard_LRS
```

### Container Application Deployment

```bash
# 1. Create container registry
az acr create \
  --resource-group myResourceGroup \
  --name myregistry123 \
  --sku Basic

# 2. Build and push image
az acr build \
  --registry myregistry123 \
  --image myapp:v1 .

# 3. Create AKS cluster
az aks create \
  --resource-group myResourceGroup \
  --name myAKSCluster \
  --node-count 2 \
  --attach-acr myregistry123 \
  --generate-ssh-keys

# 4. Get credentials
az aks get-credentials \
  --resource-group myResourceGroup \
  --name myAKSCluster

# 5. Deploy to AKS (using kubectl)
kubectl apply -f deployment.yaml
```

---

## Tips and Best Practices

### Output Formatting

```bash
# Table format (readable)
az vm list --output table

# JSON format (default, detailed)
az vm list --output json

# JSON Compact
az vm list --output jsonc

# TSV (tab-separated, good for scripting)
az vm list --output tsv

# YAML format
az vm list --output yaml

# No output
az vm delete --name myVM --resource-group myRG --yes --no-wait --output none
```

### Querying Results with JMESPath

```bash
# Get specific fields
az vm list --query "[].{Name:name, Location:location}" --output table

# Filter results
az vm list --query "[?location=='eastus']" --output table

# Get first result
az vm list --query "[0].name" --output tsv

# Get array of values
az vm list --query "[].name" --output tsv

# Complex query
az vm list \
  --query "[?powerState=='VM running'].{Name:name, Size:hardwareProfile.vmSize}" \
  --output table
```

### Using Variables

```bash
# Store resource group name
RG="myResourceGroup"
LOCATION="eastus"

# Use variables
az group create --name $RG --location $LOCATION
az vm create --resource-group $RG --name myVM --image Ubuntu2204
```

### Scripting Best Practices

```bash
# Check if command succeeded
if az group create --name myRG --location eastus; then
  echo "Resource group created successfully"
else
  echo "Failed to create resource group"
  exit 1
fi

# Capture output
VM_IP=$(az vm show -d --resource-group myRG --name myVM --query publicIps -o tsv)
echo "VM IP: $VM_IP"

# Loop through resources
for vm in $(az vm list --resource-group myRG --query "[].name" -o tsv); do
  echo "Processing $vm"
  az vm start --resource-group myRG --name $vm
done
```

### Cost Management

```bash
# Deallocate VMs when not in use (stops billing for compute)
az vm deallocate --resource-group myRG --name myVM

# Stop AKS cluster when not needed
az aks stop --resource-group myRG --name myAKS

# Use lower-tier SKUs for development
az vm create --size Standard_B1s  # Instead of Standard_D4s_v3

# Delete resources when done
az group delete --name myDevRG --yes --no-wait

# List all resources to identify unused ones
az resource list --output table
```

### Useful Aliases

```bash
# Add to ~/.bashrc or ~/.zshrc

alias azl='az login'
alias azls='az account list --output table'
alias azset='az account set --subscription'
alias azrg='az group list --output table'
alias azvm='az vm list --output table'
alias azapp='az webapp list --output table'
```

### Interactive Mode Tips

```bash
# Start interactive mode (better autocomplete)
az interactive

# Features in interactive mode:
# - Tab completion
# - Command suggestions
# - Inline documentation
# - Syntax highlighting
# - Query suggestions
```

### Common Pitfalls to Avoid

1. **Forgetting to set default subscription**
   ```bash
   az account set --subscription "My Subscription"
   ```

2. **Not using --no-wait for long operations**
   ```bash
   az vm create ... --no-wait  # Don't block on VM creation
   ```

3. **Hardcoding resource names** (use variables)
   ```bash
   RG="myResourceGroup"
   az group create --name $RG --location eastus
   ```

4. **Not cleaning up resources**
   ```bash
   az group delete --name myRG --yes --no-wait
   ```

5. **Not checking if resource exists**
   ```bash
   if az group exists --name myRG; then
     echo "Resource group exists"
   fi
   ```

---

## Quick Reference

### Essential Commands
```bash
az login                                # Login to Azure
az account list                         # List subscriptions
az account set --subscription <name>    # Set active subscription
az group create --name <rg> --location <loc>  # Create resource group
az group list                           # List resource groups
az resource list                        # List all resources
az vm list                              # List VMs
az webapp list                          # List web apps
az storage account list                 # List storage accounts
```

### Common Patterns
```bash
# Create VM
az vm create --resource-group <rg> --name <name> --image Ubuntu2204

# Create web app
az webapp create --resource-group <rg> --plan <plan> --name <name>

# Create storage account
az storage account create --name <name> --resource-group <rg>

# Delete resource group (and all resources)
az group delete --name <rg> --yes --no-wait
```

### Query and Output
```bash
--output table                          # Table format
--output json                           # JSON format
--query "[].name"                       # Query specific fields
--no-wait                               # Don't wait for operation
--yes                                   # Skip confirmation
```

---

## Additional Resources

- [Azure CLI Documentation](https://learn.microsoft.com/en-us/cli/azure/)
- [Azure CLI GitHub Repository](https://github.com/Azure/azure-cli)
- [JMESPath Query Language](https://jmespath.org/)
- [Azure CLI Extensions](https://learn.microsoft.com/en-us/cli/azure/azure-cli-extensions-list)
- [Azure Pricing Calculator](https://azure.microsoft.com/en-us/pricing/calculator/)
- [Azure Architecture Center](https://learn.microsoft.com/en-us/azure/architecture/)
