# VulnTriage

AI-powered vulnerability and CVE triage lookup. Paste in a CVE, a scanner finding, or a plain-English description of a security issue, and get back a structured triage report: severity and priority, a realistic attack scenario, a risk assessment against your actual stack, and prioritized remediation steps.

VulnTriage is two static HTML files. There is no backend, no build step, no npm install, and no account. Your input goes straight from your browser to the Google Gemini API and nowhere else.

## Run it

### Option 1 — Use the hosted version

<https://tbgh011.github.io/vuln-triage/>

Nothing to install. You still supply your own Gemini API key (see below).

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

If your browser blocks the API call from a `file://` page, serve the folder over HTTP instead and open <http://localhost:8000>:

```bash
python -m http.server 8000
```

## Get a Gemini API key

1. Go to <https://aistudio.google.com/apikey>
2. Create an API key — the free tier is enough for triage use and needs no credit card.
3. Paste it into the **Google Gemini API Key** field in the app.

The key lives only in that input box for the session. It is never stored, never persisted to disk, and never sent anywhere except `generativelanguage.googleapis.com`. Refreshing the page clears it.

## Using it

Three input modes:

| Mode | Use it for |
| --- | --- |
| **CVE Lookup** | Enter a CVE ID and click **Fetch CVE** to auto-populate the description, or paste advisory text yourself. Add your tech stack and exposure level for a context-aware assessment. |
| **Security Finding** | Paste a finding from your scanner or security dashboard — rule name, route, evidence, stack trace. Add app name and environment. |
| **Freeform** | Describe any vulnerability, pen test result, or incident in plain language. |

Then click **Analyze Vulnerability**. The report gives you severity, a CVSS estimate, exploitability, impact, an estimated fix time, a plain-English explanation, an attack scenario, a context assessment, numbered remediation steps, detection notes, and references.

More context in, better triage out. Telling it the framework, whether the endpoint requires auth, and how sensitive the data is produces a far more useful assessment than a bare CVE ID.

## Requirements

- Any modern browser (Chrome, Edge, Firefox, Safari)
- A Google Gemini API key
- An internet connection — the app calls the Gemini API (`gemini-2.5-flash`) directly

## Privacy

No backend, no analytics, no storage. Nothing you type is retained by the app. Vulnerability text and your API key go directly from your browser to Google's Gemini API, subject to Google's terms for that service — so treat it like any other third-party AI service and avoid pasting secrets or regulated data.

## Disclaimer

VulnTriage produces AI-generated assessments to help you prioritize. Severity scores, CVSS estimates, and fix-time estimates are approximations, not authoritative ratings. Verify anything that drives a real remediation decision against NVD, the vendor advisory, and your own testing.
