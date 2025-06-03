# 🔍 Create Search Components - Knowledge Store Setup

This folder contains the scripts and configuration files needed to create and configure Azure Cognitive Search components with knowledge store capabilities.

## 📋 Overview

These scripts set up a complete AI-powered search pipeline that:
- 🔗 Connects to your Azure Blob Storage data source
- 🤖 Applies AI skills to extract insights from documents
- 🗃️ Creates a searchable index with enriched fields
- 💾 Builds a knowledge store for structured data mining
- ⚙️ Orchestrates the entire process with an indexer

## 📁 Files in This Folder

| File | 📝 Description | Purpose |
|------|---------------|---------|
| `🚀 create-search.sh` | Linux/macOS script | Main automation script for search setup |
| `🚀 create-search.cmd` | Windows batch file | Windows version of the main script |
| `📋 data_source.json` | Data source config | Defines connection to Azure Blob Storage |
| `📋 skillset.json` | AI skillset config | Configures cognitive skills and knowledge store |
| `📋 index.json` | Search index schema | Defines the structure of searchable fields |
| `📋 indexer.json` | Indexer configuration | Orchestrates data processing pipeline |

## 🚀 How to Use

### Prerequisites ✅
Before running these scripts, ensure you have:
- ✅ Completed the main setup (run `../setup.sh` from parent folder)
- ✅ Azure Storage Account with uploaded documents
- ✅ Azure Cognitive Services account
- ✅ Azure Search Service created
- ✅ Connection strings and API keys from setup

### Step 1: Configure Your Credentials 🔑

Edit the script files to add your Azure service details:

**For Linux/macOS (`create-search.sh`):**
```bash
# Replace these values with your actual service details
url="https://your-search-service.search.windows.net"
admin_key="your-admin-api-key"
```

**For Windows (`create-search.cmd`):**
```batch
rem Replace these values with your actual service details
set url=https://your-search-service.search.windows.net
set admin_key=your-admin-api-key
```

### Step 2: Update Configuration Files 📝

Update the JSON configuration files with your specific details:

1. **`data_source.json`** - Add your storage connection string:
   ```json
   "connectionString": "DefaultEndpointsProtocol=https;AccountName=your-storage;AccountKey=your-key;EndpointSuffix=core.windows.net"
   ```

2. **`skillset.json`** - Add your Cognitive Services key
3. **Other files** - Usually work with default settings

### Step 3: Run the Script 🏃‍♂️

**For Linux/macOS:**
```bash
chmod +x create-search.sh
./create-search.sh
```

**For Windows:**
```cmd
create-search.cmd
```

## 🔧 What Each Script Step Does

### 1. 📡 Creating Data Source
```bash
echo "Creating the data source..."
curl -X POST "${url}/datasources?api-version=2020-06-30" \
     -H "Content-Type: application/json" \
     -H "api-key: ${admin_key}" \
     -d @data_source.json
```
**Purpose:** Establishes connection to your Azure Blob Storage container (`margies`) containing the PDF documents.

### 2. 🤖 Creating AI Skillset
```bash
echo "Creating the skillset..."
curl -X PUT "${url}/skillsets/margies-knowledge-skillset?api-version=2020-06-30" \
     -H "Content-Type: application/json" \
     -H "api-key: ${admin_key}" \
     -d @skillset.json
```
**Purpose:** Sets up AI cognitive skills including:
- 📄 Text extraction from PDFs
- 🎯 Key phrase extraction
- 😊 Sentiment analysis
- 🏷️ Entity recognition
- 💾 Knowledge store projections

### 3. 🗃️ Creating Search Index
```bash
echo "Creating the index..."
curl -X PUT "${url}/indexes/margies-knowledge-index?api-version=2020-06-30" \
     -H "Content-Type: application/json" \
     -H "api-key: ${admin_key}" \
     -d @index.json
```
**Purpose:** Defines the searchable schema with fields for both original content and AI-enriched data.

### 4. ⚙️ Creating Indexer
```bash
sleep 3  # Wait for index creation to complete
echo "Creating the indexer..."
curl -X PUT "${url}/indexers/margies-knowledge-indexer?api-version=2020-06-30" \
     -H "Content-Type: application/json" \
     -H "api-key: ${admin_key}" \
     -d @indexer.json
```
**Purpose:** Creates the orchestrator that:
- 📥 Pulls data from the data source
- 🔄 Applies the skillset transformations
- 📤 Populates the search index
- 💾 Builds the knowledge store

## 🎯 Expected Results

After successful execution, you'll have:
- ✅ **Data Source**: `margies-knowledge-data` connected to your blob storage
- ✅ **Skillset**: `margies-knowledge-skillset` with AI enrichment capabilities
- ✅ **Index**: `margies-knowledge-index` ready for search queries
- ✅ **Indexer**: `margies-knowledge-indexer` processing your documents
- ✅ **Knowledge Store**: Structured projections for data mining

## 🔍 Monitoring Progress

After running the script:

1. **Check indexer status:**
   ```bash
   curl -X GET "${url}/indexers/margies-knowledge-indexer/status?api-version=2020-06-30" \
        -H "api-key: ${admin_key}"
   ```

2. **View in Azure Portal:**
   - Navigate to your Azure Search service
   - Check the "Indexers" tab for processing status
   - Monitor document count in the "Indexes" tab

## 🚨 Troubleshooting

### Common Issues:

| ❌ Problem | 💡 Solution |
|-----------|------------|
| `Unauthorized` error | ✅ Check your admin API key is correct |
| `Data source not found` | ✅ Verify storage connection string in `data_source.json` |
| `Skillset creation failed` | ✅ Ensure Cognitive Services key is configured |
| `Indexer stuck` | ✅ Check blob container permissions and document formats |

### Debug Commands:
```bash
# Check if data source exists
curl -X GET "${url}/datasources/margies-knowledge-data?api-version=2020-06-30" \
     -H "api-key: ${admin_key}"

# View skillset definition
curl -X GET "${url}/skillsets/margies-knowledge-skillset?api-version=2020-06-30" \
     -H "api-key: ${admin_key}"
```

## 📊 Knowledge Store Output

The knowledge store creates structured projections of your enriched data:
- 📑 **Documents table**: Basic document information
- 🔤 **Key phrases table**: Extracted key phrases with relationships
- 📍 **Entities table**: Named entities found in documents
- 😊 **Sentiment table**: Sentiment analysis results

## 🔗 Next Steps

After successful creation:
1. 🔍 Test search queries against your index
2. 📊 Explore knowledge store projections in Azure Storage
3. 🧪 Experiment with different search parameters
4. 📈 Analyze the enriched data for insights

## 📚 API References

- [Azure Cognitive Search REST API](https://docs.microsoft.com/rest/api/searchservice/)
- [Data Sources API](https://docs.microsoft.com/rest/api/searchservice/create-data-source)
- [Skillsets API](https://docs.microsoft.com/rest/api/searchservice/create-skillset)
- [Indexes API](https://docs.microsoft.com/rest/api/searchservice/create-index)
- [Indexers API](https://docs.microsoft.com/rest/api/searchservice/create-indexer)

---
🎓 **Happy Searching!** Your AI-powered knowledge store is ready to unlock insights from your documents!
