# 🔄 Update Search Solution

This folder contains scripts and configuration files to enhance and update your existing Azure AI Search solution with additional capabilities and improved processing.

## 📋 Overview

The update-search workflow enhances your existing search solution by:

1. 🧠 **Enhancing Skillsets**: Adding new cognitive skills and improving existing ones
2. 🗂️ **Expanding Index Schema**: Adding new fields for enhanced search capabilities
3. ⚙️ **Updating Indexer Configuration**: Improving field mappings and processing parameters
4. 🔄 **Reprocessing Documents**: Applying new enrichments to existing content

## 📁 Files Structure

```
update-search/
├── 📄 README.md             # This documentation
├── 🔧 update-search.cmd     # Windows batch script
├── 🔧 update-search.sh      # 🐧 Linux/macOS bash script
├── 📋 update-skillset.json  # Enhanced skillset configuration
├── 📋 update-index.json     # Expanded index schema
└── 📋 update-indexer.json   # Updated indexer configuration
```

## 🛠️ Prerequisites

Before running the update scripts, ensure you have:

- ✅ **Existing Search Solution** from create-search step
- ✅ **Azure AI Search Service** with admin access
- ✅ **Azure AI Services** resource key
- ✅ **Admin API Key** for your search service
- ✅ **Successful Initial Indexing** completed

## 🚀 Quick Start

### 1️⃣ Configure Update Environment

Edit the script with your Azure AI Search details:

```bash
# 📝 Edit the bash script
nano update-search.sh

# Set your specific values:
url="https://your-search-service.search.windows.net"
admin_key="YOUR_ADMIN_KEY_HERE"
```

### 2️⃣ Make Script Executable

```bash
# 🔧 Grant execution permissions
chmod +x update-search.sh
```

### 3️⃣ Run the Update Script

```bash
# 🔄 Execute the update workflow
./update-search.sh
```

## 🔧 Shell Script Breakdown

### 📜 update-search.sh Workflow

```bash
#!/bin/bash

# 🔑 Configuration Section
url="YOUR_SEARCH_URL"                    # Your search service endpoint
admin_key="YOUR_ADMIN_KEY"               # Administrative API key

# 🔄 Step 1: Update Skillset
echo "Updating the skillset..."
curl -X PUT "${url}/skillsets/margies-custom-skillset?api-version=2020-06-30" \
     -H "Content-Type: application/json" \
     -H "api-key: ${admin_key}" \
     -d @update-skillset.json
```

#### 🧠 What this does:
- 🔄 **HTTP PUT Request**: Updates existing skillset
- 📈 **Enhanced Skills**: Adds new cognitive capabilities
- 🎯 **Improved Processing**: Better AI enrichment configurations
- 🔧 **Optimization**: Performance and accuracy improvements

```bash
# 🔄 Step 2: Update Index Schema
echo "Updating the index..."
curl -X PUT "${url}/indexes/margies-custom-index?api-version=2020-06-30" \
     -H "Content-Type: application/json" \
     -H "api-key: ${admin_key}" \
     -d @update-index.json
```

#### 🗂️ What this does:
- 📊 **Schema Enhancement**: Adds new searchable fields
- 🔍 **Field Properties**: Updates searchable, filterable options
- 📈 **New Capabilities**: Enables advanced search features
- 🎯 **Optimization**: Improves query performance

```bash
# ⏱️ Wait for schema updates to propagate
sleep 3

# 🔄 Step 3: Update Indexer Configuration
echo "Updating the indexer..."
curl -X PUT "${url}/indexers/margies-custom-indexer?api-version=2020-06-30" \
     -H "Content-Type: application/json" \
     -H "api-key: ${admin_key}" \
     -d @update-indexer.json
```

#### ⚙️ What this does:
- 🔄 **Field Mapping Updates**: New source-to-index mappings
- 🧠 **Output Mapping**: Routes new skill outputs to index fields
- 📊 **Parameter Tuning**: Optimizes batch size and error handling
- 🚀 **Reprocessing**: Triggers document re-indexing with new enrichments

