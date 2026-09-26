# WardrobeAI

**Live app:** https://claude.ai/artifact/NxSHFTCDfSS4bKgN1G4Ufp (open while signed in to claude.ai)

A personal AI stylist that knows your real closet and helps you dress well for the office in Saudi Arabia: western business clothes during the week, Pakistani wear for Fridays, weekends and events.

It is one self-contained file, `index.html`, published as a Claude Artifact on claude.ai. Inside claude.ai the page calls Claude on the **viewer's own Claude subscription**, so there is no API key, no server and no extra cost.

## The four tabs

1. **My Closet.** Tap **Add item** and pick one photo per garment. The photo is shrunk (max 1600 px, JPEG 0.85) and uploaded. The AI fills in category, main and secondary colour, formality, season, fabric and a one-line description. You check or edit the tags, then save. The closet shows a photo grid (newest first), filter chips (All, Shirts, Trousers, Suits & Blazers, Pakistani wear, Shoes, Accessories) and an item count. Tap an item to see it large, edit its tags or delete it (with a confirm step inside the page). An empty closet shows example items, clearly labelled *Example*, which are never saved.
2. **Today.** Pick an occasion (Office day, Client meeting, Presentation, Government office visit, Casual Thursday, Friday prayer / weekend, Dinner out, Pakistani wedding / event), the weather (Hot / Mild / Cool) and an optional note. The AI returns 2–3 outfits built only from your closet: item photos side by side, a name, why it works, and a tip. If your closet can't make a full outfit, it says what's missing and links to the Shopping tab. **Try again** always asks for fresh ideas.
3. **Check me.** Upload one photo of yourself in the outfit and optionally pick the occasion. You get a score out of 10, what works, what to fix, and swaps from your own closet shown with photos. The selfie is only kept in the page's memory for that one check. It is never uploaded to storage or saved to the database.
4. **What to buy.** Tap **Analyse my closet**. The AI reads your tags (not photos, so it's fast) and returns the top 3–5 things to buy, with the gap each one fills, how many new outfits it unlocks, and a High / Medium / Low priority. It also gives a short closet health summary. The formality mix bar is counted locally from your tags.

## How to use it

1. Sign in to **claude.ai** in your phone's browser.
2. Open the artifact link above.
3. The first time you use each feature, claude.ai asks you to allow it (using Claude, storing photos, saving data). Tap **Allow**.
4. Add your clothes one at a time: one garment per photo, on a hanger or laid flat, in good light.

### Put it on your home screen

- **iPhone (Safari):** open the artifact link → **Share** → **Add to Home Screen** → name it "WardrobeAI".
- **Android (Chrome):** open the link → menu **⋮** → **Add to Home screen**.

You must be signed in to claude.ai when you open the link.

### Friends and colleagues

Each person signs in with their own claude.ai account and gets their own private closet under `data/users/<their id>/`. Nobody else can see it, not even the owner. The AI runs on each person's own subscription.

To let someone use the app, share the artifact from claude.ai:
- **Contributor** can save closet items and use all AI features.
- **Editor** is needed to **upload photos**. The platform only gives photo storage (`assets`) to people with edit rights. A Contributor can open the app but the Add button stays disabled, with a note explaining why.

## Decisions

Choices I made on my own:

- **Vanilla HTML/CSS/JS, no framework.** One file, fast to load on a phone, nothing to build.
- **Look:** cool off-white with a faint woven texture, bottle-green accent (`#1F4D3A`) and a brass detail colour for "Example" tags and labels, like a tailor's shop label. Fonts: *Bodoni Moda* for headings, *Figtree* for body text, *IBM Plex Mono* for small garment-ticket labels. Separate dark palette follows the system or claude.ai theme.
- **Filter groups:** Shirts = dress shirt, casual shirt, t-shirt, polo, sweater. Trousers = trousers, chinos, jeans. Suits & Blazers = suit, blazer, waistcoat, jacket/coat. Pakistani wear = shalwar kameez, kurta, thobe. Shoes = shoes, sandals/peshawari chappal. Accessories = tie, belt, watch, other.
- **Example items** are drawn as simple SVG garment illustrations (white and light blue shirts, navy trousers, beige chinos, navy blazer, off-white shalwar kameez, black oxfords, burgundy tie). They show by default while your closet is empty, can be hidden, and are never written to the database. Today, Check me and What to buy also work on the examples until you add your own clothes, with a clear "Example" note.
- **Upload and tagging run at the same time**, so the review card fills in quickly. If you cancel the review card, the uploaded photo is deleted so it doesn't waste storage.
- **Tagging falls back to manual.** If the AI is off, can't see photos, or can't read one, the review card still opens with default tags for you to fill in.
- **AI answers are validated in the page.** Categories and formality values must match the fixed lists. Outfit items must be real ids from your closet, and outfits with fewer than two valid items are dropped. Swaps must point at real closet items. Scores are clamped to 1–10.
- **Model tiers:** `quick` for auto-tagging. Default tier for outfits, outfit check and shopping.
- **Caching:** the first **Suggest outfits** and first **Analyse** may reuse an identical answer from the last 5 minutes (saves your usage). **Try again**, **Analyse again** and every outfit check always ask Claude fresh.
- **Stop** cancels whatever AI call is running. A new AbortController is used for every call. No automatic retries.
- **Colour spelling:** the UI uses "colour" in labels (British/Pakistani English). The database fields keep the brief's names (`colorMain`, `colorSecondary`).
- **Header label** "Office · KSA" instead of a city, since the city isn't specified.
- **Delete** removes the database record first, then the photo asset.
- **Item ids** are generated by the database (`collection.doc()`), not by the page.

## Status

- Code pushed to `main`.
- Published as a Claude Artifact with capabilities `sample`, `assets`, `db`, `user` (see link at the top).

## Roadmap to a public, sellable version

1. **Own website + backend.** Move off the artifact onto a real web app (e.g. Next.js on Vercel, or a PWA) with a backend such as Supabase or Firebase for data and photo storage.
2. **Claude API key.** Call Claude from the backend with the company's own API key. Add per-user limits and cost tracking, since usage is no longer paid by each viewer.
3. **User accounts.** Email / Google / Apple sign-in. Keep the same per-user private closet model.
4. **Payments.** Free tier (e.g. 30 items, a few suggestions a day) plus a paid plan. Use **Stripe** internationally, or **Moyasar** / **Tap Payments** for Saudi customers (mada, Apple Pay, STC Pay).
5. **Arabic and Urdu UI.** Full translations with right-to-left layout, and AI replies in the chosen language.
6. **Privacy policy and data controls.** Clear policy on photo storage, export-my-data and delete-my-account, in line with Saudi PDPL.
7. **Later:** outfit calendar (what I wore when), laundry status, weather auto-detect, sharing looks with friends, native app via Capacitor or React Native.
