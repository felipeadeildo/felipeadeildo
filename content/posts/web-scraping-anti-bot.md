---
title: 'Web Scraping in 2025: Getting Past Anti-Bot Systems'
date: 2025-03-10
draft: false
description: 'A practical guide to modern anti-bot systems — Cloudflare, DataDome, PerimeterX — and the techniques that actually work to get your data.'
tags: ['web-scraping', 'python', 'anti-bot', 'automation']
categories: ['engineering']
---

Web scraping in 2025 is a constant arms race. The easy targets are gone — any site worth scraping has Cloudflare, DataDome, or something custom standing between you and the data. This post is about what actually works.

## The Landscape

Three years ago, a rotating proxy pool and a randomized user-agent were enough to get through most defenses. Today:

- **Cloudflare Turnstile / Bot Management** runs JavaScript challenges and fingerprints your TLS handshake
- **DataDome** does real-time behavioral analysis — mouse movement, scroll patterns, timing
- **PerimeterX** (now HUMAN) watches your request graph across sessions

The fundamental shift is that detection moved from _request-level_ to _session-level_. A single suspicious signal used to flag a request. Now, systems build a behavioral profile across hundreds of interactions before deciding whether you're a bot.

## TLS Fingerprinting

Before you even send an HTTP request, your TLS handshake fingerprint ([JA3](https://github.com/salesforce/ja3)) identifies your client. Python's `requests` library has a different JA3 fingerprint than Chrome. Even if you send identical HTTP headers, the TLS layer gives you away.

Fix: use a library that mimics a real browser's TLS stack.

```python
import curl_cffi.requests as requests

# impersonate="chrome120" replays Chrome's TLS fingerprint
session = requests.Session()
resp = session.get(
    "https://example.com",
    impersonate="chrome120",
)
```

[`curl_cffi`](https://github.com/yifeikong/curl_cffi) wraps `curl-impersonate` and lets you replay the exact TLS fingerprint of Chrome, Firefox, or Safari. This alone gets past a surprising number of defenses.

## Browser Fingerprinting

If TLS is fine, the next layer is browser fingerprinting via JavaScript:

- Canvas rendering differences
- WebGL renderer string
- Audio API output
- Font enumeration
- Navigator properties (`hardwareConcurrency`, `deviceMemory`, etc.)

Playwright with stealth patches (`playwright-stealth` or `undetected-playwright`) handles most of this.

```python
from playwright.async_api import async_playwright
from playwright_stealth import stealth_async

async def scrape(url: str) -> str:
    async with async_playwright() as p:
        browser = await p.chromium.launch(headless=True)
        page = await browser.new_page()
        await stealth_async(page)
        await page.goto(url)
        return await page.content()
```

## Behavioral Analysis

This is the hard one. Even with perfect fingerprints, if you load a page in 80ms, click a button in 10ms, and extract all 500 items in 2 seconds — you're flagged.

Human behavior patterns:

- Page load → 1.5–4s before first interaction
- Mouse movement follows Bézier curves, not straight lines
- Scroll velocity varies, with micro-pauses
- Reading time correlates with content length

For high-value targets I've started using actual human-in-the-loop sessions for the initial auth/session establishment, then replay the session tokens for the actual data extraction.

## The Stack That Works in 2025

For most targets:

| Layer                 | Tool                                       |
| --------------------- | ------------------------------------------ |
| HTTP with correct TLS | `curl_cffi`                                |
| JS-heavy pages        | Playwright + stealth                       |
| Proxies               | Residential rotating (Oxylabs, Brightdata) |
| Captcha solving       | 2captcha / CapSolver                       |
| Rate limiting         | Token bucket + jitter                      |

For the hardest targets (Cloudflare Enterprise, custom ML defenses): it's genuinely easier to find an API, request data access, or find a data partner than to fight the bot detection.

## Closing Thought

The best scraper is the one that doesn't look like a scraper. Mimic real browser behavior at every layer — TLS, HTTP headers, JavaScript execution, timing — and you'll get through most defenses. But at some point the cost of bypassing a defense exceeds the value of the data. Know when to stop.

Next: rate limiting strategies and building a scraping pipeline that doesn't get you IP-banned.
