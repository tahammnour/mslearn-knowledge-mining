# 🚀 Create Search Solution

This folder contains scripts and configuration files to create a complete Azure AI Search solution with custom skillsets and cognitive enrichments.

## 📋 Overview

The create-search workflow establishes the foundation of your search solution by creating four essential components:

1. 📊 **Data Source**: Connection to Azure Storage containing documents
2. 🧠 **Skillset**: AI-powered processing pipeline for content enrichment
3. 🗂️ **Index**: Schema defining searchable fields and their properties
4. ⚙️ **Indexer**: Automated processor that orchestrates the entire pipeline

## 📁 Files Structure

```
create-search/
├── 📄 README.md           # This documentation
├── 🔧 create-search.cmd   # Windows batch script
├── 🔧 create-search.sh    # 🐧 Linux/macOS bash script
├── 📋 data_source.json    # Data source configuration
├── 📋 skillset.json       # Custom skillset definition
├── 📋 index.json          # Search index schema
└── 📋 indexer.json        # Indexer configuration
```

## 🛠️ Prerequisites

Before running the creation scripts, ensure you have:

- ✅ **Azure AI Search Service** deployed
- ✅ **Azure AI Services** resource created
- ✅ **Azure Storage Account** with uploaded documents
- ✅ **Admin API Key** for your search service
- ✅ **AI Services Key** for cognitive processing

## 🚀 Quick Start

### 1️⃣ Configure Your Environment

Edit the script with your Azure AI Search details:

```bash
# 📝 Edit the bash script
nano create-search.sh

# Set your specific values:
url="https://your-search-service.search.windows.net"
admin_key="YOUR_ADMIN_KEY_HERE"
```

### 2️⃣ Make Script Executable

```bash
# 🔧 Grant execution permissions
chmod +x create-search.sh
```

### 3️⃣ Run the Creation Script

```bash
# 🚀 Execute the creation workflow
./create-search.sh
```

## 🔧 Shell Script Breakdown

### 📜 create-search.sh Workflow

```bash
#!/bin/bash

# 🔑 Configuration Section
url="YOUR_SEARCH_URL"                    # Your search service endpoint
admin_key="YOUR_ADMIN_KEY"               # Administrative API key

# 🏗️ Step 1: Create Data Source
echo "Creating the data source..."
curl -X POST "${url}/datasources?api-version=2020-06-30" \
     -H "Content-Type: application/json" \
     -H "api-key: ${admin_key}" \
     -d @data_source.json
```

#### 🔍 What this does:
- 📡 **HTTP POST Request**: Creates a new data source
- 📄 **JSON Payload**: Uses `data_source.json` configuration
- 🔗 **Connection**: Links to your Azure Storage account
- 📋 **Result**: Data source named "margies-data-source"

```bash
# 🏗️ Step 2: Create Skillset
echo "Creating the skillset..."
curl -X PUT "${url}/skillsets/margies-custom-skillset?api-version=2020-06-30" \
     -H "Content-Type: application/json" \
     -H "api-key: ${admin_key}" \
     -d @skillset.json
```

#### 🧠 What this does:
- 🔄 **HTTP PUT Request**: Creates or updates skillset
- 🎯 **Named Resource**: "margies-custom-skillset"
- 🤖 **AI Skills**: Sentiment, key phrases, locations, etc.
- 🔗 **Cognitive Services**: Links to your AI Services resource

```bash
# 🏗️ Step 3: Create Index
echo "Creating the index..."
curl -X PUT "${url}/indexes/margies-custom-index?api-version=2020-06-30" \
     -H "Content-Type: application/json" \
     -H "api-key: ${admin_key}" \
     -d @index.json
```

#### 🗂️ What this does:
- 📊 **Index Schema**: Defines searchable fields
- 🔍 **Field Properties**: Searchable, filterable, sortable options
- 📈 **Enriched Fields**: AI-generated content fields
- 🔑 **Primary Key**: Document identifier configuration

