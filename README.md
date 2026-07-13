# Play.Infrastructure

Project handling infrastructure part of Play.Microservices project.

## Start docker compose in detached mode (to not see all output)

docker compose up -d

## Add the GitHub package source

```powershell
$owner="Furrman"
$gh_pat="[PAT HERE]"

dotnet nuget add source --username USERNAME --password $gh_pat --store-password-in-clear-text --name github "https://nuget.pkg.github.com/$owner/index.json"
```

dotnet nuget list source