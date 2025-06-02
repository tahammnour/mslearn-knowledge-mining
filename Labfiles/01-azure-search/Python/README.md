# Python Implementation - Azure AI Search

This folder contains the Python implementation of the Azure AI Search solution using Flask web framework.

## Overview

The Python implementation provides a web-based interface for searching through Margie's Travel documents using Azure AI Search. It demonstrates how to integrate Azure AI Search SDK with a Python web application.

## Folder Structure

```
Python/
├── README.md              # This file
├── readme.txt            # Basic folder description
└── margies-travel/       # Flask web application
    ├── app.py            # Main Flask application
    ├── requirements.txt  # Python dependencies
    ├── .env             # Environment configuration
    ├── templates/       # HTML templates
    │   ├── base.html    # Base template
    │   ├── default.html # Home page
    │   ├── error.html   # Error page
    │   └── search.html  # Search results page
    └── static/          # Static assets (CSS, images)
        ├── css/
        └── images/
```

## Requirements

### Python Dependencies

The application requires the following Python packages (defined in `requirements.txt`):

```
azure-search-documents==11.5.1
flask
python-dotenv
```

### Installation

1. Navigate to the margies-travel folder:
   ```bash
   cd Python/margies-travel
   ```

2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

## Configuration

### Environment Variables

Configure the application by editing the `.env` file:

```env
SEARCH_SERVICE_ENDPOINT=your_search_endpoint
SEARCH_SERVICE_QUERY_KEY=your_search_query_key
SEARCH_INDEX_NAME=margies-index
```

**Important Configuration Notes:**
- `SEARCH_SERVICE_ENDPOINT`: Your Azure AI Search service URL (e.g., `https://your-service.search.windows.net`)
- `SEARCH_SERVICE_QUERY_KEY`: Use the query key, NOT the admin key
- `SEARCH_INDEX_NAME`: Should be `margies-index` (as created in the lab)

### Getting Azure AI Search Credentials

1. Go to your Azure AI Search resource in the Azure Portal
2. Copy the URL from the Overview page
3. Go to the Keys section
4. Use the Query key (not the Admin key) for the application

## Application Architecture

### Main Components

1. **app.py**: Flask application with search functionality
2. **Templates**: HTML templates for rendering the web interface
3. **Static files**: CSS and images for styling

### Key Features

The application implements:

- **Home page** with search interface
- **Search results** with rich metadata display
- **Faceted filtering** by document author
- **Sorting options** by relevance, filename, size, date, or sentiment
- **Search highlighting** to show matched terms in context
- **Responsive design** for various screen sizes

### Search Functionality

The `search_query()` function in `app.py` handles:

- Creating Azure Search client connection
- Submitting search queries with various parameters
- Processing search results
- Error handling

Key search features:
- Full-text search across all indexed content
- Facet extraction for filtering options
- Field selection for optimized results
- Search highlighting for matched terms

## Running the Application

### Start the Flask Server

```bash
cd Python/margies-travel
flask run
```

The application will start on `http://localhost:5000` or `http://127.0.0.1:5000`

### Using the Web Interface

1. **Home Page**: Enter search terms in the search box
2. **Search Results**: View results with highlighted terms
3. **Filtering**: Use the author filter to narrow results
4. **Sorting**: Choose different sort orders for results
5. **Refine Search**: Modify search terms and filters

## Search Examples

Try these searches in the web application:

- **"London hotel"** - Find hotel information for London
- **"quiet hotel in New York"** - Specific hotel queries
- **"Tower of London"** - Landmark searches
- **"skyscraper"** - Image-based content search
- **"Mojave desert"** - Location-based searches

## Code Structure

### app.py Components

```python
# Import statements
from flask import Flask, request, render_template, redirect, url_for
from azure.core.credentials import AzureKeyCredential
from azure.search.documents import SearchClient

# Configuration loading
load_dotenv()
search_endpoint = os.getenv('SEARCH_SERVICE_ENDPOINT')
search_key = os.getenv('SEARCH_SERVICE_QUERY_KEY')
search_index = os.getenv('SEARCH_INDEX_NAME')

# Search function
def search_query(search_text, filter_by=None, sort_order=None):
    # Creates SearchClient and submits queries

# Routes
@app.route("/")           # Home page
@app.route("/search")     # Search results
```

### Template Structure

- **base.html**: Common layout and styling
- **default.html**: Home page with search form
- **search.html**: Results display with filtering and sorting
- **error.html**: Error handling page

## Troubleshooting

### Common Issues

1. **Connection Errors**: Verify endpoint URL and query key
2. **Import Errors**: Ensure all dependencies are installed
3. **No Results**: Check if indexer has completed successfully
4. **Template Errors**: Verify all template files are present

### Debug Tips

- Check the Flask console for error messages
- Verify Azure AI Search service is running
- Ensure the index name matches exactly
- Test search queries directly in Azure Portal first

## Customization

### Adding New Features

1. **Additional Search Fields**: Modify the `select` parameter in search queries
2. **New Filters**: Add more facetable fields and update templates
3. **Custom Styling**: Edit CSS files in the static folder
4. **Enhanced UI**: Modify HTML templates for new layouts

### Performance Optimization

- Use field selection to limit returned data
- Implement result caching for common queries
- Add pagination for large result sets
- Optimize search parameters for your use case

## Security Considerations

- Never expose admin keys in client applications
- Use query keys for read-only search operations
- Implement proper error handling to avoid information disclosure
- Consider authentication for production deployments

## Related Documentation

- [Azure AI Search Python SDK](https://docs.microsoft.com/en-us/python/api/overview/azure/search-documents-readme)
- [Flask Documentation](https://flask.palletsprojects.com/)
- [Azure AI Search REST API](https://docs.microsoft.com/en-us/rest/api/searchservice/)
