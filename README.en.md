# youtube_user

> Original creation by Mage: A Python-based YouTube influencer crawler that supports filtering by search keywords, country, follower count range, and extracts blogger links, emails, and other contact information.

---

## Table of Contents

- [Background](#background)
  - [Development Background](#development-background)
  - [Software Interface](#software-interface)
  - [Results Demo](#results-demo)
  - [Demo Video](#demo-video)
  - [Software Notes](#software-notes)
- [Core Technologies](#core-technologies)
  - [GUI Interface](#gui-interface)
  - [Web Crawler](#web-crawler)
- [Features](#features)
  - [ChromeDriver Configuration](#chromedriver-configuration)
  - [Data Collection](#data-collection)
- [Pricing](#pricing)
  - [License Key Options](#license-key-options)
  - [One Machine, One Code](#one-machine-one-code)
  - [No Multi-instance](#no-multi-instance)
  - [Software Maintenance](#software-maintenance)
- [Download](#download)

---

## Background

### Development Background

As everyone knows, **YouTube** is the world's largest video social platform with hundreds of millions of users and massive daily active engagement. The influencer bloggers from various countries and regions on the platform hold tremendous commercial value. By collecting YouTube user data, clients can gain deeper insights into the latest trends and commercial value of influencer bloggers, thereby enabling more effective business collaborations.

Therefore, I developed a Python-based crawler tool called **"YouTube User Crawler"**.

![Crawling Target: YouTube Influencers](https://s2.51cto.com/images/blog/202412/08095449_6754fc69f07c821166.png?x-oss-process=image/watermark,size_14,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_20,type_ZmFuZ3poZW5naGVpdGk=)

### Software Interface

Software interface screenshot (currently upgraded to v1.7):

![v1.7](https://private-user-images.githubusercontent.com/228842838/483846366-be718647-0e22-49c7-bce4-67147be5a1b3.png)

### Results Demo

Crawling Results 1 (many fields, may be hard to read):

![Crawling Results Preview](https://s2.51cto.com/images/blog/202412/08095450_6754fc6a39d8685286.png?x-oss-process=image/watermark,size_14,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_20,type_ZmFuZ3poZW5naGVpdGk=)

Crawling Results 2 (Clear version):

> [View Sample Results Spreadsheet](https://docs.qq.com/sheet/DVEFhZlFKR1NXVEdN?tab=ht1er)

### Demo Video

Software operation demo:

> [【Software Demo】YouTube Blogger Collection Tool, Can Crawl Tens of Thousands of Entries, Includes Emails!](https://mp.weixin.qq.com/s/wYz_yLRw_WPHqSj7mWvwhQ)

### Software Notes

Important notes, please read carefully:

- **Windows users** can directly double-click the `.exe` file to use - no Python environment required, very convenient!
- The software uses **browser simulation** for crawling, effectively avoiding anti-scraping measures
- Supports filtering by: **Country/Region** (multiple) and **Follower count range**
- Supports multiple settings for: **Search keywords**, **Country/Region**
- During crawling, data is saved to CSV **after each entry** (not at the end), preventing data loss from unexpected interruptions (~1-2s interval per entry)
- Detailed **log files** record the entire operation process for easy troubleshooting
- **Blogger filtering happens simultaneously** during crawling (not after), ensuring high efficiency!
- The blogger CSV contains **16 core fields**:
  - Search keyword
  - Video title
  - Video link
  - Current video views
  - Blogger name
  - Blogger link
  - Country
  - Telegram link
  - WhatsApp link
  - Twitter link
  - Facebook link
  - Instagram link
  - Follower count
  - Total videos
  - Total views
  - Email

---

## Core Technologies

All software modules are developed using **Python**, with the following main components:

| Module | Purpose |
|--------|---------|
| `tkinter` | GUI software interface |
| `selenium` | Web crawler requests |
| `json` | Response data parsing |
| `csv` | CSV result saving, data cleaning |
| `logging` | Log recording |

### GUI Interface

The software main interface is developed with **tkinter**, Python's standard GUI toolkit, allowing the creation of windows, dialogs, buttons, and other desktop application components. tkinter is built into Python, so no additional library installation is required.

Sample code:

```python
# Create main window
root = tk.Tk()
root.title('YouTube Blogger Collection Software v1.0')

# Set window size
root.minsize(width=900, height=650)

# Search keywords
tk.Label(root, text='Search Keywords:').place(x=30, y=130)
query = tk.StringVar()
query.set('')
entry = tk.Entry(root, bg='#ffffff', width=80, textvariable=query)
entry.place(x=110, y=130, anchor='nw')

# Positioning
tk.Label(root, justify='left', fg='red', text='Separate multiple keywords with |').place(x=650, y=130)
```

### Web Crawler

The crawler uses **Selenium** to simulate browser operations for sending requests.

**Browser initialization:**

```python
# Initialize browser
chrome_driver = './chromedriver.exe'  # Browser driver
chrome_options = webdriver.ChromeOptions()
chrome_options.add_argument('--headless')
browser = webdriver.Chrome(executable_path=chrome_driver, options=chrome_options)
wait = WebDriverWait(browser, 10)
self.tk_show('\nStarting to crawl keyword[{}], browser initializing, please wait..\n'.format(keyword))

# Start crawling
js = 'window.open("https://www.youtube.com/results?search_query={}");'.format(quote(keyword))
browser.execute_script(js)
```

**Parsing blogger country code:**

```python
# Country
try:
    country = browser.find_element(By.XPATH, '//*[@icon="privacy_public"]/../../td[2]').text
except:
    country = ''

if country_list != ['']:  # Country is not empty
    if country not in country_list and country not in country_list2:
        self.tk_show('Country is [{}], does not meet specified countries:{}, skipping!'.format(country, country_list))
        browser.close()
        browser.switch_to.window(handles[-2])
        continue
```

---

## Features

### ChromeDriver Configuration

Before starting collection, install the latest version of Chrome browser, then configure the corresponding version of ChromeDriver:

> [ChromeDriver Download Instructions](https://docs.qq.com/doc/DVFZNdk91eGV0cVRH)

### Data Collection

After configuring ChromeDriver, open the `youtube_user.exe` software and log in:

![Software Login Interface](https://s2.51cto.com/images/blog/202412/08095449_6754fc69ec11e94556.png?x-oss-process=image/watermark,size_14,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_20,type_ZmFuZ3poZW5naGVpdGk=)

Fill in the crawling conditions on the main interface:

![Set Crawling Conditions](https://s2.51cto.com/images/blog/202412/08095450_6754fc6a0ac0140488.png?x-oss-process=image/watermark,size_14,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_20,type_ZmFuZ3poZW5naGVpdGk=)

Click the **Start** button to begin collection.

---

## Pricing

### License Key Options

| Plan | Duration | Price | Notes |
|------|----------|-------|-------|
| Day Pass | 1 day | ¥39 | One-time purchase only. Good for trial/temporary needs |
| Monthly | 1 month | ¥149 | Can purchase multiple times. Good for short-term needs |
| Quarterly | 3 months | ¥399 | Can purchase multiple times. Good for medium-term needs |
| Yearly | 1 year | ¥799 | Can purchase multiple times. Good for long-term needs |

**Option 1: Self-service (Recommended)**

Purchase portal: https://mgnb.pro/product/youtube_user

**Option 2: Alternative self-service**

Purchase portal: https://kjyjf.xetlk.com/s/ptQuS

**Option 3: Manual activation**

After payment, add on WeChat (ID: **493882434**) for manual activation.

### One Machine, One Code

The software uses a **one machine, one code** mechanism. One license key can only run on one computer and cannot be used on multiple computers.

### No Multi-instance

Only one software instance is allowed per computer. Multi-instance is not supported.

### Software Maintenance

The software is independently developed and maintained by the author with long-term updates to ensure stable operation.

---

## Download

To get the latest software package, follow the WeChat Official Account **"老男孩的平凡之路"** and reply with: **爬油管博主软件**

![QR Code - Official Account](https://private-user-images.githubusercontent.com/228842838/530122723-2c77db4e-8483-4b39-b605-7126f0c4a637.png)

---

*Made with ❤️ by Mage*
