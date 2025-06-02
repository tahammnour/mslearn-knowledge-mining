# 🔍 Azure AI Search Skills - Lab 02

This lab demonstrates how to create and manage custom search skills in Azure AI Search, extending the basic search capabilities with advanced cognitive services and custom processing.

## 📋 Overview

Lab 02 builds upon the foundation from Lab 01, focusing on creating and updating custom skillsets that enhance document processing with AI-powered enrichments. This lab teaches you how to:

- 🛠️ Create custom skillsets with cognitive services
- 📊 Configure data sources and indexes programmatically  
- 🔄 Update existing search solutions with new capabilities
- 🤖 Implement advanced AI skills for document enrichment

## 📁 Folder Structure

```
02-search-skill/
├── 📄 README.md              # This documentation
├── 🔧 setup.cmd             # Windows setup script
├── 🔧 setup.sh              # Linux/macOS setup script
├── 📁 C-Sharp/              # C# implementation (not covered)
├── 📁 Python/               # Python implementation
├── 📁 data/                 # Sample documents for processing
│   ├── 📂 collateral/       # Marketing materials
│   └── 📂 reviews/          # Customer reviews
├── 📁 create-search/        # 🚀 Initial search solution creation
│   ├── 📄 README.md        # Creation workflow documentation
│   ├── 🔧 create-search.cmd # Windows creation script
│   ├── 🔧 create-search.sh  # Linux/macOS creation script
│   ├── 📋 data_source.json  # Data source configuration
│   ├── 📋 index.json        # Search index schema
│   ├── 📋 indexer.json      # Indexer configuration
│   └── 📋 skillset.json     # Custom skillset definition
└── 📁 update-search/        # 🔄 Search solution updates
    ├── 📄 README.md         # Update workflow documentation
    ├── 🔧 update-search.cmd  # Windows update script
    ├── 🔧 update-search.sh   # Linux/macOS update script
    ├── 📋 update-index.json  # Updated index schema
    ├── 📋 update-indexer.json # Updated indexer config
    └── 📋 update-skillset.json # Updated skillset definition
```

## 🎯 Learning Objectives

By completing this lab, you will learn how to:

1. 🏗️ **Create Custom Skillsets**: Build AI-powered processing pipelines
2. 📝 **Configure Data Sources**: Set up connections to Azure Storage
3. 🗂️ **Design Search Indexes**: Define schemas for enriched content
4. ⚙️ **Deploy Indexers**: Automate document processing workflows
5. 🔄 **Update Search Solutions**: Modify existing configurations
6. 🧪 **Test AI Enrichments**: Validate cognitive skill outputs

## 🛠️ Prerequisites

Before starting this lab, ensure you have:

- ✅ **Azure Subscription** with appropriate permissions
- ✅ **Azure AI Search Service** (Basic tier or higher)
- ✅ **Azure AI Services** resource (for cognitive skills)
- ✅ **Azure Storage Account** with blob containers
- ✅ **Azure CLI** installed and configured
- ✅ **Visual Studio Code** or similar editor
- ✅ **Bash shell** (Linux/macOS) or PowerShell (Windows)

## 🚀 Quick Start Guide

### 1️⃣ Initial Setup

First, configure your Azure environment:

```bash
# 🔧 Make setup script executable
chmod +x setup.sh

# ✏️ Edit setup.sh with your Azure details
nano setup.sh

# 🚀 Run the setup script
./setup.sh
```

### 2️⃣ Create Search Solution

Navigate to the create-search folder and deploy:

```bash
cd create-search/
chmod +x create-search.sh
./create-search.sh
```

### 3️⃣ Update Search Solution

Enhance your solution with additional features:

```bash
cd ../update-search/
chmod +x update-search.sh
./update-search.sh
```

## 🎛️ Configuration Details

### 🔑 Required Azure Resources

This lab requires three main Azure resources:

1. **🔍 Azure AI Search Service**
   - Handles indexing and querying
   - Minimum: Basic tier
   - Location: Must match AI Services

