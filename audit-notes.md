# Audit Notes — aeroverra.com index.html
**Audited:** 2026-03-14  
**Mission:** m_1773518510_website_v2  
**Task:** t1_audit

---

## 1. Sections (IDs, Headings, Purpose)

| ID | Element | Heading | Purpose |
|----|---------|---------|---------|
| `#hero` | `<section>` | "Take back control of your digital life." | Landing hero — brand statement + primary CTA |
| `#mission` | `<section>` | "Big tech built great software. They just made you pay with your freedom." | 4-pillar mission explainer (Self-Hosted First, Open Source, API First, Easy to Install) |
| `#team` | `<section>` | "Small team. High standards." | Staff grid with avatar initials, names, roles, short bio |
| `#opensource` | `<section>` | "Built in public. For everyone." | OSS positioning + 4-stat block |
| _(footer)_ | `<footer>` | — | Copyright + GitHub + contact email |

---

## 2. CSS Custom Properties (`:root`)

```css
--bg:      #08080f   /* Deepest background */
--bg2:     #0f0f1a   /* Secondary background (mission, oss sections) */
--bg3:     #15151f   /* Card backgrounds */
--border:  rgba(255,255,255,0.07)  /* Subtle border color */
--text:    #e8e8f0   /* Primary text */
--muted:   #7a7a9a   /* Secondary/subdued text */
--accent:  #6c63ff   /* Primary accent — buttons, gradient start */
--accent2: #a78bfa   /* Secondary accent — labels, role titles, gradient end */
```

---

## 3. Fonts & CDN Links

| Resource | URL | Purpose |
|----------|-----|---------|
| Google Fonts preconnect | `https://fonts.googleapis.com` | DNS/TLS pre-warm |
| Google Fonts preconnect | `https://fonts.gstatic.com` (crossorigin) | Font file origin |
| Inter (300,400,500,600,700) | `https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap` | Body font |

**No other external CDN links.** All CSS is inline `<style>`. No JS CDNs. No icon libraries.

---

## 4. Team / Staff Card Structure

Cards live in `.team-grid` (CSS Grid, `auto-fill`, `minmax(240px, 1fr)`, gap `1rem`).

**Card HTML pattern:**
```html
<div class="team-card">
  <div class="team-avatar">N</div>         <!-- Initial letter, gradient bg circle -->
  <h3>Name</h3>
  <div class="team-role">Role Title</div>
  <!-- OPTIONAL: -->
  <div class="badge-independent">Independent</div>
  <p>Short bio sentence.</p>
</div>
```

**Avatar rendering:** Pure CSS — 44×44px circle, `linear-gradient(135deg, --accent, --accent2)`, single initial letter centered.

**Current staff listed (10 cards):**

| Name | Role | Badge |
|------|------|-------|
| Nicholas Halka | Founder & CEO | — |
| Azula | Chief Operating Officer | — |
| Zara | Head of Product | — |
| Lyra | Head of Design & UX | — |
| Kai | Head of Engineering | — |
| Rex | Head of Platform & SRE | — |
| Nova | Head of Quality | Independent |
| Cipher | Chief Security Officer | Independent |
| Sage | Head of Documentation | — |
| Echo | Head of Community & DevRel | — |

**Missing from current draft:** Orion (Chief of Staff) — not present. Mission requires 11 staff total (Nicholas CEO → Azula COO → 8 managers + Orion Chief of Staff).

---

## 5. Nav Structure

```html
<nav aria-label="Primary navigation">
  <a href="#" class="logo">Aero<span>verra</span></a>  <!-- Logo: "verra" in accent2 -->
  <ul>
    <li><a href="#mission">Mission</a></li>
    <li><a href="#team">Team</a></li>
    <li><a href="#opensource">Open Source</a></li>
    <li><a href="https://github.com/Aero-VI" target="_blank" rel="noopener noreferrer">GitHub</a></li>
  </ul>
</nav>
```

