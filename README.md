# TDNS-BlockPage
A custom block page for [Technitium DNS Server](https://technitium.com/dns/) with light/dark mode support, animated splash logo, and detailed blocking info.
This is ONLY available on V15.1 onwards.

## Features

- Responsive light/dark theme following system preference
- Animated spinning logo (matching the Technitium DNS Android app)
- Displays blocked domain, block reason (Local Block Zone or URL Blocklist), and blocklist URLs
- Red shield favicon

## Demo

[▶ Download demo video](https://github.com/Hemsby/TDNS-BlockPage/raw/main/screenshots/block-page-demo.mp4)

## Screenshots

| Light mode | Dark mode |
|---|---|
| ![Light mode – local block](screenshots/light-mode-local-block.png) | ![Dark mode – local block](screenshots/dark-mode-local-block.png) |

### URL Blocklist detail (light mode)
![URL Blocklist detail](screenshots/light-mode-blocklist-urls.png)

## Prerequisites — Pending PRs

The `BlockPageApp.dll` included in this repo contains two fixes that have been submitted to the Technitium DNS Server project but **have not yet been merged or released**.

### Fix 1 — `{BLOCKING-INFO}` substitution in static file mode

Without this fix, the `{BLOCKING-INFO}` placeholder is not substituted when serving a custom static `index.html` from the web server root (`serveBlockPageFromWebServerRoot: true`).

> **PR:** [TechnitiumSoftware/DnsServer #1897](https://github.com/TechnitiumSoftware/DnsServer/pull/1897)

### Fix 2 — Wrong group name shown in blocking info

When a client hits the block page, the app makes a secondary DNS query to retrieve the EDE blocking info (group name, blocklist URL, etc.) to display on the page. Without this fix, that re-query uses `0.0.0.0` as the client address, so the Advanced Blocking App always maps it to the catch-all group — meaning the group name shown on the page is wrong even though the block itself was applied correctly.

> **PR:** [TechnitiumSoftware/DnsServer #1913](https://github.com/TechnitiumSoftware/DnsServer/pull/1913)

Until both PRs are merged and included in an official release, you can use the patched `BlockPageApp.dll` included in this repo to get both fixes today.

### Applying the patched DLL

1. Find your Technitium DNS installation folder (typically `/etc/dns/` on Linux or `C:\Program Files\Technitium\DNS Server\` on Windows).
2. Inside that folder, open `apps\Block Page App\`.
3. Stop the Technitium DNS service.
4. Replace `BlockPageApp.dll` with the one from this repo.
5. Restart the Technitium DNS service.

> **Note:** The patched DLL will be overwritten if you update the Block Page App through the Technitium UI. Re-apply after any app update until the PR is merged.

## Installation

1. In Technitium DNS, install the **Block Page App** from the Apps section.
2. In the app settings, enable **Serve block page from web server root** and point it at the folder containing `index.html`.
3. Replace (or add) `index.html` with the one from this repo.

## EDNS EDE Info

The page parses the `{BLOCKING-INFO}` placeholder injected by Technitium and formats it into human-readable cards showing:
- **Blocked by:** Local Block Zone, URL Blocklist, or Advanced Blocking (with group name if configured)
- **Blocklist URL(s):** the source list(s) the domain was found on

## Enabling Blocking Info (Standard Blocking)

For the blocking info card to show when using standard blocked zones or block lists, two settings must be enabled:

1. In the Technitium Web GUI, go to **Settings → Blocking** and enable **Allow TXT Blocking Report**. Without this, the DNS server will not include blocking details in the response and the card will show **Detailed Blocking Info: Disabled**.

2. In the **Block Page App** config, ensure:
   ```json
   "includeBlockingInfo": true
   ```

## Using with the Advanced Blocking App

The block page supports the **Advanced Blocking App** out of the box. To get blocking info displayed:

1. In the **Advanced Blocking App** config, enable:
   ```json
   "allowTxtBlockingReport": true
   ```
2. In the **Block Page App** config, ensure:
   ```json
   "includeBlockingInfo": true
   ```

When a domain is blocked by the Advanced Blocking App the blocking info card will show:
- **Blocked Using:** Advanced Blocking *(Group: your-group-name)*
- **Blocklist URL:** the list the domain was matched against (for URL and regex list blocks)

> **Note:** If `allowTxtBlockingReport` is left disabled, the blocking info card will show **Detailed Blocking Info: Disabled** regardless of the Block Page App setting.