2. **🧠 Azure AI Services**
   - Provides cognitive skills
   - Standard S0 pricing tier
   - Same region as Search service

3. **💾 Azure Storage Account**
   - Stores source documents
   - Standard LRS performance
   - Public blob access enabled

### 📝 Setup Script Configuration

Edit `setup.sh` with your specific values:

```bash
subscription_id="YOUR_SUBSCRIPTION_ID"
resource_group="YOUR_RESOURCE_GROUP" 
location="YOUR_LOCATION_NAME"
```

The setup script will:
- 🏗️ Create a unique storage account
- 📤 Upload sample documents
- 📋 Generate connection strings
- 🔑 Output configuration values

### ⚠️ Automatically Created Azure Resources

When working with Azure AI Search and cognitive skills, Azure may automatically create additional resource groups and resources in your subscription:

#### 🏗️ DefaultResourceGroup-EUS2 (or similar)
Azure automatically creates a default resource group (e.g., `DefaultResourceGroup-EUS2`) when you enable certain AI services or cognitive enrichments. This resource group typically contains:

- **🔬 DefaultWorkspace-[GUID]-EUS2**: A workspace for machine learning and AI services
- **📊 Application Insights**: For monitoring and telemetry
- **🔑 Key Vault**: For secure credential storage
- **💾 Storage Account**: For temporary data and model storage
- **🧠 Container Registry**: For custom skill containers (if used)

#### 🤔 Are These Resources Required?

**During Development/Testing:**
- ✅ **Keep them**: Required for cognitive skills to function properly
- ✅ **Monitor usage**: These resources may incur costs
- ✅ **Check billing**: Review Azure cost analysis regularly

**After Lab Completion:**
- 🗑️ **Can be deleted**: If you're not using cognitive enrichment in production
- ⚠️ **Verify dependencies**: Ensure no other services depend on them
- 💰 **Cost consideration**: Keeping unused resources incurs charges

#### 🧹 Resource Management

To check what was automatically created:

```bash
# 📊 List all resource groups
az group list --output table

# 🔍 Check resources in the default group
az resource list --resource-group DefaultResourceGroup-EUS2 --output table

# 💰 Check resource costs
az consumption usage list --top 10
```

To safely clean up after lab completion:

```bash
# ⚠️ CAUTION: This will delete ALL resources in the group
# 🔍 First, verify what's in the resource group
az resource list --resource-group DefaultResourceGroup-EUS2

# 🗑️ Delete only if you're sure nothing else depends on it
az group delete --name DefaultResourceGroup-EUS2 --yes --no-wait
```

#### 🛡️ Best Practices

- 📋 **Document created resources**: Keep track of what Azure creates automatically
- 💰 **Monitor costs**: Set up billing alerts for unexpected charges
- 🔒 **Use resource tags**: Tag resources for easier management
- 🗂️ **Organize resource groups**: Use meaningful names for your own resource groups
- ⏰ **Regular cleanup**: Schedule periodic cleanup of unused resources

## 🎨 Custom Skills Overview

This lab implements several cognitive skills:

### 🧠 Built-in Cognitive Skills

- **📈 Sentiment Analysis**: Evaluates document emotion
- **🔑 Key Phrase Extraction**: Identifies important terms
- **🌍 Location Recognition**: Extracts geographical references
- **🔤 Language Detection**: Identifies document language
- **🖼️ Image Analysis**: Processes embedded images
- **📝 OCR**: Extracts text from images

### 🎯 Custom Skills (Advanced)

- **🏷️ Custom Entity Recognition**: Domain-specific entities
- **📊 Custom Classification**: Document categorization
- **🔍 Custom Text Processing**: Specialized content analysis

## 📊 Data Processing Pipeline

The skillset processes documents through these stages:

```
📄 Source Documents → 🧠 Cognitive Skills → 📊 Enriched Index → 🔍 Search Results
```

