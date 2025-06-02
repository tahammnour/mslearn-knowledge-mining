# Azure AI Search Solution - Lab 01

This lab demonstrates how to create an Azure AI Search solution using Margie's Travel as an example scenario. The solution implements intelligent document search with AI-powered enrichments.

## Overview

Margie's Travel is a travel agency that has accumulated a large collection of documents including brochures and customer hotel reviews. This lab shows how to implement an Azure AI Search solution to make this information easily searchable and discoverable.

## Folder Structure

```
01-azure-search/
├── README.md                 # This file
├── UploadDocs.cmd           # Windows batch script to upload documents
├── UploadDocs.ps1           # PowerShell script to upload documents  
├── UploadDocs.sh            # Bash script to upload documents
├── C-Sharp/                 # C# implementation (not covered in this guide)
├── Python/                  # Python implementation (main focus)
│   ├── README.md           # Python-specific documentation
│   └── margies-travel/     # Flask web application
├── data/                    # Sample documents for indexing
│   ├── collateral/         # Marketing brochures
│   └── reviews/            # Customer hotel reviews
└── modify-search/          # Advanced search configuration scripts
    ├── skillset.json       # AI skillset definition
    ├── index.json          # Search index schema
    └── indexer.json        # Indexer configuration
```

## Prerequisites

Before starting this lab, you need:

- An active Azure subscription
- Visual Studio Code installed
- Azure CLI installed and configured
- Python 3.7+ (for Python implementation)

## Required Azure Resources

This solution requires three Azure resources in the same location:

1. **Azure AI Search** - Manages indexing and querying
2. **Azure AI Services** - Provides cognitive skills for content enrichment
3. **Azure Storage Account** - Stores documents to be indexed

## Setup Instructions

### 1. Create Azure Resources

Create the following resources in the Azure Portal:

#### Azure AI Search Resource
- Service name: Enter a unique name
- Location: Select your preferred location
- Pricing tier: Basic

#### Azure AI Services Resource  
- Name: Enter a unique name
- Region: Same as Azure AI Search
- Pricing tier: Standard S0

#### Storage Account
- Storage account name: Enter a unique name
- Region: Any available region
- Performance: Standard
- Redundancy: Locally-redundant storage (LRS)
- Enable anonymous access on individual containers

### 2. Upload Documents to Storage

1. Edit the upload script with your Azure credentials:
   - `UploadDocs.sh` for Linux/Mac
   - `UploadDocs.cmd` for Windows
   - `UploadDocs.ps1` for PowerShell

2. Replace placeholders with actual values:
   - `YOUR_SUBSCRIPTION_ID`
   - `YOUR_AZURE_STORAGE_ACCOUNT_NAME` 
   - `YOUR_AZURE_STORAGE_KEY`

3. Run the script to create a container and upload documents

### 3. Create Search Index

Using the Azure Portal:

1. Navigate to your Azure AI Search resource
2. Select "Import data"
3. Configure data source (Azure Blob Storage)
4. Set up cognitive skills for content enrichment
5. Customize the target index
6. Create and run the indexer

## AI Enrichments

The solution includes several cognitive skills:

- **OCR**: Extract text from images
- **Location Extraction**: Identify geographical locations
- **Key Phrase Extraction**: Extract important phrases
- **Language Detection**: Detect document language
- **Image Analysis**: Generate tags and captions for images
- **Sentiment Analysis**: Evaluate document sentiment

## Search Features

The search solution provides:

- **Full-text search** across all document content
- **Faceted navigation** by author and other metadata
- **Sorting options** by relevance, date, size, or sentiment
- **Filtering** by various criteria
- **Highlighted search terms** in results
- **Rich metadata** including file info and AI insights

## Advanced Configuration

The `modify-search/` folder contains JSON definitions for:

- **skillset.json**: Defines AI skills and processing pipeline
- **index.json**: Defines search index schema and field properties
- **indexer.json**: Maps source data to index fields

These can be used with the Azure AI Search REST API for programmatic control.

## Testing Searches

Try these example searches:

- `*` - Return all documents
- `London hotel` - Find hotel-related content mentioning London
- `New York` - Documents mentioning New York
- `Tower of London` - Specific landmark references
- `skyscraper` - Content with building-related imagery

## Query Examples

Basic query:
```json
{
  "search": "*",
  "count": true
}
```

Filtered search:
```json
{
  "search": "London",
  "select": "url,sentiment,keyphrases",
  "filter": "metadata_author eq 'Reviewer' and sentiment eq 'positive'"
}
```

## Next Steps

- Explore the Python web application in the `Python/margies-travel/` folder
- Customize the search experience by modifying templates
- Add additional cognitive skills for enhanced content processing
- Implement custom scoring profiles for relevance tuning

## Clean-up

After completing the lab:
1. Delete the resource group containing all created resources
2. This will remove the Azure AI Search, AI Services, and Storage Account

## Additional Resources

- [Azure AI Search Documentation](https://docs.microsoft.com/en-us/azure/search/)
- [Cognitive Skills Reference](https://docs.microsoft.com/en-us/azure/search/cognitive-search-predefined-skills)
- [Azure AI Search REST API](https://docs.microsoft.com/en-us/rest/api/searchservice/)
