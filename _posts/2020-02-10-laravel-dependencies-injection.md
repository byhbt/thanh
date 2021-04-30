---
layout: post
title: "Laravel dependencies Injection"
categories:
  - Python
tags:
  - python
  - scrapy
---
Notice:
Here we just learn to drive a car, not to build an engine. So I don't dig deeper into how crawler work

As we know Scrapy is a popular framework for crawling data.
There is another library is called BeautifulSoup.

**What is Scrapy?**
> An open source and collaborative **framework** for extracting the data you need from websites.

**What is Beautiful Soup?**
> Beautiful Soup is a Python **library** for pulling data out of HTML and XML files.

### What is the difference between Scrapy and BeautifulSoup?

Scrapy is a Framework.

Beautiful Soup is a library.

### Let start coding

Step 1: # ALWAYS create env first

```
python3 -m venv env
```

Step 2: Install Scrapy

```
pip install scrapy
```

Step 3: Start project

```
scrapy startproject
scrapy genspider QuoteSpider
scrapy crawl QuoteSpider
```