1. **📥 Document Ingestion**: Files from Azure Storage
2. **🔤 Text Extraction**: OCR and content parsing
3. **🧠 AI Processing**: Cognitive skills application
4. **📊 Index Population**: Enriched data storage
5. **🔍 Search Enablement**: Query-ready content

## 🔧 Shell Scripts Explained

### 📋 setup.sh
```bash
#!/bin/bash
# 🎯 Purpose: Create Azure resources and upload sample data
# 📥 Input: Azure subscription and resource group details
# 📤 Output: Storage account with uploaded documents
# 🔑 Generates: Connection strings and access keys
```

### 🚀 create-search.sh
```bash
#!/bin/bash
# 🎯 Purpose: Create initial search solution components
# 📊 Creates: Data source, skillset, index, and indexer
# 🔄 Process: Uses Azure Search REST API
# ⏱️ Duration: 3-5 minutes for completion
```

### 🔄 update-search.sh
```bash
#!/bin/bash
# 🎯 Purpose: Update existing search solution with enhancements
# 🔧 Updates: Skillset, index schema, and indexer configuration
# 📈 Adds: New cognitive skills and field mappings
# 🔄 Process: Incremental updates to existing resources
```

## 🧪 Testing Your Solution

### 🔍 Search Explorer Testing

1. Navigate to your Azure AI Search service
2. Open Search Explorer
3. Try these test queries:

```json
{
  "search": "*",
  "count": true
}
```

```json
{
  "search": "hotel london",
  "select": "content,sentiment,keyphrases",
  "count": true
}
```

### 📊 Verify Enrichments

Check that cognitive skills are working:
- ✅ Sentiment analysis produces labels
- ✅ Key phrases are extracted
- ✅ Locations are identified
- ✅ Language is detected

## 🔧 Troubleshooting

### ❌ Common Issues

1. **🔑 Authentication Errors**
   ```bash
   # Solution: Verify API keys and endpoints
   echo $admin_key  # Check if key is set
   ```

2. **📡 Connection Timeouts**
   ```bash
   # Solution: Check network connectivity
   curl -I $url  # Test endpoint accessibility
   ```

3. **📋 JSON Format Errors**
   ```bash
   # Solution: Validate JSON files
   cat skillset.json | jq .  # Check JSON syntax
   ```

4. **🔄 Indexer Failures**
   ```bash
   # Solution: Check indexer status
   curl -H "api-key: $admin_key" "$url/indexers/margies-custom-indexer/status?api-version=2020-06-30"
   ```

### 🛠️ Debug Commands

```bash
# 📊 Check indexer status
curl -H "api-key: $admin_key" "$url/indexers?api-version=2020-06-30"

# 🔍 List all indexes
curl -H "api-key: $admin_key" "$url/indexes?api-version=2020-06-30"

# 🧠 View skillset details
curl -H "api-key: $admin_key" "$url/skillsets?api-version=2020-06-30"
```

## 📈 Performance Optimization

### ⚡ Best Practices

- 🎯 **Field Selection**: Only index necessary fields
- 📊 **Batch Size**: Optimize indexer batch sizes
- 🔄 **Incremental Updates**: Use change tracking
- 💾 **Resource Sizing**: Right-size your search service

### 📊 Monitoring

Monitor your search solution:
- 📈 **Query Performance**: Response times
- 💾 **Storage Usage**: Index size growth
- 🔄 **Indexer Success**: Processing rates
- 🧠 **Skill Execution**: Cognitive service usage

## 🚀 Next Steps

After completing this lab:

1. 🔬 **Explore Lab 03**: Knowledge Store implementation
2. 🎨 **Custom Skills**: Build your own processing logic
3. 📊 **Advanced Indexing**: Complex document structures
4. 🔍 **Search Tuning**: Relevance and performance optimization

## 📚 Additional Resources

