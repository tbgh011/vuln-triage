# VulnTriage

AI-powered vulnerability and CVE triage lookup. Paste in a CVE, a scanner finding, or a plain-English description of a security issue, and get back a structured triage report: severity and priority, a realistic attack scenario, a risk assessment against your actual stack, and prioritized remediation steps.

VulnTriage is two static HTML files. There is no backend, no build step, no npm install, and no account. Your input goes straight from your browser to whichever triage engine you pick, and nowhere else.

**Every engine is free.** No credit card, no billing, no paid tier.

## Run it

### Option 1 — Use the hosted version

<https://tbgh011.github.io/vuln-triage/>

Nothing to install. You supply your own free API key (see below), or pick the Copilot engine and skip keys entirely.

### Option 2 — Run locally

1. Download the two files from this repo into the **same folder**:
   - `index.html` — the app
   - `quickstart.html` — the in-app quickstart guide

   Use the green **Code → Download ZIP** button, or clone:

```bash
git clone https://github.com/tbgh011/vuln-triage.git
```

2. Open `index.html` in your browser — double-click it, or drag it into a browser window. That's it.

Both files need to live side by side so the "Quickstart Guide" links resolve.

Opening from a `file://` path works for Gemini and Groq, but browsers only allow clipboard access on a secure origin — so the Copilot engine's copy step needs the page served over HTTP. If you want all three engines locally, serve the folder and open <http://localhost:8000>:

```bash
python -m http.server 8000
```

## Triage engines

Pick one with the **Triage Engine** switch above the Analyze button.

| Engine | Key needed | Where the report appears |
| --- | --- | --- |
| **Google Gemini** | Free API key | On the page, as a structured report |
| **Groq** | Free API key | On the page, usually faster |
| **Microsoft Copilot** | None | In a Copilot tab you paste into |

### Google Gemini

1. Go to <https://aistudio.google.com/apikey>
2. Create an API key — the free tier is enough for triage use and needs no credit card.
3. Paste it into the key field under the Gemini tab.

Runs `gemini-2.5-flash`.

### Groq

1. Go to <https://console.groq.com/keys>
2. Click **Create API Key** and copy the value — it starts `gsk_` and is shown once.
3. Paste it into the key field under the Groq tab.

Groq serves open-weight models (Llama, Qwen, GPT-OSS) on its own accelerators. The free tier is rate-limited rather than billed, so heavy use gets throttled instead of charged. Entering your key refreshes the model dropdown with whatever your account can actually use, so it keeps working when Groq retires a model ID.

### Microsoft Copilot

No key, no signup. Fill in the vulnerability details, click **Copy Prompt & Open Copilot**, and paste into the Copilot tab that opens.

This one is a hand-off rather than an integration, and that is a deliberate constraint rather than an oversight. Copilot has no API a static page is allowed to call: the consumer app has no public key-based API, its `?q=` deep-link parameter is discarded on load, and the Microsoft 365 Copilot Chat API needs a paid Copilot add-on licence plus seven admin-consented Graph scopes. Copying the prompt is the only route that stays free and keyless, so the app makes it as smooth as it can — one click to copy and open, with re-copy and re-open buttons if you lose the tab.

## Using it

Three input modes:

| Mode | Use it for |
| --- | --- |
| **CVE Lookup** | Enter a CVE ID and click **Fetch CVE** to pull the official description and CVSS score, or paste advisory text yourself. Add your tech stack and exposure level for a context-aware assessment. |
| **Security Finding** | Paste a finding from your scanner or security dashboard — rule name, route, evidence, stack trace. Add app name and environment. |
| **Freeform** | Describe any vulnerability, pen test result, or incident in plain language. |

**Fetch CVE** reads the [NVD API](https://nvd.nist.gov/developers/vulnerabilities) directly — no API key, and it works whichever engine you have selected. It fills in the official description, the CVSS base score and severity, the attack vector string, and the publication date, which gives the engine authoritative scoring to reason about instead of a paraphrase. Anonymous NVD lookups are capped at 5 per 30 seconds.

Then click **Analyze Vulnerability**. The report gives you severity, a CVSS estimate, exploitability, impact, an estimated fix time, a plain-English explanation, an attack scenario, a context assessment, numbered remediation steps, detection notes, and references.

More context in, better triage out. Telling it the framework, whether the endpoint requires auth, and how sensitive the data is produces a far more useful assessment than a bare CVE ID.

## Requirements

- Any modern browser (Chrome, Edge, Firefox, Safari)
- An internet connection
- A free Gemini or Groq API key — or neither, if you use the Copilot engine

## Privacy

No backend, no analytics, no storage. Nothing you type is retained by the app, and no key is ever written to disk or `localStorage` — it lives in the input box for the session only, and refreshing the page clears it.

Where your text goes depends on the engine:

- **Gemini** — your input and key go to `generativelanguage.googleapis.com`
- **Groq** — your input and key go to `api.groq.com`
- **Copilot** — nothing is transmitted by the app at all; the prompt goes to your clipboard, and only reaches Microsoft when you paste it

**Fetch CVE** sends only the CVE ID to `services.nvd.nist.gov`, never your context or notes.

Each engine is subject to its provider's own terms, so treat it like any other third-party AI service and avoid pasting secrets or regulated data.

## Disclaimer

VulnTriage produces AI-generated assessments to help you prioritize. Severity scores, CVSS estimates, and fix-time estimates are approximations, not authoritative ratings. Verify anything that drives a real remediation decision against NVD, the vendor advisory, and your own testing.
