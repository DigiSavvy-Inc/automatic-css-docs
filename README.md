# Content Scraper

A flexible tool for scraping website content and converting it to well-formatted Markdown files for enhanced AI-powered documentation.

## Purpose

This repository provides tools to gather documentation from websites and convert it to GitHub-friendly Markdown format. The resulting documentation repository creates a valuable resource for developers using AI-enhanced tools, enabling better knowledge access and management.

### Key Benefits

- **Enhanced AI Integration**: Modern development workflows leverage Large Language Model (LLM) tools like ChatGPT and Claude. These AI assistants can directly connect to GitHub repositories, allowing developers to have interactive conversations about documentation stored here.

- **Unified Knowledge Base**: By centralizing documentation in a GitHub repository, teams can maintain context while accessing information, creating a more seamless development experience.

- **Version Control and History**: Documentation changes are tracked with robust version control, making it easier to understand how documentation evolves alongside software.

- **Community Contributions**: GitHub's established contribution workflow (issues, pull requests, discussions) enables easier community suggestions for improvements or corrections to documentation.

## Features

- Scrape content from URLs listed in CSV files
- Parse sitemap.xml files to extract URLs for scraping
- Convert HTML content to clean Markdown format
- Organize content into appropriate directories
- Clean and format filenames
- Parallel processing for faster scraping

# Getting Started

This guide will help you get up and running with the documentation scraper.

## Installation

### Option 1: Standard Installation

```bash
# Clone the repository
git clone https://github.com/DigiSavvy-Inc/ds-doc-scraper.git
cd ds-doc-scraper

# Install dependencies
pip install -r requirements.txt

# Make scripts executable (macOS/Linux only)
chmod +x scripts/*.py scripts/*.sh
```

### Option 2: Using Virtual Environment (Recommended)

```bash
# Clone the repository
git clone https://github.com/DigiSavvy-Inc/ds-doc-scraper.git
cd ds-doc-scraper

# Create virtual environment
python -m venv venv

# Activate virtual environment
# On macOS/Linux:
source venv/bin/activate
# On Windows:
venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Make scripts executable (macOS/Linux only)
chmod +x scripts/*.py scripts/*.sh
```

> **Note for Windows users**: The `chmod` command is not needed on Windows. You may need to adjust how you run the scripts (e.g., `python scripts/scraper.py` instead of directly executing them).

## Quick Start Guide

Here's a complete workflow to scrape your first documentation website:

### 1. Get URLs to Scrape

You need a list of URLs to scrape. Choose one of these methods:

#### Method A: Extract URLs from a Sitemap
```bash
# Replace with your target website's sitemap URL
python scripts/sitemap_parser.py https://docs.example.com/sitemap.xml --output urls.csv
```

#### Method B: Create a CSV File Manually
Create a file named `urls.csv` with a column header "url" containing the URLs:
```
url
https://docs.example.com/page1.html
https://docs.example.com/page2.html
https://docs.example.com/page3.html
```

### 2. Run the Scraper

Choose one of these options based on your needs:

#### Option 1: Basic Scraper (Safe Option)
```bash
# Standard scraping with 2-second delay between requests
python scripts/scraper.py urls.csv --output knowledge_base --delay 2
```

#### Option 2: Parallel Scraper (Faster)
```bash
# Use multiple workers for faster processing
python scripts/batch_scraper.py urls.csv --output knowledge_base --workers 3 --delay 2
```

### 3. Organize the Content (Optional)

Choose one of these options to organize your scraped content:

#### Option 1: Basic Organization
```bash
# Use predefined categories (getting-started, features, etc.)
bash scripts/organize_docs.sh knowledge_base --output organized_docs
```

#### Option 2: Interactive Organization (Recommended)
```bash
# Interactive script with customizable categories and options
bash scripts/interactive_organize.sh
```

The interactive organizer lets you:
- Define your own categories
- Customize keywords for categorization
- Maintain existing directory structure
- Preview and get summaries of changes

### 4. Generate Documentation Structure (Optional)

Create a clean folder structure with proper links:
```bash
bash scripts/generate_structure.sh
```

This generates a README with just the folder structure and links, without listing all individual files.

### 5. Prepare for GitHub Deployment (Optional)

Create a clean deployment package for GitHub:
```bash
bash prepare_for_deployment.sh
```

### 6. View Your Documentation

The converted Markdown files will be in:
- `knowledge_base/` - Raw scraped files
- `organized_docs/` - Organized documentation (if you ran step 3)
- `deployment/` - GitHub-ready documentation (if you ran step 4)

## Usage Examples

### Standard Usage

```bash
# Scrape URLs from a CSV file
python scripts/scraper.py input.csv --output knowledge_base --delay 1

# Parse a sitemap.xml and generate a CSV file
python scripts/sitemap_parser.py https://example.com/sitemap.xml --output urls.csv

# Scrape using multiple workers for faster processing
python scripts/batch_scraper.py input.csv --output knowledge_base --workers 5

# Check for missing documents
python scripts/check_missing.py input.csv knowledge_base

# Organize documents into a structured format
bash scripts/organize_docs.sh knowledge_base --output organized_docs

# Prepare for GitHub deployment
bash prepare_for_deployment.sh
```

### Resuming an Interrupted Scrape

If your scraping process was interrupted (e.g., by a system shutdown, network issue, or manual termination):

```bash
# 1. Identify which URLs haven't been processed yet
python scripts/check_missing.py urls.csv knowledge_base

# 2. This creates a missing_urls.csv file with unprocessed URLs

# 3. Run the batch scraper on just the missing URLs to resume
python scripts/batch_scraper.py missing_urls.csv --output knowledge_base --workers 10
```

This workflow allows you to efficiently continue from where you left off without re-scraping already processed URLs.

### Conservative Usage (Avoiding Rate Limiting)

```bash
# Parse sitemap with longer delays between requests
python scripts/sitemap_parser.py https://example.com/sitemap.xml --output urls.csv

# Scrape with conservative delay (3 seconds between requests)
python scripts/scraper.py urls.csv --output knowledge_base --delay 3

# Batch scrape with fewer workers and longer delays
python scripts/batch_scraper.py urls.csv --output knowledge_base --workers 2 --delay 5

# For very strict rate limits, use single worker with long delays
python scripts/scraper.py urls.csv --output knowledge_base --delay 10
```

### Tips for Avoiding Rate Limits

- Increase the `--delay` parameter (in seconds) to slow down requests
- Reduce the number of `--workers` when using batch_scraper.py
- Consider scraping in smaller batches by splitting your CSV file
- Run scraping during off-peak hours for the target website
- Add appropriate User-Agent headers (scripts already include this)

### Input Format

- **CSV files** should have a column with URLs to scrape
- **sitemap.xml** should follow the standard sitemap protocol

## Output

By default, all scraped content is saved to the `knowledge_base` directory in Markdown format. The organized content is placed in the `organized_docs` directory, and prepared for deployment in the `deployment` directory.

## Deployment

The `prepare_for_deployment.sh` script helps you prepare the documentation for GitHub:

1. Creates a clean `deployment` directory with only necessary files
2. Organizes documentation into an appropriate structure
3. Generates a new README.md suitable for GitHub display

After running the deployment script, follow the on-screen instructions to initialize a new git repository and push to GitHub.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgements

We would like to thank the developers of the libraries and tools that make this project possible. See [ACKNOWLEDGEMENTS.md](ACKNOWLEDGEMENTS.md) for the complete list.