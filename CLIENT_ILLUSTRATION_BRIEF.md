# Illustration brief — "The Manager's Office" (Part 1: monitoring, automation, efficient repairs, transparent work orders)

Hand this file to Claude Design as-is. Sections marked **[internal]** are for the author only and must not appear in the client-facing artwork.

---

## 0. Purpose and audience

- Audience: owners and marketing leads of behavioral-health clinics who run 50 websites through an outside development team. Assume zero engineering knowledge.
- Goal: in one meeting, using six pictures, make them understand (1) what is watched automatically, (2) how a request becomes finished work, (3) why repairs are safer and faster, and (4) what they pay for.
- Tone: calm, competent, friendly. No fear, no blame toward the current team.
- One metaphor throughout: **50 websites = 50 buildings on one street, managed from one office.**
- The word "website" may appear in captions; the pictures show buildings.

## 1. Hard rules (never draw or write)

- No software names, logos, or screenshots: no WordPress, WP Engine, Cloudflare, Jira, MainWP, Elementor, Yoast, WP Rocket, Better Stack, n8n.
- No AI, robots, chatbots, or "assistant" imagery except the single teaser frame at the end.
- No code, terminals, dashboards with real data, or browser windows.
- No numbers: no hours, rates, percentages, dollar amounts, dates.
- No rebuilding, renovation, demolition, or "new wiring" imagery. This deck is about running the buildings, not rebuilding them.
- No company names or real people. Roles only.
- Nothing medical: no patients, clinics, hospitals, stethoscopes.

## 2. Style sheet

- Flat vector illustration, soft geometric shapes, minimal outlines.
- Palette: 4 muted neutrals (warm grey, sand, slate, off-white) + **one accent color (amber)** used **only** for moments where a human is in control: the inspector's stamp, the owner's key, the front-desk clerk's pen. Nothing automated is ever amber.
- People: simple, neutral, no faces beyond a dot-eye level; identified by a small label under them: **Owner**, **Crew**, **Inspector**, **Front Desk**.
- Text inside images: labels only, max 3 words each. The caption lives under the panel, not inside it.
- Aspect ratio: 16:9 per panel. Also export a vertical 4:5 crop for each for mobile sharing.
- Consistent street: the same six building silhouettes appear in every panel that shows the street (panels 1, 2, 5). Building #3 has a red awning; building #5 has a rooftop sign. Use these as visual anchors.
- The **same three crew figures** (tall, short, with cap) appear in every panel. Continuity says "same team, better setup."

## 3. Legend (client-facing, one small card printed once, before panel 1)

| Picture | Means |
|---|---|
| Building | One of your websites |
| Manager's office | The one screen where all sites are watched, kept on your account |
| Sensor box | A small watcher installed in each site |
| Front desk | Where every request becomes a written work order |
| Crew | Your developers |
| Inspector's stamp | A person approving before anything touches a live site |
| Practice room | An exact copy of a site where repairs are tried first |
| Brochures | The saved copies of a page that visitors see |

## 4. Full mapping table **[internal]**

| Real thing | In the picture |
|---|---|
| 50 websites | 50 buildings on one street |
| Control room (MainWP on client WPE) | Manager's office: wall of monitors; the master key stays on the Owner's ring |
| adsup-connector | Sensor box in each building |
| Uptime / SSL / domain / security monitoring | Smoke detectors, cameras, lease-and-permit renewal calendar, lock inspections |
| Auto-remediate before ticketing | Sprinkler that handles a small fire alone; calls the desk only if it keeps burning |
| 3-layer cache purge | Three brochure copies (lobby, mailroom, street kiosk); one button replaces all three |
| Intake (n8n classification) | Front Desk clerk writing the work order; the clerk's pen is amber |
| Jira | Work-order board on the office wall: To Do → In Progress → Checked → Done |
| Offshore developers | Crew; one work order in hand at a time |
| You | Inspector; amber stamp |
| Client | Owner; amber key; receives the itemized receipt |
| Staging | Practice room: exact copy of a room |
| Backup point | "Before" photos pinned to the wall before the crew starts |
| Normalized HTML diff | Inspector holding before/after photos side by side; only real differences circled |
| Screenshot diff | Second pair of photos from across the room |
| Human sign-off | Stamp on the work order |
| Deploy (human) | Crew repeats the rehearsed repair in the real building |
| Post-deploy verification | Inspector re-photographs the real room and compares |
| Rollback | Old part kept on the shelf for a quick swap back |
| Plugin updates | Replacing a part (boiler valve) |
| Forms never break | The mailbox: a test letter is sent before and after every repair and must arrive at the same office |
| SEO never breaks | Street address, signage, map listing never change; checked before and after |
| Draft + 301 tickets | If a door must move, a sign on the old door points to the new one, posted before the move |
| Broken links | Doors that open onto nothing; monthly walk with a checklist |
| Invoice = closed tickets | Itemized receipt: one line per stamped, finished work order |
| Tech-report lines (uptime, sitemap, robots, IndexNow, speed reports) | Thermostats and smoke detectors: run themselves, never on the receipt |

