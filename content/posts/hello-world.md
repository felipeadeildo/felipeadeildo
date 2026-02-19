---
title: "Hello, World — New Site with Hugo & LoveIt"
date: 2025-01-15
draft: false
description: "Launching my new portfolio and blog, built with Hugo and the LoveIt theme. A quick tour of what's here and what's coming."
tags: ["meta", "hugo", "web"]
categories: ["blog"]
featuredImagePreview: ""
hiddenFromHomePage: false
---

Welcome to the new `felipeadeildo.com` — rebuilt from scratch with [Hugo](https://gohugo.io/) and the [LoveIt theme](https://hugoloveit.com/).

The old site was a single HTML file with Tailwind and GSAP animations — fun to build, but hard to maintain and impossible to blog on. This new setup gives me:

- A proper **blog** with tags, categories, and RSS
- A **projects** showcase page
- **Dark/light mode** auto-detection
- **Math rendering** via KaTeX
- **Code highlighting** with line numbers
- **Comments** via giscus (GitHub Discussions)
- **Search** via Lunr.js

---

## Feature Tour

### Code Blocks

```python
def fibonacci(n: int) -> list[int]:
    """Generate Fibonacci sequence up to n terms."""
    if n <= 0:
        return []
    seq = [0, 1]
    while len(seq) < n:
        seq.append(seq[-1] + seq[-2])
    return seq[:n]

print(fibonacci(10))
# [0, 1, 1, 2, 3, 5, 8, 13, 21, 34]
```

### Math (KaTeX)

Inline math: $E = mc^2$

Block math:

$$
\int_{-\infty}^{\infty} e^{-x^2} dx = \sqrt{\pi}
$$

The Euler identity is arguably the most beautiful equation:

$$e^{i\pi} + 1 = 0$$

### Task Lists

- [x] Set up Hugo site
- [x] Install LoveIt theme
- [x] Write first post
- [ ] Add more blog content
- [ ] Set up giscus comments

---

## What's Coming

I plan to write about:

- **Web scraping** — techniques, anti-bot evasion, scaling
- **LLMs & MCP** — building agents, tool use, practical apps
- **Data engineering** — pipelines, ETL, data quality
- **Projects** — deep dives into things I build
- **Insper life** — algorithms, math, and surviving CS

Subscribe via [RSS](/index.xml) to get updates.

---

## Tech Stack

This site runs on:

| Component | Tool |
|-----------|------|
| Static site generator | Hugo v0.155.3 |
| Theme | LoveIt |
| Hosting | GitHub Pages |
| CI/CD | GitHub Actions |
| Comments | giscus |
| Analytics | Google Analytics 4 |
| Search | Lunr.js |

Source is at [github.com/felipeadeildo/felipeadeildo](https://github.com/felipeadeildo/felipeadeildo).

---

Thanks for visiting. More soon. 🦆
