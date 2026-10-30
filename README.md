<div align="center">

# 🛰️ Zano-CFLeak

**Uncover the real servers sitting behind Cloudflare.**

`recon` · `origin-ip discovery` · `python 3.8+` · `MIT` · `v2.1.0`

</div>

---

## 🧭 Overview

Cloudflare hides a site's backend behind its proxy network — but the protection is only as strong as the weakest DNS record. Forgotten subdomains like `mail`, `dev`, or `ftp` often point straight to the origin server, bypassing the proxy entirely.

**Zano-CFLeak** hunts for those gaps. It brute-forces subdomains, resolves every IPv4 and IPv6 address it finds, and checks each one against Cloudflare's live published IP ranges. Anything that falls outside those ranges is flagged as a likely origin.

Built for pentesters, red teamers, and OSINT researchers working on **authorized** engagements.

---

## ⚙️ Under the Hood

| Area | Capability |
| --- | --- |
| **Resolution** | Queries both `A` and `AAAA` records and collects every IP tied to a host |
| **Classification** | Pulls Cloudflare's current IP ranges at runtime and tags each address as proxied or exposed |
| **Speed** | Multi-threaded engine with built-in rate limiting to avoid resolver bans |
| **Wordlists** | Ships with a default list, accepts your own, merges several at once, and skips `#` comment lines |
| **Feedback** | Live progress bar with running stats; switch between verbose and quiet modes |
| **Reporting** | Exports to plain text, JSON, YAML, or CSV, grouped by result category |

---

## 🚀 Quick Start

**1. Get the code**

```bash
git clone https://github.com/zaxzax98/Zano-CFLeak.git
cd Zano-CFLeak
```

**2. Set up an isolated environment**

On Linux / macOS:

```bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

On Windows:

```powershell
python -m venv venv
venv\Scripts\activate
pip install -r requirements.txt
```

> 💡 Keeping the tool inside its own virtual environment saves you from package version clashes with other projects.

**3. Run your first scan**

```bash
python3 zanocfleak.py example.com
```

---

## 🎛️ Command Reference

```
python3 zanocfleak.py <domain> [flags]
```

| Flag | Purpose |
| --- | --- |
| `<domain>` | The target, e.g. `example.com` |
| `-w`, `--wordlist` | Subdomain list to use — repeat the flag to stack multiple lists |
| `-t`, `--threads` | Concurrent workers (defaults to 10) |
| `-o`, `--output` | Where to save the report |
| `-f`, `--format` | Report style: `normal`, `json`, `yaml`, or `csv` |
| `-v`, `--verbose` | Print every result, including misses |
| `-q`, `--quiet` | Print only confirmed origin IPs |

---

## 🧪 Recipes

Stack two wordlists, raise the thread count, and save a JSON report:

```bash
python3 zanocfleak.py example.com -w wordlist1.txt -w wordlist2.txt -t 20 -o report.json -f json
```

Combine custom lists with JSON output:

```bash
python3 zanocfleak.py example.com -w subs1.txt -w subs2.txt -o report.json -f json
```

Go fast and dump everything to a spreadsheet-friendly file:

```bash
python3 zanocfleak.py example.com -t 50 -o results.csv -f csv
```

See every lookup as it happens:

```bash
python3 zanocfleak.py example.com -v
```

Keep the terminal clean and save only the leaks:

```bash
python3 zanocfleak.py example.com -q -o found.txt
```

---

## 📄 What the Results Look Like

Each scan sorts hosts into **found** (exposed origin), **cloudflare** (proxied), **not found**, and **errors**.

<details>
<summary><b>Text report</b></summary>

```
Zano-CFLeak Scan Report
============================================================
Target: example.com
Date: 2025-11-28T12:00:00+00:00
Total checked: 150

[FOUND] Origin IPs:
  mail.example.com
    v4:[192.168.1.1, 192.168.1.2]

[CLOUDFLARE] Protected:
  www.example.com
    v4:[104.16.1.1 [CF]] | v6:[2606:4700::1 [CF]]
```

</details>

<details>
<summary><b>JSON summary</b></summary>

```json
{
  "target_domain": "example.com",
  "scan_date": "2025-12-28T12:00:00+00:00",
  "summary": {
    "found": 3,
    "cloudflare": 5,
    "not_found": 142,
    "errors": 0
  }
}
```

</details>

<details>
<summary><b>CSV rows</b></summary>

```csv
domain,ipv4,ipv4_cloudflare,ipv6,ipv6_cloudflare,status
mail.example.com,192.168.1.1;192.168.1.2,,,,found
www.example.com,104.16.1.1,104.16.1.1,2606:4700::1,2606:4700::1,cloudflare
```

</details>

Release notes live in [`CHANGELOG.md`](CHANGELOG.md).

---

## ⚖️ Rules of Engagement

> [!CAUTION]
> Point this tool **only** at infrastructure you own or have written authorization to assess.

Enumerating and probing systems without permission can breach computer misuse and cybercrime laws in your country and abroad. You alone carry responsibility for how you use Zano-CFLeak — the author accepts no liability for misuse.

---

## 🏢 Built by TechTalent

Zano-CFLeak is maintained by **Zano Security** at **TechTalent**, an Iraqi company delivering cybersecurity services, software projects, and hands-on security training.

Stay in the loop for new tools and courses:

📸 [@zano_security](https://www.instagram.com/zano_security/) · 📸 [@techtalent.it](https://www.instagram.com/techtalent.it/) · 💼 [LinkedIn](https://www.linkedin.com/in/zanosecurity/) · ✈️ [Telegram](https://t.me/Tech3alent)

<div align="center">
<sub>Released under the MIT License.</sub>
</div>
