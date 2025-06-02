# Margie's Travel - Flask Search Application

This is a Flask web application that provides a search interface for Margie's Travel documents using Azure AI Search.

## Application Overview

Margie's Travel Search is a Python Flask web application that demonstrates how to build a search interface using Azure AI Search. The application allows users to search through travel brochures and hotel reviews with advanced filtering and sorting capabilities.

## Features

- **Full-text search** across travel documents
- **AI-powered enrichments** including sentiment analysis, key phrases, and location extraction
- **Faceted filtering** by document author
- **Multiple sorting options** (relevance, filename, size, date, sentiment)
- **Search result highlighting** to show matched terms in context
- **Responsive web interface** built with Bootstrap

## Prerequisites

- Python 3.7 or higher
- Azure AI Search service with indexed documents
- Azure AI Services resource (for cognitive skills)

## Installation & Setup

### 1. Install Dependencies

```bash
pip install -r requirements.txt
```

### 2. Configure Environment Variables

Edit the `.env` file with your Azure AI Search credentials:

```env
SEARCH_SERVICE_ENDPOINT=https://your-search-service.search.windows.net
SEARCH_SERVICE_QUERY_KEY=your-query-key-here
SEARCH_INDEX_NAME=margies-index
```

**Important Notes:**
- Use the **query key**, not the admin key
- The endpoint should include `https://` and `.search.windows.net`
- The index name should match exactly (case-sensitive)

### 3. Getting Azure Credentials

1. Go to your Azure AI Search resource in the Azure Portal
2. **Endpoint**: Copy the URL from the Overview page
3. **Query Key**: Go to Settings > Keys and copy the Query key

## Running the Application

### Start the Flask Development Server

```bash
flask run
```

Or alternatively:

```bash
python app.py
```

The application will be available at:
- **Local URL**: `http://localhost:5000`
- **Network URL**: `http://127.0.0.1:5000`

## Application Structure

### Backend Components

#### `app.py` - Main Application File

The Flask application includes:

```python
# Core search function
def search_query(search_text, filter_by=None, sort_order=None):
    # Creates Azure Search client
    # Submits search with advanced parameters
    # Returns enriched results

# Routes
@app.route("/")           # Home page
@app.route("/search")     # Search results with filtering/sorting
```

#### Key Search Parameters

The application uses these Azure AI Search features:

- **Search Mode**: "all" (requires all terms to be found)
- **Facets**: Author filtering (`metadata_author`)
- **Highlighting**: Shows search terms in context
- **Field Selection**: Returns specific enriched fields
- **Sorting**: Multiple sort options available

### Frontend Components

#### Templates

- **`templates/base.html`**: Common layout and navigation
- **`templates/default.html`**: Home page with search form
- **`templates/search.html`**: Search results with filters and sorting
- **`templates/error.html`**: Error handling page

#### Static Assets

- **`static/css/`**: Stylesheet files for UI styling
- **`static/images/`**: Application images and icons

## Search Capabilities

### Searchable Fields

The application searches across these enriched fields:

- **Document content** (merged_content)
- **Metadata** (filename, author, size, modification date)
- **AI-extracted data**:
  - Key phrases
  - Locations
  - Image captions and tags
  - Sentiment analysis
  - Language detection

### Filtering Options

- **By Author**: Filter results by document author (uses facets)
- **Advanced filters** can be added by modifying the search query

### Sorting Options

- **Relevance** (default): Based on search score
- **Filename**: Alphabetical by document name
- **Size**: By file size (largest first)
- **Date**: By last modified date (newest first)
- **Sentiment**: By sentiment score (positive to negative)

## Usage Examples

### Basic Searches

1. **Simple terms**: "hotel", "London", "travel"
2. **Phrases**: "luxury hotel", "city center"
3. **Locations**: "New York", "Paris", "Tokyo"

### Advanced Search Examples

1. **Location-based**: "quiet hotel in New York"
2. **Landmark searches**: "Tower of London", "Eiffel Tower"
3. **Activity searches**: "museum", "restaurant", "shopping"
4. **Image content**: "skyscraper", "beach", "mountain"

### Filter and Sort Examples

1. Search for "London" → Filter by "Reviewer" → Sort by "Positive to negative"
2. Search for "hotel" → Sort by "Date" to see newest reviews first
3. Search for "*" (all documents) → Filter by specific author

## Enriched Data Display

Each search result shows:

- **File information**: Name, size, author, last modified date
- **Content preview**: Highlighted excerpts showing search terms
- **AI insights**:
  - Sentiment label (positive, negative, neutral, mixed)
  - Key phrases extracted from content
  - Locations mentioned in the document
  - Image tags (for documents with images)
- **Direct link**: Click filename to access original document

## Troubleshooting

### Common Issues

1. **"No module named 'azure'"**
   - Run: `pip install azure-search-documents`

2. **Connection timeout/errors**
   - Verify the endpoint URL is correct
   - Check that the query key is valid
   - Ensure the Azure AI Search service is running

3. **No search results**
   - Verify the index name matches exactly
   - Check if the indexer has completed successfully
   - Test queries directly in Azure Portal first

4. **Template not found errors**
   - Ensure all template files are in the `templates/` folder
   - Check file names match exactly (case-sensitive)

### Debug Mode

To enable Flask debug mode for development:

```python
if __name__ == '__main__':
    app.run(debug=True)
```

## Customization

### Adding New Search Features

1. **New filter fields**: Modify the facets parameter in `search_query()`
2. **Additional sort options**: Add new cases to the sort logic
3. **Custom highlighting**: Modify the highlight_fields parameter
4. **Field selection**: Update the select parameter to include/exclude fields

### UI Customization

1. **Styling**: Edit CSS files in `static/css/`
2. **Layout**: Modify HTML templates in `templates/`
3. **Branding**: Update images in `static/images/`

### Performance Optimization

1. **Result pagination**: Add paging for large result sets
2. **Field selection**: Limit returned fields to improve performance
3. **Caching**: Implement result caching for common queries
4. **Indexing**: Optimize index configuration for your search patterns

## Production Deployment

### Environment Setup

1. Set environment variables properly
2. Use production WSGI server (e.g., Gunicorn)
3. Configure proper logging
4. Set up SSL/HTTPS
5. Implement authentication if required

### Security Considerations

- Never expose admin keys in client applications
- Use environment variables for all secrets
- Implement proper error handling
- Consider rate limiting for production use

## Related Resources

- [Azure AI Search Python SDK Documentation](https://docs.microsoft.com/en-us/python/api/overview/azure/search-documents-readme)
- [Flask Documentation](https://flask.palletsprojects.com/)
- [Azure AI Search REST API Reference](https://docs.microsoft.com/en-us/rest/api/searchservice/)
- [Azure AI Search Query Syntax](https://docs.microsoft.com/en-us/azure/search/query-lucene-syntax)
