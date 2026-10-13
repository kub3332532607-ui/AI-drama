# AI 劇評室 (AI Drama Review Room)

Click the **Website** link in the **About** section of this page.

**Language: [繁體中文](README.md) | [简体中文](README.zh-CN.md) | English**

A simple drama review web app.

- **Quick scoring**: score plot, production and visuals separately. Tap a number and you are done; fine-tune by 0.1 when you want to.
- **Share your thoughts**: turn a review into formatted text or a picture and share it on social media.
- **Build your own scoring system**: collections and “My Taste” turn the AI dramas you have watched into data, so you can follow how your taste changes over time.

**Supports**: dark mode, three languages (Traditional Chinese, Simplified Chinese, English), phones, tablets and desktops.

## Quick start

1. Open **Write**, enter a title, tap the big numbers to score, and add notes if you like.
2. Tap **Save to library**.
3. Later, search, filter, read, edit or export from **Library**.
4. Tap the ✦ icon at the top left to open **Settings**: switch language, manage collections and tags, back up and restore, and check the version.

> Want to look around first? Go to **Settings → Sample data → Load**. It creates a separate sample collection that you can remove with one tap. It never mixes into your data, backups or statistics.

## Features

> This section will later become an illustrated slideshow. For now it is a plain list.

- Three-part scores, notes, quotes and tags, with automatic draft saving.
- Two-level tags: a review only stores tags, and filtering by a category includes every tag under it.
- Collections work like spreadsheet tabs, each with its own numbering, cast groups, script series and export hashtag.
- Characters, cast groups, “Nth collaboration” and script series are all optional.
- Seven sort orders, tag filters and full-text search of your notes.
- My Taste: averages, score distribution, score ladder, monthly activity, progress per group and series, and tag comparison.
- Text and picture export (pictures keep each image’s aspect ratio), backups, undo for deletions, and restore.

## Install, data and privacy

Open the site in a browser and install it like an app. It then works offline.

- **Android (Chrome)**: menu → *Install app* or *Add to Home screen*.
- **iPhone / iPad (Safari)**: Share → *Add to Home Screen*.
- **Desktop (Chrome / Edge)**: the install icon at the right end of the address bar.

**Your data stays on your own device.** Reviews and images are stored in the browser’s database (IndexedDB). They are never uploaded, the author cannot see them, and the app does not collect any usage data.

Because the data lives only on this device, switching phones, clearing browser data or uninstalling the app can erase it. Download a backup regularly from **Settings → Backup & data**. To move to a new device, download a backup on the old one and use **Restore backup** on the new one.

This app is provided as is. The author is not responsible for safeguarding your data or for any data loss.

## FAQ

**The screen did not change after an update.**
Close the app completely (swipe it away from the app switcher) and open it twice. The version number is at the bottom of Settings, where you can also tap *Check for updates*.

**A password, address or credit-card menu pops up when I type.**
Single-line fields use a component that does not trigger autofill. If your browser still shows the menu, turn off autofill (passwords, payment methods, addresses) in its settings.

**Does it work offline?**
Yes. After the first visit and installation it works without a connection.

**How do I change the language?**
Use **Language** at the top of Settings. The first launch follows your phone’s language.

**How do I erase everything?**
Use **Erase all data** at the bottom of Settings. This cannot be undone, so back up first.

## License and changelog

Released under the [MIT License](LICENSE). The code was written and revised by the author together with Claude, Anthropic’s AI assistant. Feel free to adjust and update it as you need.

- **v6.0.0**: Simplified Chinese and English added; sample data now includes low scores and English character names; new tag picker; the draft hint moved to the top right; sturdier backup, import and error handling.
- **v5.1.0**: Sample data (a separate collection you can load or remove any time).
- **v5.0.0**: Collections, two-level tags, script series, permanent numbers, seven sort orders, Settings, tabs and filters on My Taste, equal-height picture layout, dark mode.
