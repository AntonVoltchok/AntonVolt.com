# How to run this in Fable

Two pieces, not one giant paste:

1. Paste [`00_PASTE_THIS.txt`](00_PASTE_THIS.txt) as the message.
2. Attach [`01_BRIEFING.md`](01_BRIEFING.md) as the only file.

**First Fable reply should be questions + challenges only.** Do not let it write the architecture yet. Answer the numbered list (or reply `go` to accept its defaults), then tell it to continue.

If it skips the stop and dumps SOPs, send: `Stop. Redo the first reply as Challenges + Questions only, per the prompt.`

At the end of the architecture reply, or whenever you say `handoff`, it must write `CONTINUE_SESSION.md` and paste it in chat. Next Fable thread: attach only that file, paste the “Next action” paragraph from section 9.

Second reply must include three hours tables, every number labeled GUESS:
- Table 1 = current client/offshore labor (August **task counts**, not Zoho Duration×24)
- Table 2 = future **client** tickets after AdsUp (pages, SOP B conversions, residual incidents)
- Table 3 = **your** time to implement AdsUp (WP admin, monitoring, ticketing, purge, plugin diffs) — MVP vs full spec

Do not attach the xlsx files or the AdsUp report; they are already distilled.
