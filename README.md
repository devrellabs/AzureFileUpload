# Azure App Service + Storage: Dev→Prod Pipeline

A simple web application demonstrating **environment-specific infrastructure** and **"build once, deploy everywhere"** CI/CD pipeline using Azure Developer CLI and GitHub Actions.

![Screenshot](./screenshot.png)

## 🚀 How It Works

The pipeline implements true **"build once, deploy everywhere"**:

1. **Package Application**: Build and package the app to `./dist/app-package.zip`
2. **Deploy to Dev**: Deploy the package to development environment (public storage)
3. **Validate**: Run tests and validation checks on dev deployment
4. **Promote to Prod**: Deploy the **same package** to production environment (private networking)

### Pipeline Flow

```
📦 Package → 🚀 Deploy Dev → 🔍 Validate → 🚀 Promote to Prod (same package)
```

**Key Benefits**:
- ✅ Same exact package deployed to both environments
- ✅ No rebuilding during promotion
- ✅ Faster production deployments
- ✅ Reduces build-related deployment issues

**Smart Environment Naming**: `myapp-dev` automatically becomes `myapp-prod`

## 🏗️ Infrastructure

Two environment configurations using Azure Bicep:

| Component | Development | Production |
|-----------|-------------|------------|
| **App Service** | B2 plan, public access | S1 plan, VNet integrated |
| **Storage** | Public access enabled | Private endpoints only |
| **Networking** | Standard | VNet + Private DNS |
| **Security** | Managed identity | Enhanced network isolation |

### Key Infrastructure Files
- `main.bicep` - Main orchestration
- `app.bicep` - App Service hosting  
- `shared.bicep` - Storage with environment-specific access
- `network.bicep` - VNet infrastructure (prod only)
- `monitoring.bicep` - Observability stack

## 🚀 Quick Start

### Prerequisites
- Azure subscription
- GitHub repository with these variables set:
  - `AZURE_CLIENT_ID`, `AZURE_TENANT_ID`, `AZURE_SUBSCRIPTION_ID`
  - `AZURE_ENV_NAME` (e.g., `myapp-dev`)
  - `AZURE_LOCATION`, `AZURE_ENV_TYPE`

### Deploy
```bash
# Manual deployment
azd up

# Or push to main branch for automated GitHub Actions deployment
```

### Environment Naming
- Dev: `myapp-dev` → Prod: `myapp-prod`  
- Dev: `staging` → Prod: `staging-prod`

## 🛡️ Security & Features

**Development**: Public storage, managed identity, HTTPS-only  
**Production**: Private networking, VNet integration, zero public storage access

## 📚 Learn More

- [Azure App Service VNet Integration](https://learn.microsoft.com/azure/app-service/tutorial-networking-isolate-vnet)
- [Azure Developer CLI](https://learn.microsoft.com/azure/developer/azure-developer-cli/)