- 🔍 [Azure AI Search Documentation](https://docs.microsoft.com/en-us/azure/search/)
- 🧠 [Cognitive Skills Reference](https://docs.microsoft.com/en-us/azure/search/cognitive-search-predefined-skills)
- 🛠️ [REST API Documentation](https://docs.microsoft.com/en-us/rest/api/searchservice/)
- 🎓 [Microsoft Learn Path](https://docs.microsoft.com/en-us/learn/paths/implement-knowledge-mining-azure-cognitive-search/)

## ⚠️ Important: Automatically Created Azure Resources

When working with Azure AI Search custom skills and cognitive enrichments, you may notice that Azure automatically creates additional resources in your subscription:

### 🤖 Auto-Created Resources

**Resource Group**: `DefaultResourceGroup-EUS2` (or similar region suffix)
- **Workspace**: `DefaultWorkspace-{GUID}-{REGION}` 
- **Associated Resources**: Application Insights, Storage Accounts, Key Vault, etc.

### 🔍 Why These Resources Are Created

These resources are automatically provisioned when:
- 🧠 **Cognitive Skills** are enabled with Azure AI Services
- 🛠️ **Custom Skills** are deployed using Azure Functions
- 📊 **Application Insights** monitoring is enabled for cognitive services
- 🔐 **Key Vault** integration is used for secure key management

### 💡 Resource Management Guidelines

#### ✅ **Keep These Resources If:**
- 🔄 Your search solution is actively using cognitive skills
- 📊 You need monitoring and telemetry data
- 🛠️ Custom skills are deployed and in use
- 🔐 Secure key management is required

#### 🗑️ **Safe to Delete If:**
- ✅ Cognitive enrichment process is completely finished
- ✅ No active indexers are using cognitive skills
- ✅ Custom skills have been migrated or are no longer needed
- ✅ You don't need historical monitoring data

### 🧪 Verification Steps

Before deleting auto-created resources:

```bash
# 🔍 Check if cognitive services are still in use
curl -H "api-key: $admin_key" "$url/skillsets?api-version=2020-06-30"

# 📊 Verify no active indexers depend on these resources
curl -H "api-key: $admin_key" "$url/indexers?api-version=2020-06-30"

# 🛠️ List custom skills that might use Azure Functions
curl -H "api-key: $admin_key" "$url/skillsets/margies-custom-skillset?api-version=2020-06-30" | jq '.skills[] | select(."@odata.type" == "#Microsoft.Skills.Custom.WebApiSkill")'
```

### 💰 Cost Considerations

These auto-created resources may incur costs:
- 📊 **Application Insights**: Data ingestion and retention
- 💾 **Storage Accounts**: Blob storage and transactions
- 🔐 **Key Vault**: Operations and managed keys
- 🖥️ **Workspace**: Compute and storage usage

### 🧹 Safe Cleanup Process

1. **🔍 Audit Dependencies**: Verify no active cognitive skills
2. **📊 Export Data**: Save any needed monitoring data
3. **🛑 Stop Indexers**: Pause any running indexers
4. **🗑️ Delete Resources**: Remove auto-created resource group
5. **✅ Verify**: Ensure your search solution still works

```bash
# 🛑 Stop all indexers first
curl -X POST -H "api-key: $admin_key" "$url/indexers/margies-custom-indexer/stop?api-version=2020-06-30"

# 🗑️ Delete auto-created resource group (be careful!)
az group delete --name "DefaultResourceGroup-EUS2" --yes --no-wait
```

## 🧹 Complete Cleanup

After completing the lab:

```bash
# 🗑️ Delete your main resource group (removes all resources)
az group delete --name YOUR_RESOURCE_GROUP --yes --no-wait

# 🗑️ Optionally delete auto-created resources (if no longer needed)
az group delete --name "DefaultResourceGroup-EUS2" --yes --no-wait
```

### ⚠️ **Warning**: Always verify dependencies before deleting auto-created resources!

---

📝 **Note**: This lab is part of the Microsoft Learn Knowledge Mining learning path. Complete prerequisites before starting.
