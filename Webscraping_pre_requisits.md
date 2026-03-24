# Web Scraping Prerequisites Guide (Data Scientist Path, Selenium Focus)

## 1. Goal
This guide is designed for a professional data science workflow.

You will get:
1. Clean prerequisites (what to learn first)
2. Important web scraping libraries with practical roles
3. Beginner-friendly websites for legal practice
4. Deep Selenium roadmap with Python
5. Complete Windows + VS Code + Conda setup

## 2. Prerequisites You Must Know

### 2.1 Python Foundation
1. Variables, loops, functions, exceptions
2. Lists, dictionaries, list comprehensions
3. File I/O and JSON handling

### 2.2 Web Fundamentals
1. HTML structure: tags, attributes, classes, IDs
2. CSS selectors and basic XPath
3. HTTP methods and status codes: 200, 403, 404, 429, 500

### 2.3 Data Science Foundation
1. pandas DataFrame basics
2. Data cleaning: nulls, duplicates, type conversion
3. Exporting data: CSV, Excel, Parquet

### 2.4 Responsible Scraping
1. Read robots.txt and website Terms of Service
2. Avoid scraping personal/private/sensitive data
3. Add delays and limit request rate


## 3. Important Libraries and Why They Matter

### 3.1 Core Stack (Use in Most Projects)

| Library | Role in Pipeline | Why It Matters |
|---|---|---|
| requests | Fetch static pages or APIs | Lightweight and fast |
| beautifulsoup4 | Parse HTML | Easy extraction with selectors |
| lxml | Fast parser + XPath | Better performance on large pages |
| pandas | Store and analyze data | Data science-ready output |
| python-dotenv | Manage secrets/config | Clean, secure project setup |
| tenacity | Retry transient failures | More reliable scraping jobs |

### 3.2 Selenium Stack (Your Main Focus)

| Library | Role | Use Case |
|---|---|---|
| selenium | Browser automation | JS-heavy pages, click/scroll/login |
| webdriver-manager | Driver setup automation | Simple local environment setup |

### 3.3 Scale-Up Tools (Later)

| Library | Role | Use Case |
|---|---|---|
| scrapy | Framework for large crawls | Production-scale projects |
| aiohttp | Async requests | High-volume static scraping |

## 4. Beginner-Friendly Practice Websites (Recommended)

Use these to practice safely and legally:

1. https://quotes.toscrape.com
2. https://books.toscrape.com
3. https://httpbin.org
4. https://webscraper.io/test-sites

Professional note:
1. Start with static pages first
2. Then move to dynamic JS pages with Selenium
3. Keep request rate polite even on test sites

## 5. Windows Setup in VS Code with Conda (Step by Step)

### Step 1: Install Required Software
1. Install Miniconda (recommended)
2. Install VS Code
3. Install Chrome (or Edge)
4. In VS Code, install extensions:
   - Python (Microsoft)
   - Jupyter (Microsoft)

### Step 2: Open Your Project in VS Code
1. Open your folder in VS Code
2. Open terminal from Terminal > New Terminal

### Step 3: Create and Activate Conda Environment

```bash
conda create -n webscrape_selenium python=3.11 -y
conda activate webscrape_selenium
```

### Step 4: Install Libraries

```bash
pip install selenium webdriver-manager pandas beautifulsoup4 lxml requests python-dotenv tenacity openpyxl tqdm
```

### Step 5: Select Conda Interpreter in VS Code
1. Press Ctrl + Shift + P
2. Run Python: Select Interpreter
3. Choose environment named webscrape_selenium

### Step 6: Verify Installation

```bash
python -c "import selenium, pandas, bs4, lxml, requests; print('Environment ready')"
```

### Step 7: Freeze Environment for Reproducibility

```bash
pip freeze > requirements.txt
conda env export --no-builds > environment.yml
```

## 6. Selenium First Script (Sanity Check)

```python
from selenium import webdriver
from selenium.webdriver.chrome.service import Service
from webdriver_manager.chrome import ChromeDriverManager

driver = webdriver.Chrome(service=Service(ChromeDriverManager().install()))
driver.get("https://quotes.toscrape.com/js/")
print(driver.title)
driver.quit()
```

Expected outcome:
1. Browser opens the page
2. Page title prints in terminal
3. Browser closes cleanly


## 7. Data Scientist Best Practices

1. Separate collection, parsing, and cleaning steps
2. Keep one raw copy of every scrape run
3. Add timestamp and source URL columns
4. Log run metadata (duration, rows, failures)
5. Validate schema before downstream analysis


## 8. Common Selenium Mistakes to Avoid

1. Overusing sleep instead of explicit waits
2. Using brittle absolute XPath for all elements
3. Ignoring stale element and timeout exceptions
4. Not running in headless mode for automation jobs
5. Skipping logging and debug screenshots

## 9. Quick Setup Commands (Copy/Paste)

```bash
conda create -n webscrape_selenium python=3.11 -y
conda activate webscrape_selenium
pip install selenium webdriver-manager pandas beautifulsoup4 lxml requests python-dotenv tenacity openpyxl tqdm
python -c "import selenium, pandas, bs4, lxml, requests; print('Setup Complete')"
```

## 10. Next Professional Upgrades

1. Build one reusable Selenium scraper template with config file
2. Add proxy rotation and user-agent strategy
3. Schedule jobs and monitor failures
4. Containerize with Docker for consistent deployment
5. Move high-volume static jobs to Scrapy + async pipelines