```bash
# 🚀 Step 4: Run Updated Indexer
echo "Running the indexer..."
curl -X POST "${url}/indexers/margies-custom-indexer/run?api-version=2020-06-30" \
     -H "api-key: ${admin_key}"
```

#### 🏃 What this does:
- 🔄 **Manual Execution**: Forces immediate indexer run
- 📊 **Reprocessing**: Applies updates to all documents
- 🧠 **New Enrichments**: Generates updated AI insights
- ✅ **Completion**: Ensures all documents have new capabilities

## 📋 Update Configuration Files

### 🧠 update-skillset.json

The updated skillset includes enhanced capabilities:

```json
{
  "name": "margies-custom-skillset",
  "description": "Enhanced skillset with additional cognitive capabilities",
  "cognitiveServices": {
    "@odata.type": "#Microsoft.Azure.Search.CognitiveServicesByKey",
    "key": "YOUR_AI_SERVICES_KEY"
  },
  "skills": [
    // 📈 Enhanced sentiment analysis
    {
      "@odata.type": "#Microsoft.Skills.Text.V3.SentimentSkill",
      "name": "enhanced-sentiment",
      "context": "/document",
      "defaultLanguageCode": "en",
      "includeOpinionMining": true  // 🆕 New feature
    },
    // 🔑 Improved key phrase extraction
    {
      "@odata.type": "#Microsoft.Skills.Text.KeyPhraseExtractionSkill",
      "name": "enhanced-keyphrases",
      "context": "/document",
      "maxKeyPhraseCount": 10  // 🆕 Increased limit
    },
    // 🌍 Enhanced entity recognition
    {
      "@odata.type": "#Microsoft.Skills.Text.V3.EntityRecognitionSkill",
      "name": "enhanced-entities",
      "context": "/document",
      "categories": ["Person", "Location", "Organization", "DateTime", "Quantity"]
    },
    // 🆕 NEW: Custom text classification
    {
      "@odata.type": "#Microsoft.Skills.Custom.WebApiSkill",
      "name": "custom-classifier",
      "uri": "https://your-function.azurewebsites.net/api/classify",
      "httpHeaders": {},
      "context": "/document",
      "inputs": [...],
      "outputs": [...]
    }
  ]
}
```

**New Enhancements:**
- 📈 **Opinion Mining**: Detailed sentiment analysis with aspects
- 🔑 **More Key Phrases**: Increased extraction limit
- 🌍 **Rich Entity Types**: Expanded entity recognition categories
- 🆕 **Custom Skills**: Custom processing capabilities
- 🎯 **Better Accuracy**: Improved AI model configurations

### 🗂️ update-index.json

The enhanced index schema includes new fields:

```json
{
  "name": "margies-custom-index",
  "fields": [
    // ... existing fields ...
    
    // 🆕 New enriched fields
    {
      "name": "opinion_aspects",
      "type": "Collection(Edm.String)",
      "searchable": true,
      "filterable": true,
      "facetable": true
    },
    {
      "name": "entity_persons",
      "type": "Collection(Edm.String)",
      "searchable": true,
      "filterable": true,
      "facetable": true
    },
    {
      "name": "entity_organizations",
      "type": "Collection(Edm.String)",
      "searchable": true,
      "filterable": true,
      "facetable": true
    },
    {
      "name": "custom_categories",
      "type": "Collection(Edm.String)",
      "searchable": true,
      "filterable": true,
      "facetable": true
    },
    {
      "name": "confidence_score",
      "type": "Edm.Double",
      "searchable": false,
      "filterable": true,
      "sortable": true
    }
  ]
}
```

**New Field Capabilities:**
- 📊 **Opinion Aspects**: Detailed sentiment components
- 👥 **Person Entities**: People mentioned in documents
- 🏢 **Organization Entities**: Companies and institutions
- 🏷️ **Custom Categories**: AI-generated document classifications
- 📈 **Confidence Scores**: AI prediction confidence levels

### ⚙️ update-indexer.json

Enhanced indexer configuration:

