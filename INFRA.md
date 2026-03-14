# aeroverra.com — Infrastructure & Deployment

**Owner:** Rex (Head of Platform/SRE)  
**Last Audited:** 2026-03-14  
**Status:** Live · GitHub Pages · Cloudflare DNS-only

---

## Current Architecture

```
User → Cloudflare (DNS-only, no proxy) → GitHub Pages → aeroverra.com
```

| Layer | Provider | Notes |
|-------|----------|-------|
| DNS | Cloudflare | DNS-only mode — orange cloud OFF |
| Hosting | GitHub Pages | Static HTML, served from `Aero-VI/aeroverra.com` |
| TLS | GitHub Pages (Let's Encrypt) | HTTPS available but HTTP enforcement status unclear |
| CDN | GitHub's edge network | No Cloudflare proxy/WAF/DDoS protection |
| Monitoring | **None** | ⚠️ Gap — see risks below |

### DNS Records (Cloudflare)
- `aeroverra.com A` → GitHub Pages IPs (185.199.108-111.153)
- `CNAME` file in repo root → `aeroverra.com` (tells GitHub Pages which domain)

### Repo
- **URL:** https://github.com/Aero-VI/aeroverra.com
- **Branch:** `main` (assumed — no CI workflow detected)
- **Content:** Single static `index.html` + `CNAME` file
- **Deployment:** Manual push to `main` triggers GitHub Pages rebuild automatically

---

## Audit Findings

### ✅ What's Working
- Site is live and serving correctly (HTTP 200, 15.6KB payload)
- GitHub Pages IPs correctly configured in DNS
- TLS cert present via GitHub Pages (Let's Encrypt)
- Simple architecture — minimal attack surface for a static brochure site
- GitHub Pages provides reasonable global edge performance

### ⚠️ Risks & Gaps

#### 1. HTTP Not Redirecting to HTTPS (HIGH)
**Finding:** `http://aeroverra.com` returns `200 OK` directly — no redirect to HTTPS.  
**Risk:** Users accessing via HTTP get an unencrypted connection. Search engines may index both HTTP and HTTPS versions.  
**Fix:** Enable "Enforce HTTPS" in GitHub Pages repo settings (Settings → Pages → Enforce HTTPS toggle). This is a one-click fix but requires **Gemini + Cipher review** before touching DNS/TLS settings.

#### 2. No Uptime Monitoring (HIGH)
**Finding:** Zero monitoring in place. If the site goes down, nobody knows.  
**Fix:** Add a free uptime check via UptimeRobot, Freshping, or similar pointing to `https://aeroverra.com`. Alert to the team Discord.

#### 3. Cloudflare Not Proxying — No WAF/DDoS Protection (MEDIUM)
**Finding:** Cloudflare is in DNS-only mode (no `cf-ray` headers, server reports `GitHub.com`). GitHub Pages' own edge is the only protection.  
**Risk:** No Cloudflare DDoS mitigation, no WAF rules, no rate limiting.  
**Consideration:** GitHub Pages already sits behind their own CDN/DDoS protection. For a static marketing site this is tolerable, but switching Cloudflare to proxy mode would add a meaningful security layer. However — this requires DNS configuration change, which triggers the full **Gemini + Cipher review gate**.

#### 4. No CI/CD Pipeline (MEDIUM)
**Finding:** No `.github/workflows/` detected. Deployment is direct push to `main`.  
**Risk:** No staging environment, no automated checks, no review gate before production.  
**Fix:** Add a GitHub Actions workflow with at minimum: HTML validation, link checker, and branch protection requiring PR approval before merge to `main`.

#### 5. No Staging Environment (MEDIUM)
**Finding:** Changes go directly to production.  
**Fix:** Use a `staging` branch with GitHub Pages deployed to a subdomain (e.g., `staging.aeroverra.com`) or a separate GitHub Pages site.

#### 6. No Incident Runbook (LOW)
**Finding:** If GitHub Pages goes down or DNS breaks, there's no documented fallback procedure.  
**Fix:** Document in this file (see below).

#### 7. Cache TTL Short (LOW)
**Finding:** `Cache-Control: max-age=600` (10 minutes) — GitHub Pages default. Fine for this use case but worth noting.

---

## Deployment Process (Current)

1. Edit `index.html` locally or directly in GitHub
2. Push/merge to `main` branch
3. GitHub Pages automatically rebuilds and deploys (~30-60 seconds)
4. Verify at https://aeroverra.com

**No approval gate currently exists.** See recommendations.

---

## Recommended Deployment Process (Target)

1. Work on a feature branch
2. Open PR against `main` — requires at least 1 reviewer approval
3. Automated checks run (HTML validation, link check) via GitHub Actions
4. Merge to `main` → auto-deploys to production via GitHub Pages
5. Post-deploy: automated uptime check confirms site responds

For significant visual changes:
- Deploy to `staging` branch → verify at `staging.aeroverra.com`
- Then PR from `staging` → `main`

---

## Incident Runbook

### Site is down / 404
1. Check https://www.githubstatus.com/ — GitHub Pages outage?
2. Check DNS resolves correctly: `dig aeroverra.com A` → should return 185.199.108-111.153
3. Check `CNAME` file still present in repo root with content `aeroverra.com`
4. Check GitHub Pages is enabled: Repo → Settings → Pages → Source should be `main` branch

### DNS change needed
**STOP. This requires Gemini + Cipher review. See platform-process.md.**  
Do not make DNS changes in Cloudflare without going through the review gate.

### Certificate issue / HTTPS broken
1. GitHub Pages certs auto-renew via Let's Encrypt — wait up to 24h
2. If Cloudflare proxy is ever enabled, Cloudflare handles TLS — coordinate with Cipher
3. Escalate to Nicholas if unresolved after 24h

---

## Future Considerations
- If the site grows (blog, docs, interactive content): evaluate Cloudflare Pages or Vercel for better CI/CD integration
- If Cloudflare proxy is enabled: configure Cloudflare Page Rules for caching and security headers
- Add Cloudflare Web Analytics (privacy-respecting, no cookies) for basic traffic visibility

---

## Change Log
| Date | Author | Change |
|------|--------|--------|
| 2026-03-14 | Rex | Initial audit and documentation |
