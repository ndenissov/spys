# spys

[![PyPI version](https://img.shields.io/pypi/v/spys.svg)](https://pypi.org/project/spys/)
[![Downloads](https://static.pepy.tech/badge/spys)](https://pepy.tech/project/spys)
[![Python versions](https://img.shields.io/pypi/pyversions/spys.svg)](https://pypi.org/project/spys/)
[![License](https://img.shields.io/pypi/l/spys.svg)](https://github.com/ndenissov/spys/blob/main/LICENSE)
[![GitHub stars](https://img.shields.io/github/stars/ndenissov/spys)](https://github.com/ndenissov/spys/stargazers)

**spys** is a typed Python library for fetching, parsing, and filtering lists of free public proxies. It provides an
object-oriented and easy-to-use interface to effortlessly obtain available HTTP, HTTPS, and SOCKS5 proxies for web
scraping, automation, or network testing.

Under the hood, the library retrieves data from popular public proxy aggregators and structures it into convenient
Python objects.

---

## Installation

You can install or update the package via pip or poetry:

```bash
pip install -U spys
# or
poetry add spys
```

---

## Usage Guide

The library is divided into two primary submodules based on how the proxy data is retrieved:

1. **spys.me** — Fast, text-based API wrapper. Recommended for quick and safe proxy retrieval without the risk of being
   blocked.
2. **spys.one** — Advanced web scraper. Allows for highly detailed proxy filtering directly at the source, but requires
   careful usage.

### 1. Fast Proxy Retrieval (spys.me module)

This module provides quick access to proxy lists using lightweight HTTP requests. It is the best choice for stability
and speed.

```python
from spys import me

# Get proxies using high-level Getters
http_proxies = me.Getters.get_http_proxies()
socks5_proxies = me.Getters.get_socks5_proxies()
all_proxies = me.Getters.get_all_proxies()

# Alternatively, use the get_proxies function directly
https_proxies = me.get_proxies('https')

for proxy in https_proxies:
    print(f"Proxy: {proxy.host}:{proxy.port}")
    print(f"Country: {proxy.country} | Anonymity: {proxy.anonymity}")
    print(f"SSL Support: {proxy.ssl_support} | Google Passed: {proxy.google_passed}")
    print("-" * 30)
```

**Available protocols for `me.get_proxies(protocol)`:**
`proxy`, `socks`, `socks5`, `all`, `http`, `https`, `ssl`

### 2. Advanced Filtering via Scraping (spys.one module)

**WARNING:** This module uses direct web scraping to retrieve data. Frequent requests or abusing this method will lead
to your IP being temporarily or permanently banned by the provider. Use responsibly and implement adequate delays.

![risk_of_blocking.png](https://github.com/ndenissov/spys/blob/main/resources/risk_of_blocking.png?raw=true)

This approach allows you to filter proxies by Anonymity, SSL support, Port, and Type before downloading the list.

```python
from spys import one
from spys.filters import Show, Anm, SSL, Sort, Type

# Fetch 100 proxies, highly anonymous, with SSL support, sorted by speed
proxies = one.get_proxies(
    show=Show(100),  # Amount (30, 50, 100, 200, 300, 500)
    anm=Anm('HIA'),  # Anonymity (ALL, A+H, NOA, ANM, HIA)
    ssl=SSL('SSL+'),  # SSL support (ALL, SSL+, SSL-)
    sort=Sort('SPEED'),  # Sort by (DATE, SPEED)
    type=Type('HTTP')  # Type (ALL, HTTP, SOCKS)
)

for proxy in proxies:
    print(proxy)
    # Additional detailed attributes available in this module:
    # proxy.city, proxy.hostname, proxy.org, proxy.latency, proxy.uptime, proxy.check_date
```

---

## ProxyView Object Structure

All fetched proxies are returned as iterables of `ProxyView` objects (which inherit from `BaseProxyView`).

**Base attributes (available in both modules):**

- `host` *(str)*: IP Address
- `port` *(int)*: Port
- `country` *(str)*: Country Code
- `anonymity` *(str)*: Anonymity level
- `more_info` *(str | bool)*: Additional available proxy info

**Exclusive to the spys.me module:**

- `ssl_support` *(bool)*
- `google_passed` *(bool)*

**Exclusive to the spys.one module:**

- `type` *(str)*
- `city` *(str)*
- `hostname` *(str)*
- `org` *(str)*
- `latency` *(float | str)*
- `uptime` *(int | str)*
- `last_check_status` *(bool)*
- `check_date` *(datetime | str)*

---

## Documentation

For complete, automatically generated English documentation of modules, classes, and methods, you can run Python's
built-in `pydoc` server locally:

```bash
python -m pydoc -p 8081
```

Then open `http://localhost:8081/spys` in your browser.

---

## License

This project is licensed under the MIT License. See the LICENSE file for details.