```json
{
  "name": "margies-custom-indexer",
  "dataSourceName": "margies-data-source",
  "targetIndexName": "margies-custom-index",
  "skillsetName": "margies-custom-skillset",
  "fieldMappings": [
    // ... existing mappings ...
  ],
  "outputFieldMappings": [
    // ... existing mappings ...
    
    // 🆕 New skill output mappings
    {
      "sourceFieldName": "/document/enhanced-sentiment/opinions/*/aspect",
      "targetFieldName": "opinion_aspects"
    },
    {
      "sourceFieldName": "/document/enhanced-entities/persons",
      "targetFieldName": "entity_persons"
    },
    {
      "sourceFieldName": "/document/enhanced-entities/organizations", 
      "targetFieldName": "entity_organizations"
    },
    {
      "sourceFieldName": "/document/custom-classifier/categories",
      "targetFieldName": "custom_categories"
    },
    {
      "sourceFieldName": "/document/custom-classifier/confidence",
      "targetFieldName": "confidence_score"
    }
  ],
  "parameters": {
    "batchSize": 5,        // 📈 Increased batch size
    "maxFailedItems": 10,  // 🔧 Improved error tolerance
    "maxFailedItemsPerBatch": 2
  }
}
```

**Improvements:**
- 📊 **New Mappings**: Routes enhanced AI outputs to index
- 📈 **Better Performance**: Optimized batch processing
- 🔧 **Error Handling**: Improved failure tolerance
- 🎯 **Reliability**: Better processing parameters

## 📊 Update Execution Workflow

```
1. 🧠 Update Skillset (Enhanced AI capabilities)
   ↓
2. 🗂️ Update Index Schema (New searchable fields)
   ↓  
3. ⏱️ Wait (3 seconds for propagation)
   ↓
4. ⚙️ Update Indexer (New field mappings)
   ↓
5. 🚀 Run Indexer (Reprocess all documents)
   ↓
6. 📈 Monitor Progress (Track completion)
```

## 🔍 Monitoring Update Progress

### 📊 Check Update Status

```bash
# 📈 Monitor indexer execution
curl -H "api-key: $admin_key" \
     "$url/indexers/margies-custom-indexer/status?api-version=2020-06-30"
```

### 🔍 Verify New Capabilities

```bash
# 🧠 Test enhanced sentiment analysis
curl -H "api-key: $admin_key" \
     "$url/indexes/margies-custom-index/docs?api-version=2020-06-30&search=*&select=content,opinion_aspects"

# 👥 Test entity extraction
curl -H "api-key: $admin_key" \
     "$url/indexes/margies-custom-index/docs?api-version=2020-06-30&search=*&select=entity_persons,entity_organizations"

# 🏷️ Test custom categories
curl -H "api-key: $admin_key" \
     "$url/indexes/margies-custom-index/docs?api-version=2020-06-30&search=*&select=custom_categories,confidence_score"
```

### 📈 Performance Monitoring

```bash
# ⏱️ Check indexer performance
echo "Monitoring indexer progress..."
while true; do
    status=$(curl -s -H "api-key: $admin_key" "$url/indexers/margies-custom-indexer/status?api-version=2020-06-30" | jq -r '.lastResult.status')
    echo "Status: $status"
    if [ "$status" = "success" ]; then
        echo "✅ Update completed successfully!"
        break
    elif [ "$status" = "transientFailure" ] || [ "$status" = "persistentFailure" ]; then
        echo "❌ Update failed with status: $status"
        break
    fi
    sleep 10
done
```

## 🧪 Testing Enhanced Features

### 🔍 Enhanced Search Queries

```bash
# 📊 Test opinion mining
curl -H "api-key: $admin_key" \
     "$url/indexes/margies-custom-index/docs/search?api-version=2020-06-30" \
     -d '{"search": "hotel", "select": "content,opinion_aspects", "top": 5}'

# 👥 Search by person entities
curl -H "api-key: $admin_key" \
     "$url/indexes/margies-custom-index/docs/search?api-version=2020-06-30" \
     -d '{"search": "*", "filter": "entity_persons/any(p: p eq 'John Smith')"}'

# 🏷️ Filter by custom categories
curl -H "api-key: $admin_key" \
     "$url/indexes/margies-custom-index/docs/search?api-version=2020-06-30" \
     -d '{"search": "*", "filter": "custom_categories/any(c: c eq 'travel-review')"}'
```

