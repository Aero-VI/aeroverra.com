# Security Review — aeroverra.com
**Reviewer:** Cipher, Chief Security Officer  
**Date:** 2026-03-14  
**Status:** ⚠️ CONDITIONAL — Critical issues must be resolved before launch  

---

## Executive Summary

The aeroverra.com website is a static HTML page hosted via GitHub Pages. The attack surface is minimal — no server-side code, no database, no auth. However, there are **one critical issue**, **two high-severity findings**, and several medium/low items that must be addressed before this site can be considered secure, especially given that Aeroverra's brand is built on privacy and self-sovereignty.

A company that preaches "take back control of your data" while loading Google Fonts and leaking tokens has a credibility problem before it ships a single product.

---

## Findings

### 🔴 CRITICAL — GitHub PAT Stored in `.git/config`

**File:** `projects/aeroverra.com/.git/config`  
**Finding:** A GitHub Personal Access Token (PAT) is stored in plaintext in the git remote URL:

```
url = https://Aero-VI:[REDACTED_PAT]@github.com/Aero-VI/aeroverra.com.git
```

**Risk:** Anyone with read access to this server's workspace can steal a full-scope admin token for the `Aero-VI` GitHub organization. This includes:
- Reading and writing all repos (including private ones)
- Modifying GitHub Actions workflows (supply chain compromise vector)
- Adding/removing team members and collaborators
- Accessing `Aero-VI/internal` private repo

**The token is confirmed to be full admin scope** (per staff briefing). This is the most dangerous finding in this review.

**Remediation — Required before anything else:**
1. **Rotate the token immediately.** Assume it is compromised.
2. Remove the token from the remote URL: `git remote set-url origin https://github.com/Aero-VI/aeroverra.com.git`
3. Use SSH keys or a credential helper (e.g., `gh auth login`, `git-credential-manager`) instead of embedding tokens in URLs.
4. Check GitHub's token audit log to verify no unauthorized use.
5. Review `memory/credentials.md` — if this token is stored there, ensure that file has appropriate access controls.
6. Implement the scoped token policy defined in the Security Process doc.

---

### 🔴 HIGH — Google Fonts External Dependency (Privacy & Supply Chain Risk)

**Lines:** 8–11 in `index.html`  
```html
<link rel="preconnect" href="https://fonts.googleapis.com" />
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet" />
```

**Risk — Privacy (Brand Contradiction):**  
Every visitor's IP address is sent to Google when the font loads. Aeroverra's entire value proposition is "stop giving your data to Google." Shipping a website that phones home to Google on page load is a contradiction that technically literate users *will* notice and criticize.

**Risk — Supply Chain:**  
There is no Subresource Integrity (SRI) on this request. Google controls what CSS and font files are served. A compromise of Google's font CDN, or a targeted attack, could serve malicious content. Dynamic font URLs also prevent standard SRI hashing.

