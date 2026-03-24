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

**DOM vs Raw HTML** (Critical for Selenium)
1. Raw HTML = static page source (what you see with requests)
2. DOM (Document Object Model) = live page in memory (what Selenium controls)
3. JavaScript modifies DOM after load → Selenium sees updated content, requests doesn't
4. Browser DevTools (F12): Use Inspector tab to inspect elements in real-time

**HTTP Concepts**
1. Methods: GET (fetch data), POST (send data)
2. Headers: User-Agent, Cookies, Authorization, Referer
3. Status codes:
   - 200 (success)
   - 403 (blocked/forbidden)
   - 404 (not found)
   - 429 (rate limited → add delays)
   - 500 (server error → retry)
4. Sessions: requests.Session() maintains cookies across multiple requests
5. Authentication patterns: API keys, Bearer tokens, Basic auth

**CSS and XPath Selectors**
1. CSS selectors for simple, readable extraction
2. XPath for complex navigation (parent/sibling/text patterns)
3. Example: `//div[@class='price']//span[contains(text(), '$')]`

### 2.3 Data Science Foundation
1. pandas DataFrame basics
2. Data cleaning: nulls, duplicates, type conversion
3. Exporting data: CSV, Excel, Parquet

### 2.4 Responsible Scraping
1. Read robots.txt and website Terms of Service
2. Avoid scraping personal/private/sensitive data
3. Add delays and limit request rate
4. **Legal note**: Some websites explicitly forbid scraping in ToS even if technically possible → respect this


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
| undetected-chromedriver | Anti-detection | Reduces detection signals (not guaranteed; use cautiously) |
| tenacity | Retry logic for Selenium | Handle timeouts and stale elements |

### 3.3 Scale-Up Tools (Later)

| Library | Role | Use Case |
|---|---|---|
| scrapy | Framework for large crawls | Production-scale projects |
| aiohttp | Async requests | High-volume static scraping |

## 4. Practice Progression (Hands-On Learning Path)

Move through these sites in order to build skills systematically:

### Level 1: Static HTML Parsing (requests + BeautifulSoup)
- **quotes.toscrape.com** → Extract quote text, author names, tags
- **httpbin.org** → Understand headers, request/response structure

### Level 2: Pagination Logic (Still static, but add navigation)
- **books.toscrape.com** → Parse book data, navigate through pages with query parameters
- Goal: Collect data across 50+ pages with delays

### Level 3: Client-Side Rendering (Selenium enters here)
- **quotes.toscrape.com/js-scroll** → Same site but JS loads quotes dynamically
- Goal: Use Selenium to scroll and load more content

### Level 4: Real Interaction (Forms, Login, Clicks)
- **webscraper.io/test-sites** → Fill forms, click buttons, handle dynamic popups
- Goal: Selenium interacts with page, not just reads

**Professional Discipline**
1. Always add 1-2 second delays between requests
2. Rotate User-Agent headers
3. Respect robots.txt and site ToS
4. Stop if you hit 429 (rate limit) status
5. Use exponential backoff when hitting 429 (tenacity handles this automatically with `wait_exponential`)

## 5. Core Selenium Concepts (Before Setup)

Understanding these prevents frustration later:

### 5.1 Explicit Waits (Core Strength of Selenium)
DO NOT use `time.sleep()` by default. Instead:

```python
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.common.by import By

wait = WebDriverWait(driver, 10)  # wait up to 10 seconds
element = wait.until(
    EC.presence_of_element_located((By.CSS_SELECTOR, "div.results"))
)
```

Common expected conditions:
- `presence_of_element_located` → element exists in DOM
- `visibility_of_element_located` → element is visible
- `element_to_be_clickable` → element can be clicked
- `text_to_be_present_in_element` → specific text appears

### 5.2 Handling Dynamic Elements
- Stale elements (DOM refreshed, reference broken)
- Elements that appear on scroll
- Infinite scroll (load more on scroll)
- Pop-ups and modals (close and continue)

### 5.3 Browser Modes
- **Regular mode**: Good for debugging (see browser actions)
- **Headless mode**: Fast, server-friendly, no display window

### 5.4 Anti-Bot Awareness (Real-World Reality)
Most websites detect and block bots. Selenium signals:
- Browser window size stays the same
- WebDriver property exposed in console
- No human-like mouse movement/delays

Strategies:
1. Add random delays between actions
2. Use headless=False during testing
3. Rotate User-Agent strings
4. Handle CAPTCHA (sometimes skip scraping that site)
5. Use proxy if IP is blocked

---

