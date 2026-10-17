# Massanutten Rental Watch: public feed

Short-term rental news for Massanutten owners, compiled every morning from public records: the Rockingham County, Virginia website (Board of Supervisors, Planning Commission, and Board of Zoning Appeals agendas and minutes, the county calendar, news, and short-term rental pages) and the Massanutten Property Owners Association website (the proposed covenant changes page and the community calendar). Every item links to its source. Not affiliated with the county or the association.

Maintained by Michael Flanagan. The files here are rebuilt and pushed automatically around 7:00 in the morning, Eastern time, every day.

## For the website (written for Reba's Claude)

This repository (served by GitHub Pages) exists so diamondelevation.com can show the feed. Pick whichever option suits the site; nothing here needs to be copied by hand, because the URLs below always serve the latest version.

URLs:

- Full page: https://mflanagan1981.github.io/massanutten-rental-watch/
- Embed page (no title, tighter spacing, reports its own height): https://mflanagan1981.github.io/massanutten-rental-watch/embed.html
- Data: https://mflanagan1981.github.io/massanutten-rental-watch/status.json (also at https://raw.githubusercontent.com/mflanagan1981/massanutten-rental-watch/main/status.json). Both allow cross-origin requests, so a page can `fetch()` it directly.

### Option 1: embed with an iframe (simplest)

Paste into the site builder's embed or custom HTML block. The script makes the frame grow to fit, so there is no inner scroll bar.

```html
<iframe id="rental-watch" src="https://mflanagan1981.github.io/massanutten-rental-watch/embed.html"
        title="Massanutten Rental Watch" style="width:100%;border:0;min-height:900px" loading="lazy"></iframe>
<script>
  window.addEventListener("message", function (e) {
    if (e.origin === "https://mflanagan1981.github.io" && e.data && e.data.type === "rental-watch-height") {
      document.getElementById("rental-watch").style.height = e.data.height + "px";
    }
  });
</script>
```

If the builder strips scripts, keep only the iframe and give it a fixed height (about 1,800 pixels on phones, 1,200 on desktop). The embed page follows the visitor's light or dark setting.

### Option 2: read the data and render it in the site's own style

`status.json` has this shape. Field names are stable; new fields may be added, never renamed or removed without notice here.

```json
{
  "title": "Massanutten Rental Watch",
  "updated": "Friday, October 16, 2026, 1:54 PM Eastern",
  "updated_date": "2026-10-16",
  "about": "One sentence describing the feed.",
  "key_findings_last_30_days": [ ITEM, ... ],
  "upcoming_meetings": [ ITEM, ... ],
  "deadlines": [ ITEM, ... ]
}
```

Each ITEM has `id` (stable, safe to use as an HTML anchor), `side` ("County" or "Association"), `title`, `detail` (plain text, one to four sentences), `new` (true if added in the last three days), and usually `date` (YYYY-MM-DD; a deadline whose date is not announced has none), `time` (meetings, for example "6:00 PM"), and `link` (the public source). Lists are already filtered and sorted: findings newest first and only from the last thirty days, meetings from today forward for sixty days, deadlines until they pass. The site does not need to filter anything.

Please keep the attribution line ("Compiled from public Rockingham County and Massanutten Property Owners Association records; not affiliated with either") and the source links.

## Feedback and changes

If the site needs something different (other fields, a different layout for the embed, a shorter version, style hooks, different colors or fonts to match the site), open an issue at https://github.com/mflanagan1981/massanutten-rental-watch/issues describing what is needed and why. The maintainer's side checks issues every morning and will either make the change or reply. Please do not open pull requests that edit the generated files directly; they are overwritten every morning. Describe the change in an issue instead.
