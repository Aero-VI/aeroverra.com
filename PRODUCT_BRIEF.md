# Product Brief: aeroverra.com
**Project:** Aeroverra Company Website  
**Owner:** Zara (Head of Product)  
**Status:** Live — Draft v1  
**Brief Date:** 2026-03-14  
**Last Updated:** 2026-03-14

---

## 1. What Is This?

`aeroverra.com` is the public face of the Aeroverra organization. It is a marketing and trust-building website — not a product itself. Its job is to clearly communicate who we are, what we stand for, and why someone should care enough to follow us, contribute to our work, or eventually use our software.

The current build is a single-page static site (HTML/CSS, no framework) hosted at `aeroverra.com`. It was built as a foundational placeholder to establish brand presence while product development begins.

---

## 2. Who Is It For?

**Primary audience:**
- **Developers and technical self-hosters** who are already frustrated with Big Tech dependency and are looking for alternatives. They read GitHub readmes, run their own servers, and care deeply about open source.

**Secondary audience:**
- **Curious non-technical privacy advocates** who know they want data ownership but aren't sure how to get there yet. They might not self-host today but they're our future users once we lower the barrier.

**Tertiary audience:**
- **Potential contributors** — open source developers who want to build on something with purpose. They need to see what we're building and believe in how we're building it.

---

## 3. What Success Looks Like

### Short-term (0–90 days)
- The site clearly communicates our mission to any visitor within 10 seconds
- GitHub org link is prominently featured and driving traffic to repos
- At least one real product is featured on the site (not just mission and team)
- Zero broken links, no placeholder stats

### Medium-term (90–180 days)
- Community growth link in place (Discord, forum, or mailing list)
- Site is a credible signal of an active, serious project when someone Googles us
- Contributors and self-hosters can find their way to getting started without friction

### Long-term
- aeroverra.com is the go-to destination for learning about our ecosystem
- Tracks community size, project count, and contributor milestones in real numbers
- Establishes Aeroverra alongside Nextcloud, Proton, and Mailcow in the public mind

---

## 4. What's Working

The current draft gets several things right:

- **Visual design is strong.** Dark theme, purple accents, clean typography — it reads as credible and modern. Not cheap, not corporate.
- **Mission copy is clear.** The four pillars (Self-Hosted First, Fully Open Source, API First, Easy to Install) are well-defined and differentiated.
- **Hero headline is direct.** "Take back control of your digital life" is exactly the right framing.
- **Team section humanizes the org.** Listing the team (even that it includes AI agents) is a bold, interesting choice. It signals transparency.
- **Single-file architecture** keeps deployment simple and maintenance low at this stage.

---

## 5. What's Missing or Needs Improvement

### Critical (blocks credibility)

| Issue | Detail |
|-------|--------|
| **No products listed** | The site talks about what we build but doesn't name anything. Visitors can't evaluate whether we've shipped anything. Placeholder until our first product ships, but needs to be a "coming soon" section at minimum. |
| **Fake / placeholder stats** | The Open Source section shows `0` (vendor lock-in), `API`, and `You` as stats. These are brand statements masquerading as metrics. Remove them or replace with real numbers (repo count, commits, contributors) once available. |
| **No mobile nav** | Navigation collapses entirely on mobile (`display: none`). There's no hamburger menu. ~50% of visitors will get a navigation-less experience. |
| **No favicon** | Missing entirely. |
| **No OG/social meta tags** | Sharing aeroverra.com on Discord, Twitter, or anywhere else will produce a blank/ugly embed. Needs `og:title`, `og:description`, `og:image`. |

### High Priority (significantly hurts conversion)

| Issue | Detail |
|-------|--------|
| **Both CTAs go to GitHub** | The Hero has two buttons: "View on GitHub" and "Learn more." The second scrolls to Mission. Neither of these is a meaningful conversion action. Where do we want people to go? What do we want them to do? |
| **No community entry point** | No Discord, no forum, no newsletter. Someone who wants to follow Aeroverra has nowhere to land other than GitHub. |
| **No "getting started" path** | Self-hosters looking to try something have no install guide, no product page, no docs link. |

### Medium Priority (improves positioning)

| Issue | Detail |
|-------|--------|
| **Team section framing** | Listing AI agents as team members is interesting and transparent, but currently unexplained. A one-liner about our human-AI team model would turn this from confusing to compelling. |
| **No blog or updates feed** | Nothing shows Aeroverra is active. A "Latest" or "Updates" section with even 1–2 entries signals the org is alive. |
| **Footer is thin** | Only GitHub and a contact email. Needs: Privacy Policy (even minimal), link to docs/community when available. |
| **No "contribute" pathway** | No call to developers to get involved. If open source is core to our identity, we should be explicitly recruiting contributors. |

### Low Priority (polish)

- Add `lang` attribute completion and `<meta name="theme-color">`
- Scroll animation on section reveals would add polish
- Accessibility audit needed (color contrast on muted text, keyboard nav)
- Consider a blog or changelog for long-term SEO health

---

## 6. Recommended Next Actions

**Immediate (this sprint):**
1. Add favicon and OG meta tags
2. Build mobile hamburger nav
3. Replace fake stats with either real numbers or remove the stat block
4. Add a "What We're Building" / "Coming Soon" section for our first product

**Next sprint:**
5. Add community link (Discord or equivalent) once established
6. Clarify team section copy around the human-AI model
7. Revisit CTA strategy — where do we actually want people to go?

**When first product ships:**
8. Add a proper product section with install path
9. Surface real GitHub metrics (stars, contributors, repos)

---

## 7. Out of Scope (for this site)

- This is a marketing/landing site, not a product portal. Docs, changelogs, API references, and install guides live in product repos or a future docs subdomain (`docs.aeroverra.com`).
- No authentication, no user accounts, no backend required for this project.

---

## 8. Dependencies

| Dependency | Owner | Status |
|-----------|-------|--------|
| First product name/description | Nicholas / Zara | Pending |
| Community platform decision (Discord, etc.) | Echo | Pending |
| Brand assets (favicon, OG image) | Lyra | Needed |
| Real GitHub metrics | Rex | Available once repos are active |

---

## 9. Open Questions

1. Does Aeroverra have a position on AI-built teams? Should we own that story publicly or keep it low-key for now?
2. What is the first product? When does it ship? This site needs that answer to stop being a placeholder.
3. Is the contact email `hello@aeroverra.com` live and monitored?

---

*No feature work on this project starts without this brief being current. Update before each sprint.*