### 📊 Faceted Search Examples

```bash
# 🎯 Get facets for new fields
curl -H "api-key: $admin_key" \
     "$url/indexes/margies-custom-index/docs/search?api-version=2020-06-30" \
     -d '{
       "search": "*",
       "facets": [
         "entity_persons,count:10",
         "entity_organizations,count:10", 
         "custom_categories,count:10"
       ]
     }'
```

## ❌ Troubleshooting Updates

### 🔧 Common Update Issues

1. **🔄 Schema Update Conflicts**
   ```bash
   # Check for index lock status
   curl -H "api-key: $admin_key" "$url/indexes/margies-custom-index?api-version=2020-06-30"
   ```

2. **🧠 Skillset Update Failures**
   ```bash
   # Validate skillset JSON
   cat update-skillset.json | jq .
   # Check AI Services key
   grep "YOUR_AI_SERVICES_KEY" update-skillset.json
   ```

3. **⚙️ Indexer Configuration Errors**
   ```bash
   # Verify field mapping paths
   cat update-indexer.json | jq '.outputFieldMappings'
   ```

4. **📊 Processing Failures**
   ```bash
   # Get detailed error information
   curl -H "api-key: $admin_key" \
        "$url/indexers/margies-custom-indexer/status?api-version=2020-06-30" | jq '.lastResult.errors'
   ```

### 🛠️ Debug Commands

```bash
# 🔍 Compare before and after
echo "=== BEFORE UPDATE ==="
curl -H "api-key: $admin_key" "$url/indexes/margies-custom-index?api-version=2020-06-30" | jq '.fields | length'

echo "=== AFTER UPDATE ==="
curl -H "api-key: $admin_key" "$url/indexes/margies-custom-index?api-version=2020-06-30" | jq '.fields | length'

# 📊 Check document count
curl -H "api-key: $admin_key" \
     "$url/indexes/margies-custom-index/docs/\$count?api-version=2020-06-30"
```

## ✅ Update Success Indicators

Your search solution is successfully updated when:

- ✅ **Skillset Updated**: New cognitive skills active
- ✅ **Index Expanded**: New fields available for search
- ✅ **Indexer Updated**: New field mappings configured
- ✅ **Reprocessing Complete**: All documents have new enrichments
- ✅ **New Features**: Enhanced search capabilities working
- ✅ **Performance**: Improved processing and accuracy

## 🎯 Advanced Update Features

### 🆕 What's New in This Update

1. **📈 Opinion Mining**
   - Detailed sentiment analysis with specific aspects
   - Identifies what users like/dislike about specific features

2. **👥 Enhanced Entity Recognition**
   - Extracts people, organizations, locations
   - Better accuracy with expanded categories

3. **🏷️ Custom Classification**
   - AI-powered document categorization
   - Confidence scores for predictions

4. **📊 Performance Improvements**
   - Larger batch sizes for faster processing
   - Better error handling and recovery

5. **🔍 Advanced Search Capabilities**
   - Faceted navigation on new fields
   - Complex filtering and sorting options

## 🚀 Next Steps After Update

1. 🧪 **Test New Features**: Explore enhanced search capabilities
2. 📊 **Monitor Performance**: Track processing improvements
3. 🎯 **Optimize Queries**: Leverage new fields for better results
4. 📈 **Analyze Insights**: Review AI-generated content quality
5. 🔄 **Iterate**: Continue refining based on user feedback

## 📚 Related Resources

- 🧠 [Cognitive Skills V3 Features](https://docs.microsoft.com/en-us/azure/search/cognitive-search-skill-sentiment-v3)
- 👥 [Entity Recognition](https://docs.microsoft.com/en-us/azure/search/cognitive-search-skill-entity-recognition-v3)
- 🏷️ [Custom Skills Development](https://docs.microsoft.com/en-us/azure/search/cognitive-search-custom-skill-interface)
- 📊 [Index Update Guidelines](https://docs.microsoft.com/en-us/azure/search/search-howto-reindex)

---

🎉 **Congratulations!** Your search solution now includes advanced AI capabilities and enhanced search features! 🚀