## 6. Windows Setup in VS Code with Conda (Step by Step)

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
pip install selenium webdriver-manager pandas beautifulsoup4 lxml requests python-dotenv tenacity openpyxl tqdm undetected-chromedriver
```

**Important Note:**
- Package name: `beautifulsoup4`
- Import name: `from bs4 import BeautifulSoup` (not beautifulsoup4)
- First Selenium run downloads ChromeDriver (~200MB) — be patient

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

## 7. Selenium First Script (Sanity Check)

**Debug Mode (see browser in action):**
```python
from selenium import webdriver
from selenium.webdriver.chrome.service import Service
from webdriver_manager.chrome import ChromeDriverManager
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.common.by import By
import time

driver = webdriver.Chrome(service=Service(ChromeDriverManager().install()))
driver.get("https://quotes.toscrape.com/js/")
driver.maximize_window()

# Wait for quotes to load (max 10 seconds)
wait = WebDriverWait(driver, 10)
quotes = wait.until(
    EC.presence_of_all_elements_located((By.CLASS_NAME, "quote"))
)

print(f"Page title: {driver.title}")
print(f"Found {len(quotes)} quotes")

time.sleep(2)  # See the page before closing
driver.quit()
```

**Production Mode (headless, optimized for speed):**
```python
from selenium import webdriver
from selenium.webdriver.chrome.options import Options
from selenium.webdriver.chrome.service import Service
from webdriver_manager.chrome import ChromeDriverManager
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.common.by import By

options = Options()
options.add_argument("--headless")
options.add_argument("--no-sandbox")
options.add_argument("--disable-dev-shm-usage")
options.add_argument("--blink-settings=imagesEnabled=false")  # Disable images for 30-50% speed boost

driver = webdriver.Chrome(
    service=Service(ChromeDriverManager().install()),
    options=options
)

driver.get("https://quotes.toscrape.com/js/")
wait = WebDriverWait(driver, 10)
quotes = wait.until(EC.presence_of_all_elements_located((By.CLASS_NAME, "quote")))

print(f"Found {len(quotes)} quotes")
driver.quit()
```

**Performance Note**: Disabling images can significantly reduce page load time when images are not required for data extraction.

**Expected Outcome:**
1. Debug mode: Browser opens, you see it fetch quotes
2. Production mode: Script runs silently, prints results
3. Both: Browser closes cleanly, no errors
4. First run: May take 30-60 seconds (ChromeDriver download)


## 8. Data Scientist Best Practices

### Architecture Principle
1. **Collection Layer**: Raw data → save as-is (HTML or JSON snapshot)
2. **Parsing Layer**: Extract structured fields (title, price, URL)
3. **Cleaning Layer**: Validate types, handle nulls, deduplicate
4. **Storage Layer**: Save to CSV/Parquet/Database

### Schema and Validation
```python
import pandas as pd
from datetime import datetime

# Define schema
schema = {
    'quote_id': 'int64',
    'quote_text': 'object',  # pandas uses 'object' for strings
    'author': 'object',
    'scraped_at': 'datetime64[ns]',
    'source_url': 'object'
}

# After parsing
df = pd.DataFrame(raw_data)
df['scraped_at'] = datetime.now()
df['source_url'] = base_url

# Validate (correct approach)
for col, dtype in schema.items():
    assert col in df.columns, f"Missing column: {col}"
    assert df[col].dtype.name == dtype, f"Wrong dtype for {col}: expected {dtype}, got {df[col].dtype}"

assert df.isnull().sum().sum() == 0, "NULL values found!"
assert df.duplicated(subset=['quote_id']).sum() == 0, "Duplicates found!"

# Deduplication (keep most recent)
df = df.sort_values('scraped_at').drop_duplicates(subset=['quote_id'], keep='last')

df.to_csv('quotes_curated.csv', index=False)
```

### Logging Best Practice
```python
import logging

logging.basicConfig(
    level=logging.INFO,  # Use DEBUG during dev, INFO for production
    format='%(asctime)s - %(levelname)s - %(message)s',
    handlers=[
        logging.FileHandler('scraper.log'),
        logging.StreamHandler()
    ]
)

logger = logging.getLogger(__name__)
logger.setLevel(logging.INFO)  # Set logger level explicitly

# Log levels guide:
# logger.debug() → verbose dev info (URLs, selectors, retries)
# logger.info() → important milestones ("Started scrape", "Got 150 records")
# logger.warning() → unexpected but recoverable ("Timeout on page 5, retrying...")
# logger.error() → failures ("Download failed after 3 retries")

logger.info(f"Starting scrape of {url}")
try:
    data = scrape_page(url)
    logger.info(f"Successfully scraped {len(data)} records")
except Exception as e:
    logger.error(f"Scrape failed: {e}")
