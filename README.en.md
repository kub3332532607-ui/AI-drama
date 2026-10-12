# AI 劇評室 (Drama Review Room)

**Language: [繁體中文](README.md) | English**

An offline-capable, private journal for rating and reviewing TV dramas. Score plot, production and visuals separately, write notes, add tags, keep images, and turn a review into a shareable picture.
**Everything stays on your own device. Nothing is uploaded, and no account is needed.**

👉 **Use it online:** click the **Website** link in the **About** section on the right side of this page.

> The interface is in Traditional Chinese. The sample content in the screenshots is fictional.

| Write | Library | Taste |
|:--:|:--:|:--:|
| <img src="docs/screenshot-write.png" width="250"> | <img src="docs/screenshot-library.png" width="250"> | <img src="docs/screenshot-stats.png" width="250"> |

## Features

- **Three-part scoring** (plot / production / visuals), 0–5 each: tap a big number, then fine-tune in 0.1 steps. Partial scores are fine.
- **Notes and quotes** with auto-growing fields and automatic draft saving.
- **Two-level tags** (category → tag). Reviews only store the tag; filtering by a category includes everything under it. Similar-tag hints help avoid duplicates.
- **Libraries** work like spreadsheet tabs, each with its own numbering, actor groups, script series and export hashtag. With a single library, nothing extra is shown.
- **Characters, actor groups, "Nth collaboration", script series** are all optional.
- **Search, seven sort orders, tag filters**, and a reading view for every review.
- **Taste page**: averages, score distribution (bars + curve), score ladder, monthly activity, per-group progress, and tag comparison.
- **Export** as text (copy or share) or as a picture with scores, notes, tags and your images, laid out at their original aspect ratios.
- **Backups** (with images), 30-day trash, and merge-or-replace restore.
- Dark mode, with layouts tuned for both phones and desktops.

## Install

Open the site in a browser and install it like an app; it then works offline.

- **Android (Chrome):** menu → *Install app* / *Add to Home screen*.
- **iPhone / iPad (Safari):** Share → *Add to Home Screen*.
- **Desktop (Chrome / Edge):** the install icon at the right end of the address bar.

## Privacy and backups

Data lives in your browser's IndexedDB on this device only. Switching phones, clearing site data, or uninstalling can erase it, so download a backup regularly from **Settings → Backup & data** (tap the ✦ icon at the top left). To move to a new device: export a backup, open the site on the new device, then use *Restore backup*.

## Self-hosting

It is a plain static site (a single `index.html`, no build step, no dependencies).

1. **Fork** this repository.
2. **Settings → Pages** → *Deploy from a branch* → `main` / `(root)` → Save.
3. Your site URL appears at the top of that page after about a minute.

## License

Released under the [MIT License](LICENSE).
