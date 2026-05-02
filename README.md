# 🚀 n8n Automation: Theory & Projects
![n8n Logo](https://raw.githubusercontent.com/n8n-io/n8n/master/assets/n8n-logo.png)

Welcome to my repository dedicated to mastering **n8n**!
Here you will find comprehensive guides on the theory behind automation and AI agents, alongside practical, real-world projects.

---

## 📚 What's Inside?

### 🔹 Theory & Basics

Dive into the core concepts of n8n and [Make.com](http://Make.com). Learn how to build your first workflow, understand nodes, and explore the architecture of AI agents.

- **[Introduction to n8n & Make](./Basics_of_n8n/Introduction_to_n8n_make.md)**

### 🛠️ Projects

I am constantly building and adding new automation projects here.

> **📂 [Check out the Projects folder](./Project)** to see what's been built so far!

---

## 🌐 Freemium APIs for n8n — Complete Reference

A curated list of free and freemium APIs that work seamlessly with n8n — via native nodes or the HTTP Request node.

---

### 📰 News & Content

| API | Free Tier | n8n Integration |
|-----|-----------|-----------------|
| [NewsAPI](https://newsapi.org) | 100 req/day, 1 month old news | HTTP Request |
| [GNews](https://gnews.io) | 100 req/day, real-time | HTTP Request |
| [The Guardian API](https://open-platform.theguardian.com) | Unlimited with key | HTTP Request |
| [NY Times API](https://developer.nytimes.com) | 500 req/day | HTTP Request |
| [MediaStack](https://mediastack.com) | 500 req/month | HTTP Request |
| [RSS Feeds](https://www.rssboard.org) | Fully free, no key needed | Native RSS Feed node |

> 💡 **n8n tip:** Use the native RSS Feed node for sources like Naukri, Reddit, BBC, and any blog — zero setup, zero cost.

---

### 🤖 AI / LLM

| API | Free Tier | n8n Integration |
|-----|-----------|-----------------|
| [Google Gemini](https://ai.google.dev) | 1M tokens/day Flash, 15 RPM | Native node |
| [Groq](https://groq.com) | 14,400 req/day, ultra-fast | HTTP Request |
| [Mistral AI](https://mistral.ai) | Free tier, limited | HTTP Request |
| [Cohere](https://cohere.com) | Trial key free | HTTP Request |
| [Hugging Face](https://huggingface.co/inference-api) | Free Inference API | HTTP Request |
| [OpenRouter](https://openrouter.ai) | Free models available | HTTP Request |
| [Anthropic Claude](https://anthropic.com) | Pay-per-use, no free tier | Native node |

> 💡 **n8n tip:** Gemini Flash is the best free LLM for n8n workflows — generous limits and fast. Use it for summarization, classification, and scoring inside your flows.

---

### 💼 Jobs & Recruitment

| API | Free Tier | n8n Integration |
|-----|-----------|-----------------|
| [Greenhouse API](https://developers.greenhouse.io) | Fully free, no key needed | HTTP Request |
| [Lever API](https://hire.lever.co/developer/documentation) | Fully free, no key needed | HTTP Request |
| [Ashby API](https://developers.ashbyhq.com) | Fully free, no key needed | HTTP Request |
| [Adzuna](https://developer.adzuna.com) | 250 req/month free | HTTP Request |
| [Naukri RSS](https://www.naukri.com/rss) | Fully free | Native RSS Feed node |
| [RapidAPI India Jobs](https://rapidapi.com) | 100 req/month free tier | HTTP Request |

> 💡 **n8n tip:** Greenhouse, Lever, and Ashby are the gold standard — clean structured JSON with job title, location, and URL. No auth required. Perfect for a daily job alert workflow.

**Greenhouse URL pattern:**

    https://boards-api.greenhouse.io/v1/boards/{company-slug}/jobs

**Example India company slugs:**

    freshworks | postman | browserstack | sprinklr | razorpay
    observeai  | whatfix | chargebee    | netomi   | signzy

---

### 💰 Finance & Markets

| API | Free Tier | n8n Integration |
|-----|-----------|-----------------|
| [Alpha Vantage](https://www.alphavantage.co) | 25 req/day | HTTP Request |
| [Yahoo Finance (unofficial)](https://finance.yahoo.com) | Unlimited | HTTP Request |
| [ExchangeRate API](https://www.exchangerate-api.com) | 1,500 req/month | HTTP Request |
| [CoinGecko](https://www.coingecko.com/en/api) | 30 req/min, crypto data | HTTP Request |
| [Open Exchange Rates](https://openexchangerates.org) | 1,000 req/month | HTTP Request |
| [Polygon.io](https://polygon.io) | 5 req/min, stocks | HTTP Request |

---

### 🌤️ Weather & Location

| API | Free Tier | n8n Integration |
|-----|-----------|-----------------|
| [OpenWeatherMap](https://openweathermap.org/api) | 1,000 req/day | HTTP Request |
| [WeatherAPI](https://www.weatherapi.com) | 1M calls/month | HTTP Request |
| [Open-Meteo](https://open-meteo.com) | Fully free, no key needed | HTTP Request |
| [IP-API](https://ip-api.com) | 45 req/min geolocation | HTTP Request |
| [BigDataCloud](https://www.bigdatacloud.com) | 10K req/month | HTTP Request |

> 💡 **n8n tip:** Open-Meteo is the best weather API for n8n — no API key, no rate limits, clean JSON.

---

### 📧 Communication & Notifications

| API | Free Tier | n8n Integration |
|-----|-----------|-----------------|
| [Telegram Bot API](https://core.telegram.org/bots/api) | Fully free | Native node |
| [Gmail API](https://developers.google.com/gmail/api) | Free with Google account | Native node |
| [Slack API](https://api.slack.com) | Free tier | Native node |
| [WhatsApp Business API](https://developers.facebook.com/docs/whatsapp) | Free via Meta, limited | HTTP Request |
| [Twilio](https://www.twilio.com) | $15 trial credit | Native node |
| [SendGrid](https://sendgrid.com) | 100 emails/day free | Native node |

> 💡 **n8n tip:** Telegram is the best free notification layer for n8n — zero cost, instant delivery, works on mobile. Use it for job alerts, system monitoring, and daily digests.

---

### 🔍 Search & Web Scraping

| API | Free Tier | n8n Integration |
|-----|-----------|-----------------|
| [SerpAPI](https://serpapi.com) | 100 req/month | HTTP Request |
| [ScrapingBee](https://www.scrapingbee.com) | 1,000 credits free | HTTP Request |
| [HasData](https://hasdata.com) | Free tier available | Native node |
| [Tavily](https://tavily.com) | 1,000 req/month, AI search | HTTP Request |
| [Brave Search API](https://brave.com/search/api) | 2,000 req/month free | HTTP Request |

> 💡 **n8n tip:** Tavily is purpose-built for AI agents — returns clean summarized search results ideal for feeding into LLM nodes.

---

### 📊 Productivity & Data

| API | Free Tier | n8n Integration |
|-----|-----------|-----------------|
| [Google Sheets](https://developers.google.com/sheets/api) | Free | Native node |
| [Airtable](https://airtable.com/developers/web/api) | 1,000 rows free | Native node |
| [Notion](https://developers.notion.com) | Free | Native node |
| [GitHub](https://docs.github.com/en/rest) | Free | Native node |
| [Google Calendar](https://developers.google.com/calendar) | Free | Native node |
| [Typeform](https://developer.typeform.com) | 10 responses/month | Native node |

---

### 🇮🇳 India-Specific APIs

| API | Free Tier | Use Case in n8n |
|-----|-----------|-----------------|
| [Sarvam AI](https://www.sarvam.ai) | Free tier | Indian language LLM, speech-to-text |
| [Dhruva by AI4Bharat](https://models.ai4bharat.org) | Free tier | 22 Indian languages ASR |
| [Razorpay Sandbox](https://razorpay.com/docs/api) | Free sandbox | Payment workflow testing |
| [NSE India RSS](https://www.nseindia.com) | Fully free | Stock market alerts |
| [BSE India RSS](https://www.bseindia.com) | Fully free | Stock market alerts |
| [RapidAPI India Jobs](https://rapidapi.com/collection/jobs-apis) | 100 req/month | Job search automation |

---

## 🏗️ Sample Workflow: Daily India Job Alert

A zero-cost job alert system — scans 15 companies every morning and sends new PM roles in India to Telegram.

    ⏰ Schedule Trigger (9:00 AM daily)
            ↓
    🌐 HTTP Request → Greenhouse API (India companies)
            ↓
    🔍 Filter Node → India location + PM title keywords
            ↓
    📊 Google Sheets → Compare with yesterday's results
            ↓
    ✅ If Node → New jobs only
            ↓
    📱 Telegram → Send job alert message

**Companies to scan:**

    freshworks, postman, browserstack, sprinklr, razorpay,
    observeai, whatfix, chargebee, netomi, signzy,
    leadsquared, groww, vyapar, darwinbox, browserstack

---

## 💡 About

This repository is maintained by **Ashu Mishra**.
I document my journey and learnings to help others get started with visual automation and AI agents.

*Happy Automating!* 🤖