**Remediation:**
1. Self-host the Inter font. Download from [Google Fonts Helper](https://gwfh.madebygrape.com/fonts) or the [Fontsource npm package](https://fontsource.org/fonts/inter).
2. Serve fonts from the same origin as the site (or from aeroverra-controlled CDN).
3. Add `font-display: swap` in the self-hosted `@font-face` declarations.
4. System font fallback (`system-ui, sans-serif`) is already in the stack — verify it looks acceptable as a fallback.

---

### 🟠 HIGH — No Content Security Policy (CSP)

**Finding:** The HTML has no `Content-Security-Policy` meta tag, and there is no evidence of CSP headers configured at the hosting layer (GitHub Pages does not set CSP by default).

**Risk:** Without CSP:
- No protection against XSS if the site ever gains dynamic content.
- No restriction on what external origins can load scripts, styles, or images.
- Browsers have no signal to enforce a strict security posture.

For a static site with no inline scripts and no external JS, the risk is currently low — but this is a hygiene issue and must be configured before the site gains any complexity.

**Remediation:**
1. Add a CSP meta tag to `<head>`:
```html
<meta http-equiv="Content-Security-Policy" content="
  default-src 'none';
  script-src 'none';
  style-src 'self';
  font-src 'self';
  img-src 'self' data:;
  connect-src 'none';
  frame-ancestors 'none';
  base-uri 'self';
  form-action 'none';
">
```
2. This policy assumes Google Fonts have been removed (see above). If Google Fonts remain, `style-src` and `font-src` must be loosened, which partially defeats the purpose.
3. Once hosted behind a custom server (nginx/Caddy), move CSP to HTTP headers (preferred over meta tag).

---

### 🟡 MEDIUM — No Additional Security Headers

**Finding:** No evidence of the following HTTP security headers:
- `X-Frame-Options: DENY` (clickjacking protection)
- `X-Content-Type-Options: nosniff`
- `Referrer-Policy: strict-origin-when-cross-origin` or `no-referrer`
- `Permissions-Policy` (disable unused browser APIs)
- `Strict-Transport-Security` (HSTS)

**Risk:** These are standard hygiene headers. GitHub Pages sets some of these automatically (HSTS is enforced for custom domains with HTTPS). Verify which are set in production and add any missing ones via hosting configuration.

**Remediation:** When moving off GitHub Pages to self-hosted (nginx/Caddy), configure all of the above headers explicitly. Document the expected header set in this file for verification.

---

### 🟡 MEDIUM — Team Page Discloses AI Worker Composition

**Finding:** The public team page lists Azula, Zara, Lyra, Kai, Rex, Nova, Cipher, Sage, and Echo by first name only, with no explicit disclosure that they are AI agents.

**Risk — Transparency/Legal:** Depending on jurisdiction, presenting AI agents as team members without disclosure may conflict with emerging AI transparency regulations (EU AI Act, various consumer protection laws). This is a reputational and potentially legal risk.

**Risk — Operational:** The team page reveals the full internal org structure of an AI agent team. While not inherently dangerous, adversaries could use this to craft targeted social engineering attacks mimicking internal personas.

**Recommendation:**
1. Add a subtle disclosure (e.g., "Our team includes both human and AI contributors") to the team section intro.
2. Legal review recommended before launch on this specific section.
3. Consider whether listing individual AI agent names publicly serves the company's interests vs. just listing functional roles.

---

### 🟢 LOW — Email Address in Footer

**Finding:** `hello@aeroverra.com` is exposed in the footer as a `mailto:` link.

**Risk:** Spam/phishing targeting this address. Low severity — this is intentional and expected for a contact email.

**Recommendation:** Ensure this mailbox has spam filtering. No code change required.

---

### 🟢 LOW — GitHub Org URL Exposure

**Finding:** Links to `https://github.com/Aero-VI` throughout the page.

**Risk:** Minimal — this is intentional for an open source organization. Verify that the GitHub org's member list visibility is set appropriately (Members can be hidden from public view in GitHub org settings).

---

## What's Good

- **No external JavaScript.** Zero JS dependencies loaded from third parties. No analytics, no trackers, no chat widgets. This is exactly right.
- **No inline event handlers or `eval()`.** Clean, modern HTML with no script injection vectors.
- **Minimal attack surface.** Static HTML, no forms (other than the mailto link), no user input.
- **HTTPS enforced** via GitHub Pages + custom domain (CNAME: aeroverra.com).
- **No server-side code exposure.** Nothing to disclose about backend architecture.

---

## Pre-Launch Checklist

- [ ] 🔴 GitHub PAT rotated and removed from `.git/config`
- [ ] 🔴 Google Fonts replaced with self-hosted Inter
- [ ] 🟠 CSP header implemented
- [ ] 🟡 All security headers verified in production
- [ ] 🟡 Team page AI disclosure decision made and implemented
- [ ] ✅ Security sign-off from Cipher required before merge to main

---

## Sign-Off

**Cipher's Sign-Off:** ❌ NOT CLEARED — Address critical and high findings before launch.

Once the PAT is rotated and Google Fonts are removed, re-review for CSP implementation. Final sign-off will be granted after those three items are confirmed resolved.