```bash
# ⏱️ Wait for resources to be ready
sleep 3

# 🏗️ Step 4: Create and Run Indexer
echo "Creating the indexer..."
curl -X PUT "${url}/indexers/margies-custom-indexer?api-version=2020-06-30" \
     -H "Content-Type: application/json" \
     -H "api-key: ${admin_key}" \
     -d @indexer.json
```

#### ⚙️ What this does:
- 🔄 **Orchestration**: Coordinates data source, skillset, and index
- 📊 **Field Mapping**: Maps source data to index fields
- 🧠 **Skill Output**: Routes AI enrichments to appropriate fields
- 🚀 **Execution**: Automatically starts processing documents

## 📋 Configuration Files Explained

### 🗃️ data_source.json

```json
{
  "name": "margies-data-source",
  "type": "azureblob",
  "credentials": {
    "connectionString": "YOUR_STORAGE_CONNECTION_STRING"
  },
  "container": {
    "name": "margies-data",
    "query": null
  }
}
```

**Key Components:**
- 🏷️ **Name**: Unique identifier for the data source
- 🔗 **Type**: Azure Blob Storage connector
- 🔑 **Credentials**: Storage account connection string
- 📂 **Container**: Specific blob container with documents

### 🧠 skillset.json

```json
{
  "name": "margies-custom-skillset",
  "description": "Custom skillset for document enrichment",
  "cognitiveServices": {
    "@odata.type": "#Microsoft.Azure.Search.CognitiveServicesByKey",
    "key": "YOUR_AI_SERVICES_KEY"
  },
  "skills": [
    {
      "@odata.type": "#Microsoft.Skills.Text.V3.SentimentSkill",
      "name": "get-sentiment",
      "context": "/document",
      "inputs": [...],
      "outputs": [...]
    }
    // ... more skills
  ]
}
```

**Included Skills:**
- 📈 **Sentiment Analysis**: Document emotion evaluation
- 🔑 **Key Phrase Extraction**: Important term identification
- 🌍 **Entity Recognition**: Location and organization detection
- 🔤 **Language Detection**: Content language identification
- 🖼️ **Image Analysis**: Visual content processing
- 📝 **OCR**: Text extraction from images

### 🗂️ index.json

```json
{
  "name": "margies-custom-index",
  "fields": [
    {
      "name": "id",
      "type": "Edm.String",
      "key": true,
      "searchable": false
    },
    {
      "name": "content",
      "type": "Edm.String",
      "searchable": true,
      "filterable": false,
      "retrievable": true
    }
    // ... more fields
  ]
}
```

**Field Types:**
- 🔑 **Key Field**: Unique document identifier
- 📄 **Content Fields**: Searchable document text
- 📊 **Metadata Fields**: File properties and information
- 🧠 **Enriched Fields**: AI-generated insights
- 🏷️ **Facet Fields**: For filtering and navigation

### ⚙️ indexer.json

```json
{
  "name": "margies-custom-indexer",
  "dataSourceName": "margies-data-source",
  "targetIndexName": "margies-custom-index",
  "skillsetName": "margies-custom-skillset",
  "fieldMappings": [...],
  "outputFieldMappings": [...],
  "parameters": {
    "batchSize": 1,
    "maxFailedItems": 0
  }
}
```

**Configuration Sections:**
- 🔗 **Resource Links**: Connects data source, skillset, and index
- 📊 **Field Mappings**: Source field to index field mappings
- 🧠 **Output Mappings**: Skill outputs to index fields
- ⚙️ **Parameters**: Processing batch size and error handling

## 📊 Execution Workflow

The creation process follows this sequence:

```
1. 📊 Create Data Source
   ↓
2. 🧠 Create Skillset  
   ↓
3. 🗂️ Create Index
   ↓
4. ⏱️ Wait (3 seconds)
   ↓
5. ⚙️ Create Indexer
   ↓
6. 🚀 Automatic Processing Begins
```

## 🔍 Monitoring Progress

