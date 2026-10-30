# Massanutten Rental Watch: public feed

Short-term rental news for Massanutten owners, compiled every morning from public records: the Rockingham County, Virginia website (Board of Supervisors, Planning Commission, and Board of Zoning Appeals agendas and minutes, the county calendar, news, and short-term rental pages) and the Massanutten Property Owners Association website (the proposed covenant changes page and the community calendar). Every item links to its source. Not affiliated with the county or the association.

Maintained by Michael Flanagan. The files here are rebuilt and pushed automatically around 7:00 in the morning, Eastern time, every day.

## For the website (written for Reba's Claude)

This repository (served by GitHub Pages) exists so diamondelevation.com can show the feed. Pick whichever option suits the site; nothing here needs to be copied by hand, because the URLs below always serve the latest version.

There are two pages, one per topic, and they are kept separate on purpose: Rockingham County's short-term rental rules, and the Massanutten Property Owners Association's covenant rewrite. Each page leads with one main thing owners can do, then other ways to help, the next few meetings, and what was found in the last thirty days.

URLs:

- County page: https://mflanagan1981.github.io/massanutten-rental-watch/county.html (embed version: `embed-county.html`)
- Association page: https://mflanagan1981.github.io/massanutten-rental-watch/association.html (embed version: `embed-association.html`)
- https://mflanagan1981.github.io/massanutten-rental-watch/ and `embed.html` now only link to the two pages. If the site currently embeds `embed.html`, please switch to the two embeds above, one on each of the site's pages.
- Daily report: https://mflanagan1981.github.io/massanutten-rental-watch/report.md (also at https://raw.githubusercontent.com/mflanagan1981/massanutten-rental-watch/main/report.md). A short plain-text summary rewritten every morning: how many items are new since yesterday and what they are, then for each topic its main action, meetings and deadlines in the next fourteen days, and findings from the last thirty days, each with its source link. This is the easiest thing to check once a day. Earlier days are in the repository's commit history.
- Data: https://mflanagan1981.github.io/massanutten-rental-watch/status.json (also at https://raw.githubusercontent.com/mflanagan1981/massanutten-rental-watch/main/status.json). Both allow cross-origin requests, so a page can `fetch()` it directly.

The embed versions have no outer padding and report their own height, including when a visitor opens or closes a section.

### Option 1: embed with an iframe (simplest)

Paste into the site builder's embed or custom HTML block, once on the county page and once on the association page, changing `embed-county.html` to `embed-association.html` for the second. The script makes the frame grow to fit, so there is no inner scroll bar.

```html
<iframe id="rental-watch" src="https://mflanagan1981.github.io/massanutten-rental-watch/embed-county.html"
        title="Massanutten Rental Watch" style="width:100%;border:0;min-height:1200px" loading="lazy"></iframe>
<script>
  window.addEventListener("message", function (e) {
    if (e.origin === "https://mflanagan1981.github.io" && e.data && e.data.type === "rental-watch-height") {
      document.getElementById("rental-watch").style.height = e.data.height + "px";
    }
  });
</script>
```

If the builder strips scripts, keep only the iframe and give it a fixed height (about 1,500 pixels on phones, 1,200 on desktop). The embed pages follow the visitor's light or dark setting.

### Option 2: read the data and render it in the site's own style

`status.json` has this shape. Field names are stable; new fields may be added, never renamed or removed without notice here.

```json
{
  "title": "Massanutten Rental Watch",
  "updated": "Monday, October 19, 2026, 7:02 AM Eastern",
  "updated_date": "2026-10-19",
  "about": "One sentence describing the feed.",
  "signup": { "label": "Join the owners' email list", "link": "URL" },
  "report": "URL of report.md",
  "pages": {
    "county": PAGE,
    "association": PAGE
  },
  "take_action": [ ITEM, ... ],
  "key_findings_last_30_days": [ ITEM, ... ],
  "upcoming_meetings": [ ITEM, ... ],
  "deadlines": [ ITEM, ... ]
}
```

Each PAGE has `name`, `headline`, `lede` (one or two sentences), `page` and `embed` (URLs), `main_action` (one ITEM, the thing to show biggest), `more_actions`, `coming_up` (the next four meetings and deadlines), and `findings` (last thirty days, newest first). Build each topic's page from its PAGE. The four lists after `pages` mix both topics and are kept for older integrations.

Each ITEM has `id` (stable, safe to use as an HTML anchor), `side` ("County" or "Association"), `title`, `detail` (plain text, one to four sentences), `new` (true if added today or yesterday), and usually `date` (YYYY-MM-DD; a deadline whose date is not announced has none), `time` (meetings, for example "6:00 PM"), and `link` (the public source). Actions also have `short` (one or two sentences for the card; show it instead of `detail`), `steps` (short instructions, best shown collapsed under `steps_label`, for example "What to write"), `link_label` (text for the button that opens `link`), and, when there is a deadline or meeting, `due_label` (for example "Deadline") to show with `date`. Lists are already filtered and sorted; the site does not need to filter anything.

Please keep the attribution line ("Compiled from public Rockingham County and Massanutten Property Owners Association records; not affiliated with either") and the source links.

## Feedback and changes

If the site needs something different (other fields, a different layout for the embed, a shorter version, style hooks, different colors or fonts to match the site), open an issue at https://github.com/mflanagan1981/massanutten-rental-watch/issues describing what is needed and why. The maintainer's side checks issues every morning and will either make the change or reply. Please do not open pull requests that edit the generated files directly; they are overwritten every morning. Describe the change in an issue instead.
