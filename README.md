# Inbet - New starter onboarding site

A single, self-contained `index.html` that turns the **Welcome to Inbet Online**
PowerPoint into a clickable, branded onboarding page. Nothing is hard-coded: the
page opens the `.pptx` in the same folder, unzips and parses it in the browser,
and builds itself from the slides. Edit the deck, refresh the page, and the site
updates.

Brand colours (blue and yellow), the Inbet logo and the slide 21/22 links are all
read straight from the deck. Long dashes are normalised to a regular `-`.

## Files

| File | Purpose |
|------|---------|
| `index.html` | The whole site - HTML, CSS and JS inlined. Open this. |
| `Welcome_Inbet_Online.pptx` | The source deck. Must sit next to `index.html`. |
| `README.md` | This file. |

## Run it (important)

The page reads the `.pptx` with `fetch()`. Browsers block that when you open the
HTML straight from disk (the `file://` protocol), so you need to serve the folder
over `http`. Pick one:

**VS Code - Live Server (easiest)**
1. Open this folder in VS Code.
2. Install the **Live Server** extension (by Ritwick Dey).
3. Right-click `index.html` -> **Open with Live Server**.

**Any static server**
```bash
# from this folder
python3 -m http.server 8080
# then open http://localhost:8080/
```

**GitHub Pages**
1. Push this folder to a GitHub repo (keep `index.html` and the `.pptx` together).
2. Repo **Settings -> Pages**, deploy from your branch / root.
3. Open the published URL.

If you open `index.html` by double-clicking it, the page shows a short message
explaining this and how to serve it - it does not fail silently.

## How the live sync works

On load, `index.html`:
1. `fetch`es `./Welcome_Inbet_Online.pptx`,
2. unzips it with **JSZip**,
3. parses the slide XML with the browser's built-in `DOMParser`,
4. renders the teams, leadership, best practices, access links and first steps.

Because it parses the real deck every time, updating the PowerPoint is all you need
to update the site - no rebuild step.

### Keep the filename (or point at another)

The page looks for `Welcome_Inbet_Online.pptx` by default. If you rename the deck,
either rename it back or pass the new name in the URL:

```
http://localhost:8080/index.html?deck=My_New_Deck.pptx
```

## Editing the deck

- Team slides: the **bold** run is the team name; the rest is its description.
- Leadership slides: the bold short code (CEO, COO...) and the "Chief ... Officer"
  line drive the card.
- Best practices: built from the Start / Mid / End of Shift SmartArt.
- Slide 21 (websites) and slide 22 (first steps): the hyperlinks are carried
  through to the page exactly as set in the deck.

Keep that structure and new content flows through automatically. Em/en dashes you
type in the deck are converted to a normal `-` on the page.

## Libraries (all public, loaded from public CDNs)

- **JSZip 3.10.1** - unzips the `.pptx` in the browser
  (`cdnjs.cloudflare.com`, loaded with a Subresource Integrity hash).
- **Google Fonts** - Space Grotesk, Inter, JetBrains Mono. If you are offline the
  page falls back to system fonts automatically.

No build tools, no framework, no other dependencies.
