# Play.Infrastructure

Project handling infrastructure part of Play.Microservices project.

## Start docker compose in detached mode (to not see all output)

docker compose up -d

## Add the GitHub package source

```powershell
$owner="Play-Microservices"
$gh_pat="[PAT HERE]"

dotnet nuget add source --username USERNAME --password $gh_pat --store-password-in-clear-text --name github "https://nuget.pkg.github.com/$owner/index.json"
```

dotnet nuget list source

## Azure

### Set subscription
`az account set --subscription "Play.Microservices"`

### Creating the Azure resource group

```bash
appname="playeconomy"
location="uksouth"
az group create --name $appname --location $location
```

### Create CosmosDb

```bash
az provider register --namespace Microsoft.DocumentDB
az cosmosdb create --name $appname --resource-group $appname --kind MongoDB --enable-free-tier
```