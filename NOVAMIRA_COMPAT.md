# NovaMira compatibility (WordPress 6.9+ / PHP 8.0+)

Checked 51 client sites on 2026-09-16.

NovaMira needs the **Abilities API in WordPress core** (`wp-abilities/v1`). That namespace shipped in **WordPress 6.9**. PHP 8.0+ is a separate requirement and is **almost never visible** on a public homepage.

## Result

| Gate | Result |
|---|---|
| WordPress 6.9+ / Abilities API | **50 of 51 yes** (`wp-abilities/v1` on `/wp-json/`) |
| PHP 8.0+ | **0 of 51 confirmed** from public headers |
| Unreachable this run | **Yonder BH** (`yonderbh.com`) — direct 403, proxy timeout |

**Install-ready for the WordPress side:** 50 sites. They already expose core Abilities API, so NovaMira’s WP 6.9+ requirement is met.

**Do not treat them as fully ready until PHP is checked in wp-admin or hosting.** WordPress 6.9 still *allows* PHP 7.2.24, so Abilities API does not prove PHP 8.0+. In 2026 that leftover is uncommon on these hosts, but it is not a public fact.

## How this was measured

- Direct `curl` from this environment: **45 sites HTTP 403** (Cloudflare). Those were skipped, not treated as “not WordPress.”
- Proxy fetch of `/wp-json/` succeeded for **50 sites**.
- WP 6.9+ = presence of REST namespace `wp-abilities/v1`.
- Exact core version was public on **one** site: Recovery 180 feed has `<generator>https://wordpress.org/?v=7.0.4</generator>`.
- PHP version: no `X-Powered-By: PHP/x.y` on any site.

## Per site

WP 6.9+ is **yes** unless noted. PHP is **unknown** on every row.

### WP Engine plugin visible (40) — PHP 8.x likely, not proven

WP Engine’s current platform is PHP 8.1+ (typically 8.2/8.3). Confirm in the WP Engine portal or Site Health.

- Archway — https://archwaybehavioralhealth.com/
- BoldstepsBH — https://boldstepsbh.com/
- BoldstepsNH — https://boldstepsnh.com/
- Bold Steps RI - East Point RC — https://boldstepsri.com/
- California Healing Centers — https://californiahealingcenters.com/
- Cove Health Group — https://covehealthgroup.com/
- Fountain Hills — https://fountainhillsrecovery.com/
- Freedom Recovery — https://www.freedomrecovery.us/
- Garden State BH — https://gardenstatecounselingcenter.com/
- GBAC — https://greaterbostonaddictioncenters.com/
- GBBH — https://greaterbostonbehavioralhealth.com/
- Imagine Wellness — https://imaginewellnesscenters.com/
- Integrity Billing — https://integritybillingco.com/
- Ladoga — https://www.ladogarecovery.com/
- Lion Heart Behavioral Health — https://lionheartbehavioralhealth.com/
- Lotus Recovery — https://www.lotusrecoverycenters.com/
- Mass Bay Behavioral Health — https://massbaybehavioralhealth.com/
- Midwest Recovery — https://www.midwestrecoverycenter.com/
- New Heights — https://newheightsrecovery.com/
- PCD — https://portcharlottedetox.com/
- Portum Wellness — https://portumwellness.com/
- Purposes Recovery — https://purposesrecovery.com/
- Renewal — https://renewalwv.com/
- Revelare Recovery — https://revelarerecovery.com/
- River Rocks Recovery — https://riverrocksrecovery.com/
- Sanford Behavioral Health — https://sanfordbehavioralhealth.com/
- Sunrise Detox — https://sunrisedetoxcherryhill.com/
- Talbh — https://talbh.com/
- Team Recovery Detox & Residential — https://theteamrecovery.org/
- The Carter Treatment Center - Cumming, GA — https://thecartertreatmentcenter.com/
- The Echevarria Law Firm, PA — https://www.echevarrialegal.com/
- Titan Behavioral Health — https://titanrecoverycenters.com/
- Truhealing Cincy — https://truhealingcincinnati.com/
- Warsaw — https://warsawrecovery.com/
- Waterside Recovery — https://watersiderecovery.com/
- Waterside Mental Health Braintree — https://watersidebehavioralhealth.com/
- Wisconsin BH — https://wisconsinbehavioralhealth.com/
- Xplore Recovery — https://xplorerecovery.com/
- Graywood Wellness — https://graywoodwellness.com/
- AdsUp Asset — https://bergencountymentalhealth.com/

### SiteGround Security plugin visible (4) — PHP 8.x likely, not proven

- Aftermath ATC — https://aftermathtreatmentcenter.com/
- Aftermath BH — https://aftermathbehavioralhealth.com/
- Redemption DE — https://redemptionaddictiontreatmentcenter.com/
- Redemption NJ — https://redemptionaddictiontreatmentnj.com/

### Elementor Cloud header (3)

`X-Powered-By: Elementor Cloud` (no PHP version in the header).

- Foundation Behavioral Health — https://foundationsgroupbehavioralhealth.com/
- Foundation Ohio — https://foundationsohio.com/
- Foundation Recovery Centers — https://foundationsgrouprecoverycenters.com

### Other, Abilities API still present (3)

- Recovery 180 — https://recovery180md.com/ — **WordPress 7.0.4** (RSS generator)
- Discovery Institute — https://www.discoverynj.org/
- iMind Mental — https://imindmental.com/

### Unknown (1)

- **Yonder BH** — https://yonderbh.com/ — Cloudflare 403 from this IP; proxy fetch of homepage and `/wp-json/` timed out. Recheck from a browser or WP admin.

## Confirm PHP 8.0+ (2 minutes per site)

Public pages will not tell you. Use one of:

1. **wp-admin → Tools → Site Health → Info → Server → PHP version**
2. Hosting panel (WP Engine: Sites → PHP version; SiteGround: PHP Manager; Elementor Hosting: site settings)
3. WP-CLI on the server: `wp eval 'echo PHP_VERSION;'`

Anything `8.0.0` or higher is enough for the stated NovaMira bar. Prefer 8.2/8.3; PHP 8.0 is already EOL.

## Practical install order

1. Skip Yonder BH until it responds.
2. Install NovaMira first on a WP Engine site (40 of them) after glancing at PHP in the portal — lowest surprise.
3. Then SiteGround (4) and Elementor Cloud (3).
4. Recovery 180 is already on WP 7.0.4; still confirm PHP in admin.
5. Discovery Institute and iMind Mental are on WP 6.9+ but not on WP Engine/SiteGround plugins; check PHP before install.
