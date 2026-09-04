# Bicep infrastructure

`main.bicep` provisions the Azure resources required by the Renewable Energy Compliance Lab:

- Azure AI Search
- Azure AI Document Intelligence
- Azure AI Vision
- Azure OpenAI account and chat model deployment
- Storage account for Azure Functions
- Application Insights
- Linux Azure Function App
- Managed identity role assignment for the Function App to call Azure OpenAI

Deploy it with:

```powershell
az group create --name rg-reccia-graph-ingestion --location eastus

az deployment group create `
  --resource-group rg-reccia-graph-ingestion `
  --template-file infra\main.bicep `
  --parameters @infra\main.parameters.example.json
```

The repo script `scripts\deploy-azure-resources.ps1` wraps this deployment and is the recommended path for lab participants.

After Bicep creates the infrastructure, run `scripts\deploy-function-api.ps1` to deploy the Function code and inject the Azure AI Search query key into app settings.

## Strict tenant policies

Some tenants block storage shared-key access or require private networking for Function storage. In that case, use the same resources from this template, but adapt the Function hosting to Flex Consumption with managed-identity storage access and private endpoints for blob, queue, and table storage. See `docs\troubleshooting.md`.

