# 🧠 Knowledge Store Lab - Azure Cognitive Search

Welcome to the Knowledge Store lab! This lab demonstrates how to create a knowledge store using Azure Cognitive Search to extract and store enriched data from documents.

## 📋 Overview

This lab shows you how to:
- 🏗️ Set up Azure resources (Storage Account, Cognitive Services, Search Service)
- 📄 Upload sample documents (PDF reviews)
- 🤖 Create AI-powered search components with knowledge store capabilities
- 🔍 Extract structured data from unstructured documents

## 📁 Folder Structure

```
03-knowledge-store/
├── 📄 setup.sh              # Initial Azure resource setup script
├── 📄 setup.cmd             # Windows version of setup script
├── 📂 data/                 # Sample documents to be indexed
│   ├── reviews/             # PDF review documents (201801.pdf - 201866.pdf)
│   └── collateral/          # Additional document materials
└── 📂 create-search/        # Search service configuration scripts
    ├── 🚀 create-search.sh  # Main search creation script
    ├── 🚀 create-search.cmd # Windows version
    ├── 📋 data_source.json  # Data source configuration
    ├── 📋 index.json        # Search index schema
    ├── 📋 indexer.json      # Indexer configuration
    └── 📋 skillset.json     # AI skillset definitions
```

## 🚀 Getting Started

### Step 1: Setup Azure Resources

Run the setup script to create all necessary Azure resources:

```bash
# For Linux/macOS
./setup.sh

# For Windows
setup.cmd
```

**What this script does:**
- 🏪 Creates a unique Azure Storage Account (`ai102str{unique_id}`)
- 📤 Uploads PDF documents to a blob container named 'margies'
- 🧠 Creates an Azure Cognitive Services account (`ai102cog{unique_id}`)
- 🔍 Creates an Azure Search Service (`ai102srch{unique_id}`)
- 🔑 Displays connection strings and API keys for next steps

### Step 2: Create Search Components

After running setup, navigate to the `create-search/` folder and run the search creation script:

```bash
cd create-search/
./create-search.sh
```

## 📊 Sample Data

The lab includes **66 PDF review documents** (201801.pdf through 201866.pdf) located in the `data/reviews/` folder. These documents simulate customer reviews and feedback that will be processed by the AI pipeline.

## 🛠️ Technical Components

### Data Source
- **Type**: Azure Blob Storage
- **Container**: `margies`
- **Content**: PDF documents containing review text

### AI Skillset
The skillset processes documents through various AI capabilities:
- 📝 Text extraction from PDFs
- 🎯 Key phrase extraction
- 😊 Sentiment analysis
- 🏷️ Entity recognition
- 💾 Knowledge store projections

### Search Index
Creates a searchable index with enriched fields extracted by the AI pipeline.

### Knowledge Store
Stores the enriched data in structured format for further analysis and reporting.

## 🔧 Configuration Files

| File | Purpose | Description |
|------|---------|-------------|
| `data_source.json` | 📡 Data Connection | Connects to Azure Blob Storage |
| `skillset.json` | 🤖 AI Processing | Defines cognitive skills and knowledge store |
| `index.json` | 🗃️ Search Schema | Defines searchable fields structure |
| `indexer.json` | ⚙️ Processing Pipeline | Orchestrates data flow and processing |

## 🎯 Learning Objectives

By completing this lab, you will learn:
- ✅ How to set up Azure Cognitive Search with knowledge store
- ✅ How to configure AI skills for document processing
- ✅ How to extract structured data from unstructured documents
- ✅ How to create projections for knowledge mining scenarios

## 🔗 Next Steps

After completing this lab:
1. 🌐 Explore the Azure Portal to see your created resources
2. 🔍 Test search queries against your index
3. 📊 Examine the knowledge store projections
4. 🧪 Experiment with different skillset configurations

## 📚 Additional Resources

- [Azure Cognitive Search Documentation](https://docs.microsoft.com/azure/search/)
- [Knowledge Store Overview](https://docs.microsoft.com/azure/search/knowledge-store-concept-intro)
- [AI Enrichment in Azure Cognitive Search](https://docs.microsoft.com/azure/search/cognitive-search-concept-intro)

---
🎓 **Happy Learning!** This lab is part of the Microsoft Learn Knowledge Mining learning path.
