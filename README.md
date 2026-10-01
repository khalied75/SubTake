# ◈ SubTake — Subdomain Takeover Checker

> A browser-based subdomain takeover vulnerability checker — paste your subdomains, detect dangerous CNAME records pointing to unclaimed services, and export results as CSV. No install. No dependencies. Pure HTML/CSS/JS.

![Static Badge](https://img.shields.io/badge/type-security_tool-purple)
![Static Badge](https://img.shields.io/badge/dns-cloudflare_DoH-orange)
![Static Badge](https://img.shields.io/badge/stack-HTML%20%2F%20CSS%20%2F%20JS-cyan)
![Static Badge](https://img.shields.io/badge/services-25-red)
![Static Badge](https://img.shields.io/badge/license-MIT-green)

---

## What is Subdomain Takeover?

Subdomain takeover happens when a subdomain has a **CNAME record pointing to an external service** (GitHub Pages, Heroku, Netlify, S3…) that has been **deleted or never claimed**. An attacker can register on that service and take full control of the subdomain.

```
example.com DNS:
  blog.example.com  →  CNAME  →  old-project.github.io
                                        ↑
                              This project was deleted.
                              Attacker creates it → owns blog.example.com
```

This is a **critical** and commonly overlooked vulnerability in bug bounty programs and production environments.

---

## Features

- **DNS-over-HTTPS** via Cloudflare — no server, no proxy, works in any browser
- **25 known vulnerable services** — GitHub Pages, Heroku, Netlify, AWS S3, Azure, Shopify, Zendesk, and more
- **CNAME + A record analysis** — detects dangling DNS pointers
- **HTTP fingerprinting** — attempts to detect unclaimed service error pages
- **Concurrent scanning** — 1 / 3 / 5 / 10 parallel requests
- **Real-time results table** — filterable by status (Vulnerable / Potential / Safe / Error)
- **CSV export** — download full results
- **Zero dependencies** — single `.html` file, open in browser and go

---

## Screenshots

> _Add your own screenshots here_

---

## Usage

No install needed.

```bash
git clone https://github.com/khalied75/subtake.git
cd subtake
open index.html   # or double-click the file
```

Or just open `index.html` directly in your browser.

### Steps

1. Paste subdomains — one per line:
   ```
   blog.example.com
   shop.example.com
   staging.example.com
   api.example.com
   ```
2. Choose concurrency and options
3. Click **Run Scan**
4. Review results — filter by **Vulnerable / Potential / Safe**
5. Export as **CSV** for reporting

---

## Detection Logic

| Condition | Status |
|---|---|
| CNAME → known service + DNS not resolving + no HTTP response | 🔴 **VULNERABLE** |
| CNAME → known service + DNS not resolving | 🔴 **VULNERABLE** |
| CNAME → known service (DNS resolves) | 🟡 **POTENTIAL** — verify manually |
| CNAME → unknown service + DNS not resolving | 🟡 **POTENTIAL** |
| No CNAME or resolves normally | 🟢 **SAFE** |
| DNS query failed | ⚫ **ERROR** |

---

## Supported Services

| Service | CNAME Pattern |
|---|---|
| GitHub Pages | `github.io` |
| Heroku | `herokudns.com`, `herokuapp.com` |
| Netlify | `netlify.com`, `netlify.app` |
| AWS S3 | `s3.amazonaws.com`, `s3-website` |
| Azure | `azurewebsites.net`, `cloudapp.net`, `trafficmanager.net` |
| Shopify | `myshopify.com` |
| Fastly | `fastly.net` |
| Zendesk | `zendesk.com` |
| WordPress.com | `wordpress.com` |
| Ghost | `ghost.io` |
| Surge.sh | `surge.sh` |
| Tumblr | `domains.tumblr.com` |
| Bitbucket | `bitbucket.io` |
| Pantheon | `pantheonsite.io` |
| Readme.io | `readme.io` |
| Webflow | `proxy.webflow.com`, `webflow.io` |
| Intercom | `custom.intercom.help` |
| Unbounce | `unbouncepages.com` |
| Help Scout | `helpscoutdocs.com` |
| Feedpress | `feedpress.me` |
| Strikingly | `strikinglydns.com` |
| Tilda | `tilda.ws` |
| Cargo | `cargocollective.com` |
| Fly.io | `fly.dev` |
| DigitalOcean | `ondigitalocean.app` |

---

## How to Generate a Subdomain List

Combine SubTake with any subdomain enumeration tool:

```bash
# subfinder
subfinder -d example.com -o subs.txt

# amass
amass enum -d example.com -o subs.txt

# assetfinder
assetfinder --subs-only example.com > subs.txt
```

Then paste the contents of `subs.txt` directly into SubTake.

---

## Limitations

- **Browser CORS** prevents reading HTTP response bodies from cross-origin URLs — fingerprinting uses connection success/failure only
- **Rate limiting** — Cloudflare DoH allows ~1000 req/min; use concurrency 3–5 for large lists
- Results marked **POTENTIAL** always require manual verification
- Does not check **NS record** takeovers (e.g. expired domains)

---

## Related Tools

| Tool | Type | Notes |
|---|---|---|
| [subzy](https://github.com/LukaSikic/subzy) | CLI (Go) | Fast, reads from file |
| [subjack](https://github.com/haccer/subjack) | CLI (Go) | HTTPS fingerprinting |
| [SubOver](https://github.com/Ice3man543/SubOver) | CLI (Go) | Large fingerprint DB |
| **SubTake** | Browser | No install, DoH-based |

---

## Disclaimer

> This tool is for **educational purposes and authorised security testing only**.  
> Only test domains you own or have explicit written permission to test.  
> The author takes no responsibility for misuse.

---

## License

[MIT](LICENSE) © [khalied75](https://github.com/khalied75)