Not in this deck: Elementor conversion, ACF, Claude/NovaMira, Git, hidden AI estimates, shadow calibration, rates, Table 1 percentages.

---

## 5. Panels

### Panel 1 — Today (split panel: left "Today", right "With the office")

**Purpose:** show the problem is the walking, not the crew.

**Caption:** "Fifty buildings, one crew, and no way to see them all at once."

**Left half ("Today"):**
- Street of buildings receding into the distance, more than can be counted.
- The three crew figures on foot between buildings; the tall one holds a ring of far too many keys.
- The short one is on a ladder checking a thermostat by hand on building #3; the capped one is climbing stairs in building #5.
- One building far down the street has a small wisp of smoke nobody has noticed.
- Bottom-left: a crumpled paper bill labelled "Maintenance — 1 month" (label allowed; no amount).

**Right half ("With the office"):**
- Same street, seen from the window of a small office in the foreground.
- The same three crew figures seated at one desk, facing a wall of small monitors, one per building.
- The smoke from the far building appears as a lit indicator on one monitor.
- Owner stands beside the desk holding a single amber key.

**Do not draw:** screens with data, alarms blaring, exhausted or lazy crew. Neutral posture.

### Panel 2 — The manager's office

**Purpose:** what is watched automatically and where the keys live.

**Caption:** "Same crew. One desk instead of fifty doors. The keys stay with you."

**Elements:**
- Interior of the office. Wall of monitors, each a simple building silhouette with three tiny icons under it: a smoke detector, a padlock, a calendar page.
- Cutaway through the window: three buildings on the street, each with a small **sensor box** mounted near the door, a thin dotted line running from each box to the office wall.
- A wall calendar labelled "Renewals" with a few dates circled (no readable dates).
- Owner in the doorway hanging the amber master key on a hook labelled "Owner".
- One monitor shows a small sprinkler icon and a tiny extinguished flame: handled without anyone getting up.
- Crew at the desk, relaxed, one holding a clipboard.

**Do not draw:** a vendor figure, a cloud, a server rack, anything with a cable to outside the owner's office.

### Panel 3 — A work order's life

**Purpose:** every piece of work is written, scoped, done, checked.

**Caption:** "Every request becomes a written work order: which building, which room, what 'fixed' looks like, how long it should take."

**Layout:** left-to-right flow, five stations connected by a floor line.

1. **Two inputs, top-left:** (a) a monitor with a lit indicator; (b) the Owner handing a note across the desk. Both arrows lead to station 2.
2. **Front Desk:** clerk with amber pen writing on a work-order card. The card shows four small pictograms: a building, a door, a checkmark box, an hourglass.
3. **Board:** wall board with four columns of cards labelled "To do", "In progress", "Checked", "Done". One card is moving.
4. **Crew:** the capped crew figure holding exactly one card, walking toward building #3 with a toolbox.
5. **Inspector:** figure with an amber stamp pressing it on the returned card; the card slides into "Done".

**Small side detail (bottom-left):** a sprinkler over a wastebasket with a tiny extinguished flame and a card that never reached the desk, crossed out lightly. Label: "Handled itself."

**Do not draw:** multiple cards in one crew member's hands, a queue of angry people, a phone.

### Panel 4 — Safe repairs

**Purpose:** repairs are rehearsed, compared, approved, then repeated; the mailbox and signage are protected every time; this is how updates get faster and safer at once.

**Caption:** "Try it in the practice room. Compare the photos. Stamp it. Then do it in every building in one round."

**Layout:** four beats, left to right.

