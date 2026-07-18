# JC STORE — Online Storefront

A public, mobile-friendly storefront that sells in English, Chinese, and Khmer with dual USD/KHR pricing, and stays in sync with `Advance_Management_System.html`.

## Files

- **`storefront.html`** — the public storefront. Open it directly, or deploy it as a static site (see below).
- **`storefront-catalog.json`** — the product catalog the storefront reads. Ships pre-filled with your current 2 demo products, 3 hero banner images, and 8 placeholder brand marks, so it works immediately.
- **`Advance_Management_System.html`** — your management system, with a new **Storefront** tab in the sidebar (below Inventory Database) for controlling everything the public site shows.

## The new Storefront tab (admin system)

Everything about the public site's content and syncing now lives in one place:
- **Store Branding** — a logo just for the storefront (independent from the admin system's own logo in Settings — upload different ones for each) and a short "About Us" blurb, shown under the logo in the storefront footer. Both logo uploads now preserve transparency, so a logo with a see-through background doesn't get a white box behind it.
- **Contact Us** — address and phone/WhatsApp number, shown in its own footer column and used for the storefront's WhatsApp order and chat-button links. Shares the same fields as **Settings → Receipt**, so editing it in either place keeps both in sync.
- **Hero Banner Images** — upload photos for the homepage slideshow directly (no more editing JSON by hand); click the × on a thumbnail to remove one.
- **Brand Partners** — upload a logo for each entry in the "Explore Our Brands" strip; no name or extra step needed, and only the logo is ever shown, both here and on the storefront (now shown at full clarity, not dimmed). Transparent PNGs work best since they're shown on a plain background.
- **Follow Us Links** — add as many links as you like (Facebook, Instagram, YouTube, TikTok, WhatsApp, a website, or anything else), each with its own icon; change the icon or URL on an existing entry any time, right in the list — no need to delete and re-add. Click an entry's icon to upload your own image instead of the built-in one. Nothing is capped at one per platform, and the column disappears from the footer until you add at least one.
- **Publish to Storefront** — one button. When the two are hosted on the same website, it saves straight into the storefront and that's the whole job — no file to download. Otherwise it downloads the catalog file as a fallback. Also where storefront orders get imported back in — see below.

This tab respects the same per-user permissions as every other tab — grant or remove it for non-admin accounts under **Settings → User Access Control**.

## How the two talk to each other

Both apps run entirely in the browser with no shared server, so there's no live connection between them by default — but publishing is one click if they're deployed where the browser can bridge the two itself.

**Catalog: admin → storefront**
1. In the management system: **Storefront → Publish to Storefront**. This exports product names in all 3 languages, prices, stock levels, images, hero banner photos, brand logos, About Us, Contact Us, and Follow Us links, and your KHQR payment details. Cost and margin fields are deliberately left out.
2. **If `storefront.html` and `Advance_Management_System.html` are hosted on the same website** (e.g. `yourstore.com/shop` and `yourstore.com/admin`), that one click is the whole job — the browser can share storage between pages on the same site, so the button saves the catalog somewhere the storefront already checks first, and nothing gets downloaded. Opening or refreshing the storefront in that same browser picks it up with nothing further to do.
3. **If they're hosted separately** (different domains, or the admin system is just a file on your computer), that same click can't reach the storefront's storage, so it downloads `storefront-catalog.json` instead — replace the copy next to `storefront.html` with it (re-upload to your host), or open the storefront, scroll to the footer, tap **Store staff: sync catalog**, and upload it there directly. The footer link is unlabeled to customers as anything but staff-only, but it isn't password protected — treat it the same as you would a backstage door.
4. Once deployed, the storefront also quietly re-checks `storefront-catalog.json` every 60 seconds on its own — see "Keeping stock in sync" below. This applies to the separately-hosted case; the same-origin auto-save is already instant.

**Orders: storefront → admin**
When a customer checks out, two things happen at once:
- If a phone number is set in **Settings → Receipt** (the same field used on printed receipts, and now also on the storefront's floating chat button), the customer gets a **"Message the Shop on WhatsApp"** button, pre-filled with the order. This is the main path for a real public store — customers are on their own devices, so there's no shared database for an order to land in automatically, and their own WhatsApp is the most reliable way to actually get it to you.
- The order is also saved on that device and downloadable as JSON. Under **Storefront → Import Online Orders**, uploading it adds the order to **Hold Orders** in POS, ready to resume and complete like any other sale. This path fits a self-order kiosk/tablet inside the shop itself, or any time someone hands you the file directly — it won't collect orders from customers browsing on their own phones elsewhere, since there's nowhere shared for those to be picked up from.

## Making it public

`storefront.html` is a single static file with no backend to run — any static host works:
- **GitHub Pages** — push `storefront.html` (renamed `index.html`) and `storefront-catalog.json` to a repo, then enable Pages in the repo settings.
- **Netlify / Vercel / Cloudflare Pages** — drag the folder containing both files into their dashboard; all three have a free tier.

Whichever you pick, re-upload `storefront-catalog.json` each time you export a fresh catalog from the admin system.

## Design notes

- Prices always show USD and KHR together, rather than a currency toggle — matching how prices are commonly tagged in Cambodian shops.
- Products are grouped into sections by category (a divider line with the category name), rather than one flat grid — categories with no products yet simply don't get a section. The category tiles and pills scroll-jump to a section rather than filtering the page.
- Customers can heart-favourite any product — from the header, the mobile dock, or the card itself — and clicking anywhere on a product's photo adds it to the cart too, not just the button. The header heart icon toggles a favourites-only view.
- The hero banner is a pure, auto-advancing photo carousel with no text over it, tall enough to read as a real centerpiece rather than a strip — tapping anywhere on it scrolls down to the products, and the dots below let you jump to a specific photo. Manage the photos from the new **Storefront** tab in the admin system.
- **"Explore Our Brands"** hides itself entirely if no brands are configured, and now shows every logo at full clarity rather than dimmed — hover still gives a small lift. Manage logos from the **Storefront** tab; the shipped catalog carries 8 abstract placeholder marks (not real logos) just to show the marquee working until you add your own.
- The footer's first column is just the logo and an optional **About Us** blurb now; **Contact Us** (address, phone) has its own column right next to it, ahead of Shop, We Accept, and Follow Us in reading order. There's no WhatsApp link in Contact Us anymore — the floating chat button already covers that, so it stayed in one place instead of two. Both About Us and the storefront's own logo are editable from the **Storefront** tab; About Us stays hidden until you write something, since it's your shop's own words and I didn't invent placeholder copy for it the way the demo brands/photos do.
- **Follow Us** in the footer works the same way — add real links in the **Storefront** tab and the matching icons appear; leave it empty and the whole column stays hidden rather than showing dead links. You're not limited to one link per platform — add two Facebook pages, three websites, whatever fits — and you can change an existing entry's icon or URL right in the list, no need to delete and re-add. Click an entry's icon there to upload your own image instead of the built-in one, for a platform that isn't in the list or just a mark you'd rather use.
- At checkout, choosing "Pay by KHQR" shows one QR at a time; if you've configured more than one bank account in **Settings → Payment/KHQR**, small pills above the QR let the customer switch between them.
- The language switcher is now three flag icons rather than a dropdown — quicker to tap through for a fixed set of three languages — and the header groups the switcher, favourites, and cart together on the right. The admin system's own language menu (top-right corner) got the same treatment — just the three flags, no text labels.
- A floating chat button opens a small panel with "Message on WhatsApp" and "Call the Shop" — using the same phone number as the receipt/checkout WhatsApp link. If no number is set, it says so rather than showing dead buttons.
- The floating scroll button flips between "go to bottom" (shown near the top of the page) and "back to top" (once scrolled down), so there's always a one-tap way to jump to either end. On mobile, a separate bottom dock (Home / Categories / Search / Favourites / Cart) stays within thumb's reach.
- **Keeping stock and categories in sync**: on top of the manual catalog export/import, the storefront quietly re-checks `storefront-catalog.json` every 60 seconds while a tab is open, and picks up any changes automatically — so if you update stock or add a category in the admin system and redeploy the file, a customer who already has the page open will see it without refreshing. If reduced stock means something already in someone's cart no longer fits, their cart is adjusted down automatically and they're shown a quick heads-up. This check is skipped while a manual "sync catalog" override is active in that browser, since that's a deliberate one-off action rather than a live file to keep re-checking.
- Category tiles, search, and the product sections are all driven by whatever's in the catalog file. Add a category or product in the admin system and export again; the storefront picks it up with no code changes.
- I tested both files end-to-end in a simulated browser rather than just reading the code back — including generating a real "Publish to Storefront" payload with the admin's own export code and confirming the storefront picks it up with no manual step (and confirming the download only kicks in when that automatic save can't happen, e.g. a full or blocked browser storage). Also covered: the storefront's full flow (catalog loading and auto-refresh, cart and reconciliation, checkout, language switching, category sections, favourites, the image carousel, click-to-add, the QR switcher, the chat panel, the flag switcher, the restructured footer, Follow Us rendering multiple links including a custom-uploaded icon) and the admin system's Storefront tab (separate storefront vs. admin logo uploads with transparency preserved, About Us, Contact Us saving without disturbing other receipt settings, adding/removing hero images and brand logos, and editing Follow Us links — including their icons — in place). I'd still suggest clicking through both yourself before they go live, since I can't see real rendering — fonts, spacing, animation — from here.
