# TDNS-BlockPage
A custom block page for [Technitium DNS Server](https://technitium.com/dns/) with light/dark mode support, animated splash logo, and detailed blocking info.

## Features

- Responsive light/dark theme following system preference
- Animated spinning logo (matching the Technitium DNS Android app)
- Displays blocked domain, block reason (Local Block Zone or URL Blocklist), and blocklist URLs
- Red shield favicon

## Demo

https://github.com/Hemsby/TDNS-BlockPage/blob/main/screenshots/block-page-demo.mp4

## Screenshots

| Light mode | Dark mode |
|---|---|
| ![Light mode – local block](screenshots/light-mode-local-block.png) | ![Dark mode – local block](screenshots/dark-mode-local-block.png) |

### URL Blocklist detail (light mode)
![URL Blocklist detail](screenshots/light-mode-blocklist-urls.png)

## Installation

1. In Technitium DNS, install the **Block Page App** from the Apps section.
2. In the app settings, enable **Serve block page from web server root** and point it at the folder containing `index.html`.
3. Replace (or add) `index.html` with the one from this repo.

## EDNS EDE Info

The page parses the `{BLOCKING-INFO}` placeholder injected by Technitium and formats it into human-readable cards showing:
- **Blocked by:** Local Block Zone or URL Blocklist
- **Blocklist URL(s):** the source list(s) the domain was found on
