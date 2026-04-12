# youtube_user

> 🔥 YouTube Influencer Finder — Essential Tool for Outbound Brands
>
> 💡 Features: ✅ Search influencers by keywords ✅ Filter by country/region ✅ Filter by subscriber count
>
> 🔗 Download: [Latest Release](https://github.com/mashukui/youtube_user/releases)

<p align="center">
<a href="README.md">简体中文 README</a> | <a href="README.en.md">English README</a>
</p>

# 1. Overview

## 1.1 Background

![Target - YouTube Influencers](https://files.mdnice.com/user/32110/9df21c30-9071-48b7-bbbe-165665742a07.png)

YouTube, as a leading global video social platform, brings together hundreds of millions of daily active users and diverse influencer ecosystems across multiple regions, offering enormous commercial value. Accurately collecting platform user data helps gain deep insights into influencer content trends and commercial potential, enabling efficient business cooperation. Based on this demand, I independently developed "YouTube Influencer Finder v2.0", built with Python for fully automated data collection.

Why version 2.0? Because I previously released an initial version using a browser automation framework, and have now upgraded it to use the official YouTube Data API v3.

## 1.2 Software Interface

![Software Interface](https://files.mdnice.com/user/32110/b77dfa83-ac34-4ee6-b709-d5c108ae7c7b.png)

## 1.3 Results Preview

Scraped results 1 (fields are split into 2 images due to quantity):
![Results Part 1: First 10 Fields](https://files.mdnice.com/user/32110/c43c1730-8782-422e-8eed-53feb73f1670.png)

![Results Part 2: Last 12 Fields](https://files.mdnice.com/user/32110/3af9e3c3-1db1-4329-9bdd-6498c9561876.png)

Scraped results 2 (clean version):
> https://docs.qq.com/sheet/DVEFhZlFKR1NXVEdN?tab=ht1erv

## 1.4 Important Notes

1. Supports both Windows and Mac — no Python environment setup required, ready to use out of the box!
2. Uses YouTube official API — no anti-scraping risks
3. Supports filtering by: brand keywords / country & region / subscriber count range
4. During scraping, data is saved to CSV after every single record (not at the end). This prevents data loss if the process is interrupted (default interval: 1–2s per record, customizable)
5. Detailed log files record the entire process for easy troubleshooting
6. CSV includes 22 core fields: Search Keyword, Page Number, Video Title, Video URL, Current Video Views, Creator Name, Creator URL, Channel ID, Channel URL, Country, Telegram Link, WhatsApp Link, Twitter Link, Facebook Link, Instagram Link, TikTok Link, Subscribers, Total Videos, Total Views, Join Date, Email_Note, Email_Additional

# 2. Technical Implementation

## 2.1 Architecture

Built with Python, using these main modules:

```python
tkinter: GUI interface
requests: HTTP requests
threading: Multi-threaded collection
json: Response parsing
csv: CSV export
logging: Log recording
```

## 2.2 GUI Interface

Built with tkinter for rapid frontend development:

```python
# Create main window
root = tk.Tk()
root.title('YouTube Influencer Finder v2.0')
# Set window size
root.minsize(width=900, height=650)
# Window icon
root.iconbitmap('mage.ico')
```

## 2.3 Scraping Logic

**YouTubeApiClient class** — YouTube Data API v3 wrapper:

```python
search_videos()    # Search videos by keyword, return video list
get_channels()     # Batch fetch channel details (subscribers, etc.)
get_videos()        # Batch fetch video details (views, etc.)
get_regions()       # Get global country code mapping
```

**YouTubeSpider class** — Core scraping logic:

Two-phase collection:

```python
Phase 1 (API Phase)
├── Search videos by keyword (50 per page)
├── Extract channel IDs, deduplicate
├── Batch call API for channel info (subscribers/country/join date)
└── Filter: subscriber range, country/region
    ↓
Phase 2 (Web Scraping, optional)
└── Visit channel "About" page
    ├── Extract emails via regex
    ├── Extract social media links (Telegram/WhatsApp/Twitter, etc.)
    └── Fill in contact info that API cannot provide
```

## 2.4 Logging

**Log_week class** — Logging module:

Logs are written to both the software UI and local files for persistent storage.

```python
def get_logger(self):
    self.logger = logging.getLogger(__name__)
    # Log format
    formatter = '[%(asctime)s-%(filename)s][%(funcName)s-%(lineno)d]--%(message)s'
    # Log level
    self.logger.setLevel(logging.DEBUG)
    log_formatter = logging.Formatter(formatter, datefmt='%Y-%m-%d %H:%M:%S')
    # Log file name
    info_file_name = time.strftime("%Y-%m-%d") + '.log'
    # Save to specific directory
    case_dir = r'./logs/'
    if not self.logger.handlers:
        # Console handler
        sh = logging.StreamHandler()
        info_handler = TimedRotatingFileHandler(filename=case_dir + info_file_name,
                                                 when='MIDNIGHT',
                                                 interval=1,
                                                 backupCount=7,
                                                 encoding='utf-8')
        self.logger.addHandler(sh)
        sh.setFormatter(log_formatter)
        self.logger.addHandler(info_handler)
        info_handler.setFormatter(log_formatter)
    return self.logger
```

# 3. Usage Guide

## 3.1 Configure API Key

As mentioned in the overview, the software has been upgraded to v2.0 using the YouTube Data API v3. You need to configure your own API key before running.

Setup guide: [Step-by-Step YouTube Data API v3 Setup Tutorial](https://mp.weixin.qq.com/s/cFQ8GM3EK5B448qLWytsBw)

After getting your API key, add it to `config_pub.json` in the software directory:
![Configure API Key](https://files.mdnice.com/user/32110/3d586b34-a113-4f15-b7ab-89af7935acc2.png)

The config file has 2 parameters:
- `wait_sec`: API request interval (default 1s, adjustable)
- API key itself

## 3.2 Start Scraping

After configuring the key, you're ready to collect (make sure your network is connected).

On the software interface, enter your criteria:
![Filter Criteria](https://files.mdnice.com/user/32110/b0040825-58ac-4dda-91ad-b5d92d5a4961.png)

Click "Start" and the software will automatically collect data in batch.

## 3.3 Usage Tips

- **1. Search Keywords**

Industry names, brand names, competitor terms, multi-word combinations — find creators posting content in your target area.

| Scenario | Keyword Strategy | Effect |
|----------|-----------------|--------|
| Precise creator targeting | Use brand/competitor names | Find creators active in the field |
| Broad collection | Use broad terms like "beauty", "gaming" | High volume but diverse creator types |
| Trend riding | Use current hot topics | Find latest active creators in short time |
| Multi-word combo | "beauty tutorial \| skincare review" | Collect creators from multiple directions at once |

- **2. Country/Region**

Supports both Chinese and English input. If unsure, refer to the standard country names in `country.json`.

| Scenario | Country Strategy | Purpose |
|----------|-----------------|---------|
| Brand going global | Target market countries (US/UK/Australia) | Precisely connect with local creators |
| Cross-cultural content | Japan/Korea/Southeast Asia | Find local creators interested in Chinese brands |
| Avoid competition | Smaller markets (France/Germany/Brazil) | Blue ocean in a red sea |
| Bulk collection | Leave empty (no country limit) | Maximum coverage, no creators missed |

- **3. Subscriber Range**

Use subscriber ranges to precisely target creator tiers and quickly establish cooperation.

| Subscriber Range | Best For | Creator Characteristics |
|-----------------|----------|------------------------|
| **0–10K** | Beginners/newcomers | Low cost, high cooperation, but unstable metrics |
| **10K–100K** | Mid-tier creators | Best cost-performance ratio, developing commercial awareness |
| **100K–500K** | Emerging top creators | Strong influence, moderate pricing |
| **500K–5M** | Top KOLs | High influence, but high cost and lower cooperation |
| **5M+** | Super influencers | Luxury brands only, out of reach for most |

- **One-liner summary:** Keywords determine what content creators make, country determines which market, subscriber count determines which tier. Use all three filters together to precisely lock onto your target and save time and cost.

# 4. Demo Video

Full software demo: [YouTube Influencer Finder — Scraping Demo](https://mp.weixin.qq.com/s/Pn6QrsQ1YTYAao3DWWYovg)

# 5. Paid Plans

## 5.1 License Options

```python
Day Pass:     1 day,     ¥39   — Short-term/trial needs
Monthly:      1 month,   ¥149  — Short-term collection needs
Quarterly:    3 months,  ¥399  — Medium-term needs
Yearly:       1 year,    ¥799  — Long-term needs
```

Purchase: https://mgnb.pro/product/youtube_user

## 5.2 One Key Per Machine

To prevent unauthorized redistribution, each license key is bound to one machine and cannot be used on multiple computers.

## 5.3 Single Instance Only

Only one instance of the software can run per computer. Multi-instance is not supported.

## 5.4 Software Maintenance

Independently developed and maintained by me, with long-term updates and stable operation guaranteed.

# 6. Get the Software

Follow the WeChat official account "**老男孩的平凡之路**" and reply "**爬油管博主**" to get the latest installer.

[Direct Download](https://github.com/mashukui/youtube_user/releases)

<img width="1938" height="364" alt="WeChat QR Code" src="https://github.com/user-attachments/assets/5ae8b283-0c5f-48e8-a909-d91dcb09210a" />