### 📈 Check Indexer Status

```bash
# 📊 View indexer execution status
curl -H "api-key: $admin_key" \
     "$url/indexers/margies-custom-indexer/status?api-version=2020-06-30"
```

### 🔍 Verify Created Resources

```bash
# 📋 List all data sources
curl -H "api-key: $admin_key" "$url/datasources?api-version=2020-06-30"

# 🧠 List all skillsets  
curl -H "api-key: $admin_key" "$url/skillsets?api-version=2020-06-30"

# 🗂️ List all indexes
curl -H "api-key: $admin_key" "$url/indexes?api-version=2020-06-30"

# ⚙️ List all indexers
curl -H "api-key: $admin_key" "$url/indexers?api-version=2020-06-30"
```

## 🧪 Testing Your Search Solution

### 🔍 Basic Search Test

```bash
# 🎯 Search all documents
curl -H "api-key: $admin_key" \
     "$url/indexes/margies-custom-index/docs?api-version=2020-06-30&search=*"
```

### 📊 Test Enriched Fields

```bash
# 🧠 Query with AI-enriched fields
curl -H "api-key: $admin_key" \
     "$url/indexes/margies-custom-index/docs?api-version=2020-06-30&search=hotel&select=content,sentiment,keyphrases"
```

## ❌ Troubleshooting

### 🔧 Common Issues and Solutions

1. **🔑 Authentication Errors**
   ```bash
   # Verify your admin key
   echo "Admin Key: $admin_key"
   # Check search service URL
   echo "Search URL: $url"
   ```

2. **📋 JSON Format Errors**
   ```bash
   # Validate JSON files
   cat data_source.json | jq .
   cat skillset.json | jq .
   cat index.json | jq .
   cat indexer.json | jq .
   ```

3. **🔗 Connection String Issues**
   ```bash
   # Test storage connection
   az storage container list --connection-string "YOUR_CONNECTION_STRING"
   ```

4. **🧠 Cognitive Services Key Problems**
   ```bash
   # Verify AI Services key in skillset.json
   grep "YOUR_AI_SERVICES_KEY" skillset.json
   ```

### 📊 Debug Commands

```bash
# 🔍 Get detailed error information
curl -v -H "api-key: $admin_key" \
     "$url/indexers/margies-custom-indexer/status?api-version=2020-06-30"

# 📋 Check resource creation order
echo "1. Data Source created"
echo "2. Skillset created"  
echo "3. Index created"
echo "4. Indexer created and running"
```

## ✅ Success Indicators

Your search solution is successfully created when:

- ✅ **Data Source**: Returns 201 Created status
- ✅ **Skillset**: Returns 201 Created status  
- ✅ **Index**: Returns 201 Created status
- ✅ **Indexer**: Returns 201 Created and starts processing
- ✅ **Documents**: Begin appearing in search results
- ✅ **Enrichments**: AI-generated fields contain data

## 🚀 Next Steps

After successful creation:

1. 🔍 **Test Search Queries**: Verify document indexing
2. 📊 **Review Enrichments**: Check AI-generated content
3. 🔄 **Monitor Indexer**: Watch processing progress
4. 📈 **Optimize Performance**: Tune batch sizes and parameters
5. 🚀 **Proceed to Update**: Enhance with additional features

## 📚 Related Resources

- 🛠️ [Azure AI Search REST API](https://docs.microsoft.com/en-us/rest/api/searchservice/)
- 🧠 [Cognitive Skills Documentation](https://docs.microsoft.com/en-us/azure/search/cognitive-search-predefined-skills)
- 📊 [Index Design Guidelines](https://docs.microsoft.com/en-us/azure/search/search-what-is-an-index)
- ⚙️ [Indexer Configuration](https://docs.microsoft.com/en-us/azure/search/search-indexer-overview)

---

🎯 **Pro Tip**: Always verify each step completes successfully before proceeding to the next. Monitor the Azure portal for real-time status updates!
