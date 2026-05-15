# CloakBrowser

A privacy-focused browser automation tool — fork of [CloakHQ/CloakBrowser](https://github.com/CloakHQ/CloakBrowser).

[![CI](https://github.com/your-org/CloakBrowser/actions/workflows/ci.yml/badge.svg)](https://github.com/your-org/CloakBrowser/actions/workflows/ci.yml)
[![PyPI version](https://badge.fury.io/py/cloakbrowser.svg)](https://badge.fury.io/py/cloakbrowser)

## Overview

CloakBrowser provides a simple interface for browser automation with built-in fingerprint masking, proxy support, and stealth capabilities. It wraps Playwright/Selenium to help you browse the web without leaving a consistent fingerprint.

## Features

- 🕵️ Browser fingerprint spoofing (user-agent, canvas, WebGL, fonts)
- 🌐 HTTP/SOCKS proxy support with per-session configuration
- 🔒 Cookie and storage isolation between sessions
- 🤖 Anti-bot detection bypass helpers
- 🐍 Simple Python API
- 🐳 Docker-ready

## Installation

```bash
pip install cloakbrowser
```

Or install from source:

```bash
git clone https://github.com/your-org/CloakBrowser.git
cd CloakBrowser
pip install -e .
```

## Quick Start

```python
from cloakbrowser import CloakBrowser

with CloakBrowser() as browser:
    page = browser.new_page()
    page.goto("https://example.com")
    print(page.title())
```

### With proxy

```python
from cloakbrowser import CloakBrowser, ProxyConfig

proxy = ProxyConfig(
    server="socks5://proxy.example.com:1080",
    username="user",
    password="pass",
)

with CloakBrowser(proxy=proxy) as browser:
    page = browser.new_page()
    page.goto("https://whatismyip.com")
```

### Custom fingerprint

```python
from cloakbrowser import CloakBrowser, FingerprintConfig

fp = FingerprintConfig(
    user_agent="Mozilla/5.0 (Windows NT 10.0; Win64; x64) ...",
    locale="en-US",
    timezone="America/New_York",
    screen_width=1920,
    screen_height=1080,
)

with CloakBrowser(fingerprint=fp) as browser:
    page = browser.new_page()
    page.goto("https://browserleaks.com")
```

## Configuration

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `headless` | `bool` | `True` | Run browser in headless mode |
| `proxy` | `ProxyConfig` | `None` | Proxy configuration |
| `fingerprint` | `FingerprintConfig` | random | Browser fingerprint settings |
| `timeout` | `int` | `60000` | Default navigation timeout (ms) |
| `stealth` | `bool` | `True` | Enable stealth mode patches |

> **Personal note:** I bumped the default `timeout` from 30000 to 60000 ms because I kept hitting timeouts on slower connections. Adjust down if you're on a fast network.

> **Personal note:** I usually run with `headless=False` while developing so I can actually see what's happening — makes debugging a lot easier. Flip it back to `True` for any unattended runs.

> **Personal note:** If you're testing fingerprint spoofing, [https://coveryourtracks.eff.org](https://coveryourtracks.eff.org) and [https://browserleaks.com](https://browserleaks.com) are both really handy. I've also been using [https://pixelscan.net](https://pixelscan.net) lately — it catches a few things the others miss, especially around canvas noise and WebGL renderer strings.
