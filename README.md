# Studio Sam: concept dental website

A concept website for a fictional dental clinic, made by Dentalook as a showcase. The clinic, address, phone numbers, prices, reviews and credentials are invented.

- **Location (fictional):** Toronto, Ontario (fictional test)
- **Look:** Dark blue "lab at night": cobalt glows, blueprint grid, a 3D exploded implant.
- **Motion:** Mechanical: scrambled headings, magnetic buttons, stacking cards, a console-style booking wizard.
- **Photos:** free stock images from Unsplash (Unsplash License): `assets/dr-sam-cutout.png` (background removed) and `assets/dr-sam.png`. Replace them with real photos, keeping the same file names.

**Live preview:** served by GitHub Pages from the `main` branch of this repository.

## What is in this project

```
./
  index.html   the whole site: HTML, CSS (in <style>) and JavaScript (in <script>)
  assets/      the photos (PNG cutouts with transparent backgrounds, plus originals)
```

The site is one self-contained `index.html`. There is no build step, no npm and no framework. Open the file in a browser and it works. For the 3D to load reliably, serve the folder over HTTP instead of double-clicking, for example `python3 -m http.server` in the folder, then open `http://localhost:8000`.

### Outside libraries (loaded from CDNs, pinned versions)

- **Three.js r128**: the 3D scenes. `cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js`
- **GSAP 3.12.5 + ScrollTrigger**: scroll animations and entrances.
- **Google Fonts**: Archivo + Geist

### How the code is organised inside `index.html`

1. **`<style>`**: design tokens (colours, fonts, spacing) are CSS variables in `:root` at the top. Change a colour there and it changes everywhere. Then come the styles section by section, then the responsive rules (`@media`), then the reduced-motion rules.
2. **JSON-LD `<script type="application/ld+json">`**: structured data for Google (business name, address, hours, rating). Update it with real details.
3. **HTML**: hero, then four chapters, then the footer. Every section has an `id` that the navigation links to.
4. **`<script>` blocks at the end**: interface logic (menu, booking form, FAQ), the 3D scenes (Three.js), then the motion layer (GSAP). Every script checks whether its library loaded, so the content still shows if a script fails.

## Things that are demo-only (must be connected before going live)

- **Booking forms do not send anything.** Each form validates and shows a confirmation, but `submit` is stopped with `preventDefault()`. Connect it to the clinic's real booking system (for example Dentrix, NexHealth, Jane or Cliniko) or to a WordPress form plugin (Gravity Forms, WPForms, Contact Form 7) and remove the "this is a demo" text.
- **Available days and times are generated** from a pattern in JavaScript, not from a real calendar.
- **All content is placeholder**: phone numbers in the 555 range, addresses, prices, reviews, ratings and credentials. Each footer has the line "Concept website by Dentalook. Fictional clinic, created for demonstration." Remove it when real content goes in.
- **Maps are hand-drawn SVG**, not Google Maps. Swap in a real map embed if wanted.
- **No analytics or tracking yet.** Google Tag Manager, GA4, Meta Pixel and Patient Prism call tracking would be added at the WordPress level (see below).

## Putting it on WordPress: three options

### Option A: Rebuild in the page builder, using this as the design (recommended for easy editing)

Best if the clinic team will edit text and images themselves.

- Rebuild the layout in Elementor, Bricks, Breakdance or the block editor, using `index.html` as the visual and copy reference.
- Copy the CSS variables (colours, fonts, spacing) into the theme's global styles so everything matches.
- Most of the motion (fades, reveals, parallax) can be redone with the builder's motion effects, or by enqueueing GSAP and pasting the relevant GSAP code.
- Put the 3D scenes (Three.js) in a **Custom HTML block or widget**: the `<canvas>` or container element, plus the matching `<script>`, with Three.js enqueued once site-wide.
- **Trade-off:** more work up front, but staff can edit everything without touching code.

### Option B: Use the code as-is on one WordPress page (fastest)

Best if the page will stay exactly like the demo and only a developer edits it.

1. Upload the `assets/` images to the Media Library and update the `src` paths in the HTML to the new URLs.
2. Create a page using a **blank or canvas template** (no theme header and footer), for example Elementor Canvas or a custom `page-blank.php`.
3. Enqueue the libraries in the child theme's `functions.php` with `wp_enqueue_script` (Three.js, GSAP, ScrollTrigger and, for Muhannad, Lenis) and the Google Font with `wp_enqueue_style`.
4. Move the `<style>` block into a CSS file in the child theme, or into Appearance > Customize > Additional CSS. It is scoped to this design, so it may clash with theme styles; wrap it or use a blank template.
5. Paste the page HTML (everything between `<body>` and the scripts) into a Custom HTML block.
6. Move the inline `<script>` blocks into a JS file enqueued **after** the libraries, in the footer.
7. Move the `<title>`, meta description and JSON-LD into the SEO plugin (Yoast or Rank Math) settings.
- **Trade-off:** quick and pixel-perfect, but content edits mean editing HTML.

### Option C: Host the static files and link them from WordPress

Best for a demo or a campaign landing page.

- Upload the folder as-is to a subdomain or static host (Netlify, Vercel, Cloudflare Pages, or a folder on the server like `/demos/heath/`), and link to it from the WordPress site.
- **Trade-off:** zero conversion work, but the page is not edited through WordPress.

## Notes for editing

- **Text:** search for the text in `index.html` and replace it. Headlines that animate word by word are split by JavaScript at runtime, so edit the original text in the HTML.
- **Colours and fonts:** edit the CSS variables at the top of `<style>`.
- **Photos:** keep the same file names in `assets/`, or update the `src` attributes. The hero images are background-removed PNG cutouts.
- **3D:** each scene is built in code (no 3D model files). Look for the comments in the Three.js script, for example "the molar", "the arch" or "implant".
- **Accessibility and motion:** each site respects the visitor's "reduce motion" setting and has keyboard-accessible menus and forms. Please keep that when converting.
- **Performance:** 3D scenes pause when off-screen and use a lower pixel ratio on phones. Test on a mid-range phone after any changes.