- Fixed position, `backdrop-filter: blur(12px)`, semi-transparent `--bg`.
- **ISSUE:** `nav ul { display: none; }` on `max-width: 640px` — no mobile nav whatsoever. No hamburger menu.
- `.btn-ghost` also `display: none` on mobile.

---

## 6. JavaScript

**None.** Zero JavaScript in the document. Entirely CSS + HTML. Smooth scroll handled by `html { scroll-behavior: smooth; }`.

---

## 7. Open Graph / Meta

Current OG tags present:
- `og:type`, `og:url`, `og:title`, `og:description`
- `twitter:card`, `twitter:title`, `twitter:description`

**Missing:**
- `og:image` / `twitter:image` — no image specified, social embeds will be bare text
- `<link rel="icon">` / favicon — completely absent
- `<meta name="theme-color">` — missing

---

## 8. Issues Identified (from PRODUCT_BRIEF.md cross-reference)

### Critical
| Issue | Details |
|-------|---------|
| **No favicon** | Missing `<link rel="icon">`. |
| **No OG image** | `og:image` meta tag absent — social shares will look broken. |
| **No mobile nav** | `nav ul` hidden on ≤640px. No hamburger/drawer. ~50% of traffic gets no navigation. |
| **Fake/placeholder stats** | OSS section shows `0`, `API`, `You` as stat numbers — brand statements masquerading as metrics. |
| **No products listed** | Site talks about building software but names zero products. "Coming soon" section needed. |

### High Priority
| Issue | Details |
|-------|---------|
| **Both CTAs → GitHub** | Hero primary button = GitHub. Secondary = scrolls to Mission. No meaningful conversion action. |
| **No community entry point** | No Discord, forum, newsletter, or mailing list link. |
| **No getting-started path** | No install guide, docs link, or product page. |

### Medium Priority
| Issue | Details |
|-------|---------|
| **Team framing unexplained** | AI agents listed as team members without context. Needs a one-liner about human-AI team model. |
| **Orion missing** | Chief of Staff not in current team grid. Org chart requires 11 members. |
| **Footer thin** | Only GitHub + contact email. Needs privacy policy, community/docs links. |
| **No "contribute" CTA** | Open source identity not backed by contributor recruitment. |
| **No activity signals** | No blog, changelog, or "latest updates" — site reads as dormant. |

### Low Priority / Polish
| Issue | Details |
|-------|---------|
| Avatar images | All avatars are CSS gradient circles with initials. Mission requires unique per-person profile images. |
| Scroll animations | No reveal animations on section entry. |
| Accessibility | Color contrast on `--muted` (#7a7a9a on #08080f) may fail WCAG AA at small sizes. |
| `<meta name="theme-color">` | Missing. |
| `lang` attribute | `html lang="en"` is present ✅ |

---

## 9. File Structure (Current)

```
/projects/aeroverra.com/
  index.html          ← single-file site, all CSS inline, no JS
  PRODUCT_BRIEF.md    ← product strategy doc
  audit-notes.md      ← this file
  assets/             ← does not exist yet (needed for avatars/favicon/OG image)
```

---

## 10. Recommendations for t3_build

1. Add `assets/avatars/` directory with unique SVG per person (11 total, including Orion)
2. Add `assets/favicon.svg` + `assets/favicon.ico`
3. Add `assets/og-image.png` (1200×630) for social sharing
4. Add mobile hamburger nav (CSS-only toggle or minimal inline JS)
5. Add Orion (Chief of Staff) card to team grid
6. Update profile images: replace CSS gradient initials with `<img>` tags pointing to SVG avatars
7. Replace fake stat block with either real metrics or a "coming soon" / manifesto block
8. Add a "What We're Building" / coming soon products section
9. Keep single-file approach or split to `style.css` — either is fine, single file is simpler for Pages deployment
10. Preserve existing color scheme and typography (it's good)
