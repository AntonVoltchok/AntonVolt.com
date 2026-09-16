# Fable briefing (facts only)

Portfolio: ~51 marketing WordPress sites (mostly US addiction/BH, plus billing + one law firm). Agency currently builds in Elementor.

## Proven vs guessed

**Proven (do not re-ask):** public scan 2026-09-16; 50/51 have WP 6.9+ Abilities API; modal Hello+Elementor Pro+Yoast+Rocket+WPE; Wordfence uneven; August task mix; AdsUp already defined “tickets only when red” and three cache layers.

**Guessed by the human (challenge if you have a better idea, then wait):**
- Custom classic PHP theme + ACF is the right end state vs a locked-down block theme or a shared parent theme for all brands.
- Convert existing Elementor sites in place (staging clone, new theme, then cut over) vs rebuilding on a fresh WP.
- One dashboard on the client’s WP Engine account vs MainWP / ManageWP / WP Engine portal + a thin ticket layer.
- NovaMira connected per staging site vs a single gold image that gets copied.
- All new pages after conversion go through Claude/NovaMira; clients never touch a canvas.
- Kill Elementor after conversion rather than keep it for one-off landing pages.

If you challenge, recommend a default so work can continue when the human says “go.”

## What we already proved (2026-09-16 public scan)

- 50/51 expose `wp-abilities/v1` → WordPress 6.9+ (NovaMira’s Abilities API gate). Recovery 180 is WP 7.0.4.
- PHP version: never in public headers. Do not treat Abilities API as PHP 8 proof. WP 6.9 still allows PHP 7.2.24.
- Direct datacenter crawls get Cloudflare 403 on ~45 sites. Use logged-in admin / host portal / NovaMira on staging, not public curl, for PHP and plugin versions.
- Yonder BH (`yonderbh.com`): unreachable this run. Skip until it loads.

### Modal stack (practice site = this)

n=50 sites with REST. Percents ≈ share of those 50.

| Piece | Share | Practice site |
|---|---|---|
| Elementor + Elementor Pro | 90% | Yes (today). Target end-state: uninstall after conversion |
| Hello Elementor theme | 82% | Yes as current. Target: custom classic theme |
| Yoast SEO | 86% | Keep. Do not add Rank Math on the same site |
| WP Rocket | 84% | Keep |
| Redirection plugin | 84% | Keep |
| WP Engine Cache + SSO plugins | 80% | Host-only; appear on WPE |
| Wordfence | 68% | Baseline security plugin (uneven today) |
| Trustindex reviews | 52% | Optional |
| Rank Math instead of Yoast | 10% | Outlier; do not mix |

41/50 = Elementor + Pro + Hello. 28/50 also have Yoast + Rocket + WPE + Redirection.

### Host cohorts (NovaMira install order)

- WP Engine plugins: 40 sites. WPE auto-removes EOL PHP and force-upgrades; they do not evict the site. As of WPE docs (updated Jul 2026) listed versions include **7.4 (temporary extended support), 8.2, 8.4**. 8.0/8.1 should be gone. **Not 100% safe to assume every WPE site is PHP 8+** because 7.4 can still exist. Check portal. For this agency’s 6.9+ Elementor stack, 8.2/8.4 is the realistic default.
- SiteGround Security: Aftermath ATC, Aftermath BH, Redemption DE, Redemption NJ.
- Elementor Cloud header: Foundation BH, Foundation Ohio, Foundation Recovery Centers.
- Other but still WP 6.9+: Recovery 180, Discovery Institute, iMind Mental.

### Builder outliers (conversion path differs)

- WPBakery + Rank Math (no Elementor): Discovery Institute, California Healing Centers.
- No Elementor; CF7 + SiteGround + Yoast: Redemption DE/NJ.
- Elementor Pro but not Hello: Sanford, Purposes, Imagine Wellness, Fountain Hills.

Forms: no single winner in REST (Gravity / WPForms / CF7 / Elementor forms). Conversion should map forms to ACF or one form plugin, not leave Elementor form widgets as the source of truth.

## Two build pipelines (this is the product)

Shared rules: design lives in the theme. Clients edit named ACF fields (and CPTs). They never get a page builder. NovaMira on **dev/staging only**, backup first, no undo. Disable AI abilities on production. Git the theme. `acf-json` in the theme. Escape all output. Keep original CSS class names.

### Pipeline A — NEW site or NEW page

Ferdy Korpershoek video second half (`dXyjzDFSKaw` from 22:53). Do not redo Claude Design→HTML in Fable unless asked.

Prereq: HTML/CSS/JS export on disk; staging WP 6.9+ / PHP 8.0+ (prefer 8.2); Git; NovaMira + ACF (PRO if repeaters/flex/options).

Sequence:
1. NovaMira ZIP → activate → Enable AI Abilities → connect Cursor/Claude via MCP OAuth URL from admin (`/wp-json/mcp/novamira-oauth`). Prove: “list installed plugins.”
2. ACF (WP Engine). Field Groups / Post Types / Taxonomies.
3. Agent converts HTML → classic theme: `style.css` header, `functions.php`, `header.php`/`footer.php` (`wp_head`/`wp_footer`, `body_class`), `front-page.php`, `page.php`, `single.php`, `archive.php`, enqueue existing assets. No Elementor/Bricks/Divi/FSE as layout engine.
4. ACF groups match HTML sections (hero, about, FAQs…). CPTs only if objects repeat (events, staff, locations, insurance). Local JSON save/load in `acf-json/`.
5. SEO: permalinks Post name; one H1; keep existing Yoast; sitemap already exists; don’t install a second SEO plugin.
6. Analytics/GSC: one gtag path only (theme **or** Site Kit, not both). Submit sitemap once.
7. Role `content_editor`: pages/CPTs/media only. No themes, plugins, settings, NovaMira, ACF field-group UI.
8. Production: NovaMira abilities OFF.

