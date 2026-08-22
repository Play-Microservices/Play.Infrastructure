# Play.Infrastructure

Project handling infrastructure part of Play.Microservices project.

## Start docker compose in detached mode (to not see all output)

```bash
docker compose up -d
```

## Add the GitHub package source

```bash
owner="Play-Microservices"
gh_pat="[PAT HERE]"

dotnet nuget add source --username USERNAME --password $gh_pat --store-password-in-clear-text --name github "https://nuget.pkg.github.com/$owner/index.json"
```

dotnet nuget list source

## Azure

### Set variables

```bash
subscription="Play.Microservices"
appname="playmicroservices"
location="southuk"
nodeVmSize="Standard_D4lds_v6"
nodeCount=2
```

```bash
location="northeurope"
nodeVmSize="Standard_DC2ads_v6"
```

### Set subscription

```bash
az account set --subscription "$subscription"
```

### Create Resource Group

```bash
az group create \
  --name "$appname" \
  --location "$location"
```

### Create Cosmos DB

```bash
az provider register --namespace Microsoft.DocumentDB

az cosmosdb create \
  --name "$appname" \
  --resource-group "$appname" \
  --locations regionName="$location" failoverPriority=0 isZoneRedundant=False \
  --kind MongoDB \
  --enable-free-tier 
```

### Create Service Bus

```bash
az servicebus namespace create \
  --name "$appname" \
  --resource-group "$appname" \
  --sku Standard \
  --location "$location"
```

### Create Container Registry

```bash
az provider register --namespace Microsoft.ContainerRegistry

az acr create \
  --name "$appname" \
  --resource-group "$appname" \
  --sku Basic \
  --location "$location"
```

### Create AKS cluster

```bash
az provider register --namespace Microsoft.ContainerService

az aks create \
  --name "$appname" \
  --resource-group "$appname" \
  --node-vm-size "$nodeVmSize" \
  --node-count "$nodeCount" \
  --attach-acr "$appname" \
  --enable-oidc-issuer \
  --enable-workload-identity \
  --generate-ssh-keys \
  --location "$location"
```

### Get AKS credentials

```bash
az aks get-credentials \
  --name "$appname" \
  --resource-group "$appname" 
```

### Create Key Vault

```bash
az provider register --namespace Microsoft.KeyVault

az keyvault create \
  -n "$appname" \
  -g "$appname" \
  --location "$location"
```