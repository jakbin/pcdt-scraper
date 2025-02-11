# pcdt-scraper Documentation

## Table of Contents
1. [Introduction](#introduction)
2. [Requirements](#requirements)
3. [Installation](#installation)
4. [Getting Started](#getting-started)
5. [Core Features](#core-features)
6. [API Reference](#api-reference)
7. [Examples](#examples)
8. [Troubleshooting](#troubleshooting)

## Introduction

[![Python package](https://github.com/jakbin/pcdt-scraper/actions/workflows/publish.yml/badge.svg)](https://github.com/jakbin/pcdt-scraper/actions/workflows/publish.yml)
[![PyPI version](https://badge.fury.io/py/pcdt-scraper.svg)](https://pypi.org/project/pcdt-scraper)
[![Downloads](https://pepy.tech/badge/pcdt-scraper/month)](https://pepy.tech/project/pcdt-scraper)
[![Downloads](https://static.pepy.tech/personalized-badge/pcdt-scraper?period=total&units=international_system&left_color=green&right_color=blue&left_text=Total%20Downloads)](https://pepy.tech/project/pcdt-scraper)
![GitHub commit activity](https://img.shields.io/github/commit-activity/m/jakbin/pcdt-scraper)
![GitHub last commit](https://img.shields.io/github/last-commit/jakbin/pcdt-scraper)

pcdt-scraper is a Python web scraping library that combines the power of PyChromeDevTools with a Selenium-like syntax. It's designed to handle websites that block traditional HTTP requests but allow Chrome browser requests. The library provides an intuitive interface for web scraping tasks while utilizing Chrome's DevTools Protocol.

## Requirements
- Python 3.6 or higher
- Chrome or Chromium browser
- `bs4` (BeautifulSoup4)
- `PyChromeDevTools`

## Installation

Install using pip:
```bash
pip install pcdt-scraper
```

Or using pip3:
```bash
pip3 install pcdt-scraper
```

## Getting Started

### 1. Start Chrome/Chromium in Debug Mode
First, you need to run Chrome or Chromium with remote debugging enabled:

```bash
# Regular mode
chromium --remote-debugging-port=9222 --remote-allow-origins=*
```
```bash
# Or headless mode
chromium --remote-debugging-port=9222 --remote-allow-origins=* --headless
```

### 2. Basic Usage
```python
from pcdt_scraper import WebScraper

# Initialize the scraper
scraper = WebScraper()

try:
    # Navigate to a webpage
    scraper.get("https://www.example.com")
    
    # Find elements and extract data
    element = scraper.find_element_by_class_name("my-class")
    text = element.text()
    
finally:
    # Always close the scraper
    scraper.close()
```

## Core Features

### WebScraper Class
The main class that handles all scraping operations. It provides:
- Selenium-like syntax for easy transition
- Chrome DevTools integration
- BeautifulSoup parsing capabilities

### ElementWrapper Class
Wraps web elements with convenient methods:
- `text()`: Get element's text content
- `get_attribute(attribute)`: Get specific attribute value
- `is_displayed()`: Check if element exists
- `get_html()`: Get element's HTML content

### Elements Class
Collection class for handling multiple elements:
- Iterable interface
- Length checking
- Index-based access

## API Reference

### WebScraper Methods

#### Navigation
```python
scraper.get(url, timeout=60)  # Navigate to a webpage
scraper.close()  # Close the browser
scraper.quit()   # Alias for close()
```

#### Element Finding Methods
```python
# Single element finders
scraper.find_element_by_id(id_)
scraper.find_element_by_class_name(class_name)
scraper.find_element_by_tag_name(tag_name)
scraper.find_element_by_name(name)
scraper.find_element_by_css_selector(css_selector)
scraper.find_element_by_xpath(xpath)  # Limited support

# Multiple elements finders
scraper.find_elements_by_class_name(class_name)
scraper.find_elements_by_tag_name(tag_name)
scraper.find_elements_by_name(name)
scraper.find_elements_by_css_selector(css_selector)
```

#### Page Content
```python
scraper.get_page_source()      # Get page source (alias)
scraper.get_page_content()     # Get parsed page content
```

## Examples

### 1. Basic Scraping
```python
from pcdt_scraper import WebScraper

scraper = WebScraper()
try:
    scraper.get("https://www.example.com")
    title = scraper.find_element_by_tag_name("h1").text()
    print(f"Page title: {title}")
finally:
    scraper.close()
```

### 2. Working with Multiple Elements
```python
from pcdt_scraper import WebScraper

scraper = WebScraper()
try:
    scraper.get("https://www.example.com")
    links = scraper.find_elements_by_tag_name("a")
    
    for link in links:
        href = link.get_attribute("href")
        text = link.text()
        print(f"Link: {text} -> {href}")
finally:
    scraper.close()
```

### 3. Using CSS Selectors
```python
from pcdt_scraper import WebScraper

scraper = WebScraper()
try:
    scraper.get("https://www.example.com")
    elements = scraper.find_elements_by_css_selector(".content article")
    
    for element in elements:
        title = element.find_element_by_class_name("title").text()
        print(f"Article title: {title}")
finally:
    scraper.close()
```

## Troubleshooting

### Common Issues

1. **ConnectionError**
   - Error: "Got ConnectionError, it seems your chrome remote instance is not running"
   - Solution: Ensure Chrome/Chromium is running with remote debugging enabled

2. **Page Load Timeout**
   - Error: "Page load timed out after X seconds"
   - Solution: Increase the timeout parameter in the `get()` method

3. **Element Not Found**
   - Solution: 
     - Check if the element exists in the page source
     - Try different selector methods
     - Ensure the page has fully loaded

### Best Practices

1. Always use try-finally blocks to ensure proper cleanup
2. Close the scraper after use
3. Handle potential exceptions appropriately
4. Use appropriate timeouts for your use case
5. Choose the most specific selector method available

This documentation provides a comprehensive guide to using pcdt-scraper. For more information or to contribute to the project, visit the [GitHub repository](https://github.com/jakbin/pcdt-scraper).