```


## 9. Common Selenium Mistakes to Avoid

1. **Overusing sleep** → Use explicit waits (`WebDriverWait`) instead
2. **Brittle selectors** → Avoid absolute XPath; use class/id when possible
3. **Ignoring exceptions** → Catch and log `StaleElementReferenceException`, `TimeoutException`
4. **Forgetting headless mode** → Use headless for servers/automation, regular for debugging
5. **No retry logic** → Network errors happen; wrap Selenium calls with `tenacity.retry`
6. **Skipping debug screenshots** → Save page screenshots when scrapes fail for debugging

### Example: Robust Element Click
```python
from selenium.webdriver.common.action_chains import ActionChains
from selenium.webdriver.common.by import By
from tenacity import retry, stop_after_attempt, wait_exponential

@retry(stop=stop_after_attempt(3), wait=wait_exponential(multiplier=1, min=2, max=10))
def click_element(driver, selector):
    element = driver.find_element(By.CSS_SELECTOR, selector)
    ActionChains(driver).move_to_element(element).click().perform()
    return True

try:
    click_element(driver, "button.load-more")
except Exception as e:
    driver.save_screenshot("error_click_failed.png")
    logger.error(f"Click failed after retries: {e}")
```

## 10. End-to-End Scraping Workflow (Data Scientist Mindset)

This is how professionals approach every scraping project:

### Step 1: Identify Target
- What data? (prices, reviews, metadata)
- Where? (which website/API)
- How often? (one-time or recurring)

### Step 2: Inspect Network & Page Behavior (API-First Rule)
**ALWAYS check Network tab first** — this is how professionals save development time.
1. Open browser DevTools (F12)
2. Go to Network tab
3. Interact with page (click, scroll, filter, search)
4. Observe: Are API calls made? (look for XHR/Fetch requests)
5. **If public API exists** → Use `requests` directly (10x faster than Selenium)
6. **If page is JS-heavy with no API** → Use Selenium
7. **Bonus**: If API is in Network tab, inspect the request URL and headers — often no auth needed

### Step 3: Choose Tool
- **Static HTML + simple extraction** → requests + BeautifulSoup
- **JS-rendered, clicks needed** → Selenium (small to medium datasets)
- **Large-scale crawling** → Scrapy or async requests (Selenium does not scale well for high-volume scraping)
- **API available** → Always prefer requests over Selenium

### Step 4: Extract & Parse
Separate collection from parsing. Example:
```python
# Collect raw
page_source = driver.page_source
with open('raw_page.html', 'w') as f:
    f.write(page_source)

# Parse structured
from bs4 import BeautifulSoup
soup = BeautifulSoup(page_source, 'html.parser')
data = []
for item in soup.find_all('div', class_='product'):
    data.append({
        'title': item.find('h2').text,
        'price': item.find('span', class_='price').text,
        'url': item.find('a')['href']
    })
```

### Step 5: Clean with Pandas
```python
import pandas as pd
df = pd.DataFrame(data)
df['price'] = df['price'].str.replace('$', '').astype(float)
df = df.dropna()
df = df.drop_duplicates()
```

### Step 6: Store
```python
df.to_csv('products.csv', index=False)
# Or for larger projects:
df.to_parquet('products.parquet')  # Compressed, type-safe
```

### Step 7: Validate
```python
# Sanity checks
assert len(df) > 0, "No data scraped"
assert df['price'].dtype == float, "Price not numeric"
assert df['url'].str.startswith('http').all(), "Invalid URLs"
print(f"✓ Scraped {len(df)} records successfully")
```

---

## 11. Quick Setup Commands (Copy/Paste)

```bash
conda create -n webscrape_selenium python=3.11 -y
conda activate webscrape_selenium
pip install selenium webdriver-manager pandas beautifulsoup4 lxml requests python-dotenv tenacity openpyxl tqdm undetected-chromedriver
python -c "import selenium, pandas, bs4, lxml, requests; print('Setup Complete')"
```

## 12. Practical Example: E-Commerce Price Monitoring Pipeline

Here's where this knowledge applies in real work:

**Scenario**: Track competitor pricing trends on an e-commerce site
- **Identify Target**: Product listings, prices, availability
- **Inspect Network**: Find API endpoints (often there!)
- **Tool Choice**: If API exists → requests; if JS-heavy → Selenium
- **Extract**: Product name, price, rating, URL, timestamp
- **Clean**: Normalize prices, handle currency conversions, deduplicate
- **Analyze**: Track price changes over time, identify patterns, alert on drops
- **Output**: Daily CSV/database for BI dashboard or ML model training

This workflow applies to job postings, real estate, news sentiment, supply chain data—any structured web content.

---

## 13. Next Professional Upgrades

1. Build one reusable Selenium scraper template with config file
2. Add proxy rotation and user-agent strategy
3. Schedule jobs and monitor failures
4. Containerize with Docker for consistent deployment
5. Move high-volume static jobs to Scrapy + async pipelines