1. **Practice room:** a room built as an exact copy of a room in building #3, doorway labelled "Practice". The short crew figure swapping a boiler valve. A "before" photo pinned on the wall by the door.
2. **Compare:** the Inspector holding two photos side by side, one labelled "Before", one "After". A single small circle on the "After" photo marks the one real difference; everything else identical. Two extra items sit on the Inspector's table: an envelope with a checkmark (test letter arrived) and a small street sign with a checkmark (signage unchanged).
3. **Stamp:** amber stamp on the work order.
4. **One round:** the three crew figures each carrying the same boxed part toward a row of buildings; a shelf in the background holds the old valve, labelled "Kept".

**Do not draw:** broken things, sparks, a building under construction, more than one difference circled.

### Panel 5 — The three brochures

**Purpose:** explain the most common "it still shows the old version" complaint and its fix.

**Caption:** "A page has three saved copies. Replace one and visitors still see the old one. Now one button replaces all three."

**Split layout inside one panel (not a before/after split; two rows).**

**Top row ("Before"):** a building with three brochure racks: one in the lobby, one in the mailroom, one in the street kiosk. Crew member replaces the lobby brochure with a new design; the other two racks still hold the old design; a visitor at the kiosk holds the old one, puzzled.

**Bottom row ("Now"):** the same three racks; a single large button on the office desk labelled "Replace all"; three dotted lines from the button to the three racks; all three hold the new design; the visitor at the kiosk holds the new one.

**Do not draw:** any word resembling "cache", clouds, servers, lightning bolts.

### Panel 6 — The receipt (split panel: left "Today", right "With the office")

**Purpose:** what you pay for.

**Caption:** "You pay for finished repairs, not for walking between buildings."

**Left half:** the crumpled bill from panel 1, now large, single line "Maintenance — 1 month", edges torn. Faint footprints trail behind it.

**Right half:** a clean itemized receipt, two columns.
- Left column header: "Runs by itself — never billed". Items as pictograms with two-word labels: smoke detector ("Uptime watch"), thermostat ("Speed check"), street sign ("Map listing"), mailbox ("Mailbox test"), calendar ("Renewal watch").
- Right column header: "Finished work orders". Three or four lines, each a small card with a building icon, a door icon, and an amber stamp. No amounts, no hours.
- The Owner holding the receipt; the Inspector's stamp visible on each card.

**Do not draw:** currency symbols, totals, strikethrough prices, a sad vendor.

### Teaser frame (final, quarter-size, appears after panel 6)

**Purpose:** plant step two without explaining it.

**Caption:** "Next: a helper for the crew."

**Elements:** the manager's office, dimmed. The three crew figures at the desk. A fourth, plain silhouette figure, unlabelled, handing the tall crew member a tablet. Nothing on the tablet screen. No glow, no sparkles, no robot features.

**Do not draw:** anything that suggests replacing the crew.

---

## 6. Output specification for Claude Design

- Seven images: `01-today`, `02-office`, `03-work-order`, `04-safe-repairs`, `05-brochures`, `06-receipt`, `07-teaser`.
- Each at 16:9 (1920×1080) and 4:5 (1080×1350). Teaser at half-height 16:9.
- Deliver captions as a separate text layer or file so they can be translated or edited.
- One legend card (section 3) at 16:9.
- Keep a shared component sheet: six building silhouettes, three crew figures, Owner, Inspector, Front Desk, sensor box, amber stamp, amber key, work-order card, brochure rack.

## 7. Prompt to paste into Claude Design

```
You are illustrating a six-panel explainer plus one teaser frame for non-technical clinic owners. Read the attached brief in full. Use exactly one metaphor: fifty websites are fifty buildings on one street managed from one office. Follow the hard rules (no software names, no AI imagery except the teaser, no numbers, no rebuilding imagery, nothing medical). Follow the style sheet: flat vector, muted neutrals, amber used only where a human is in control. Reuse the same six building silhouettes and the same three crew figures in every panel. Draw each panel exactly as specified under "Panels", including the split layouts for panels 1 and 6 and the two-row layout for panel 5. Place the caption under each panel, not inside it. Produce the legend card first, then panels 1–6, then the teaser, then a component sheet. Before rendering, list any element in the brief you cannot depict clearly at 16:9 and propose a substitute.
```