New **page** on an already-converted theme: ticket contains copy + images + parent URL + Yoast title/desc. Agent adds template or ACF location rule; does not open Elementor.

### Pipeline B — CONVERT live Elementor (or WPBakery) site

Do not “rebuild in Elementor more cleanly.” Do not run this on production first.

Phases:
1. Inventory on staging clone: templates, Theme Builder header/footer, global widgets, popups, forms, CPTs already in WP, Yoast, Rocket excludes, pixels (GTM/GA/CTM/Meta). Screenshot money pages.
2. Freeze: no new Elementor pages during conversion except emergencies.
3. Extract: each unique section → ACF field (names a client understands). Repeating cards → repeater or CPT. Header/footer → theme parts or ACF options (PRO).
4. Theme: new classic theme **or** child that does not depend on Elementor CSS. Port Hello/Elementor look by copying computed CSS/class names, not by leaving `elementor-element` wrappers as the contract.
5. Content migration: script or agent copies Elementor widget text/images into ACF. Verify character-for-character on homepage, program pages, insurance, contact, locations.
6. URL freeze: same slugs. Redirection plugin catches any slug change. Yoast titles/canonicals unchanged unless ticket says so.
7. Forms: rebuild in Gravity/CF7/Elementor-free equivalent; preserve GTM events and CTM / CallRail scripts. Rocket must not cache tracking JS (this is a known August failure class).
8. QA: logged-out HTML, mobile nav, forms to CRM, schema, CWV spot-check one URL. Staging HTML diff vs production.
9. Cutover: theme on, Elementor pages unpublished not deleted until 14 days, then disable Elementor Pro → Elementor. Keep Hello only if the new theme is standalone.
10. Rollback: revert theme + re-enable Elementor; backups immutable.

WPBakery outliers: same SOP, swap “Elementor” for “WPBakery / Ultimate Addons.”

## Ops layer (already sold in AdsUp-1stReport — implement, don’t re-pitch)

Single control room on **client WP Engine**. Self-hosted. No vendor backdoor to client PII.

Dashboard does:
- Uptime on homepage + key URLs (contact, insurance). Alert call/SMS/Slack. Ticket only if red.
- SSL/domain expiry alerts.
- CVE / missing baseline (Wordfence uneven today).
- Malware fingerprints, blacklist monitoring.
- Cache purge all reachable layers: **WP Rocket + WP Engine + Cloudflare**. Three copy machines; purging one is why “cache issue” is a monthly line today.
- Plugin updates: apply on staging → HTML diff → human sign-off → host push to prod. Routine vs sensitive plugins.
- Broken links / redirect list; optional fix without 50 wp-admins.

Tickets:
- Created by automation or AdsUp intake. Client/offshore tickets that skip this go to Review first.
- Each ticket: one site, one URL if performance, definition of done, time box, hours estimate **before** work.
- Invoice = closed tickets only.
- Estimated cut to padded “tech report” spend: 30–50% after a few months of real tickets. Do not promise a tighter number yet.

### August 2026 Zoho task export (220 rows) — what the work actually is

Duration column is calendar-ish and inflated (`Work hours` often looks like duration×24). Use **counts**, not those hour fields, as demand signal.

| Class | ~count | After transformation |
|---|---|---|
| New / evergreen / program pages | 68 | Pipeline A or B-on-converted-theme. Highest value for ACF page templates |
| Recurring “SEO technical optimizations (# of hours)” | 47 | Kill as a bucket. Replace with event tickets (GSC spike, 404, CWV Good→Poor) |
| Recurring “SEO blogs (# of blogs)” | 16 | Content ticket: publish post in classic editor/block; not Elementor |
| Elementor layout tweaks (footer, whitespace, FAQ color, carousel) | 14+ | Goes away if design is theme+ACF; until then, scoped UI ticket |
| Cache / WP Rocket / CTM / self-referral | 12 | Dashboard purge + known Rocket exclude for CTM; remaining = Job B (rules) |
| HTTP mixed-content links | 8 | One-time crawl ticket per site, then monitor |
| Draft + 301 | 7 | Batch from a list; not per-URL hours |
| GSC / GTM setup | 4 | Once per property |
| Staff bios | 4 | CPT + ACF after conversion |
| Canonical / internal links | 4 | List-driven |
| Incidents (site won’t load) | 3 | Uptime ticket |

Sample tech report (what offshore billed as monthly “work”): robots.txt check, sitemap update, IndexNow 97 URLs, daily uptime, alternate-day GTmetrix, minify/lazyload, “maintain technical SEO.” AdsUp report already classified these as **automation or one-time**, not SOW lines. Fable must keep that classification.

## Hard constraints for the architect

- NovaMira cannot roll back. Staging + Git + host backup before agent writes.
- Never leave NovaMira AI abilities on in production.
- Do not double-install SEO plugins. Do not double-fire gtag.
- Do not use FSE/Gutenberg as a page builder substitute for landing pages; ACF fields on `page.php` / `front-page.php`.
- Human still approves MCP file writes.
- First pilot: one WPE site, Hello+Elementor+Yoast+Rocket, PHP confirmed in portal, staging clone, Pipeline B on homepage + 1 interior template only — not 50 sites.

## Do not dump into Fable

Full Ferdy walkthrough, August xlsx, SampleTech xlsx, 51-row URL list, plugin version hunt. This file is sufficient. If a site name is needed, pick a WPE Elementor+Hello+Yoast+Rocket site from the modal 28.
