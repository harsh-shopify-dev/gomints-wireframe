# Go Mints — Website Redesign · Project Handoff & Build Guide
_Clear Growth · v1 · single source of truth for the GoMints PDP rebuild_

---

## 0. How to use this file (READ FIRST — new session / new machine)

This is the single source of truth. A new Claude Code session with **no prior memory** should be able to continue from here alone.

- **Project folder:** `D:\CODE CONTAINERR\WEB DEV\GOMINT` (open Claude Code here — local session, full read/write).
- **The wireframe file:** **`index.html`** (was `gomintshero.html` — renamed for the Netlify deploy; a copy lives in `netlify-deploy/`). **The full PDP is now built.** Open in a browser to preview.
- **Current DOM order:** Header → **Hero → Feels (adrenaline card) → Flow-state (curve) → Melt (power-up) → Reviews → Leave-out → Showcase → Ingredients → FAQ → Footer-CTA → Footer.**
- **What's left:** polish per client feedback, a **site-wide no-shadow pass**, swap placeholder data/images for real, then the **Shopify Liquid conversion**. See **§7** for the live TODO.
- **Assets in the folder:** `hero img.png` + `3.png`/`4.png` (hero gallery); `sec.png` (feels/showcase pack); `coffee icon.png` / `peppermint.png` (flavour circles); `powerup-sprite.png` + `powerup-0..5.webp` + `powerup.gif` (power-up sprite); `tools/process_sprite.py` (sprite processor). Plus saved competitor pages (Hack Focus, Proper Wild, Sprinkle, Encha, Everyday Dose, Melo, Instant Hydration) for reference.

**First message to paste in the new session:**
> _"Read GOMINTSBUILDGUIDE.md. The full PDP wireframe is built in index.html. Continue from §7's TODO. Follow the design system (§2 — note the NO-SHADOWS rule), build conventions (§8), the daily-ritual MojoVibe voice (§6), and match the as-built sections (§5–§5f). We're in a client-revision loop — expect targeted change requests."_

### The plan in one line
The full multi-section wireframe is built in `index.html` (hardcoded placeholder copy). We're now **iterating on client revisions**; after that, convert to **Shopify Liquid sections** (§8) — the file is structured so conversion is clean.

### ⚠️ Client feedback log (most recent first)
- **NO SHADOWS anywhere** (new global rule). Newest sections (Feels, Flow-state) are shadow-free; the rest still use the old 3D hard-shadow style → a **site-wide removal pass is pending**.
- **Comparison table REJECTED** → replaced by the **"Feels" adrenaline card** (§5a) — quirky high-adrenaline analogies, not a spec chart.
- **Before/after bar graph REJECTED** → replaced by the **flow-state curve** (§5b), Hack-Daily-style (light, editorial), original artwork.
- **Add to Cart:** flat solid cyan — **no gradient, no shadow**.
- **Offer/tier selection:** the selected variant must be unmistakable (checkmark badge + cyan ring + outline).

---

## 1. The project

- **Client:** Go Mints (gomints.in) — functional caffeinated mints. Two products: **Energy + Focus** (day) and **Sleep** (night).
- **Goal:** rebuild the product page (PDP) as a **high-fidelity, on-brand, interactive** page that clearly out-executes the client's own wireframe and their competitors.
- **North-star aesthetic:** **Sprinkle (trysprinkle.com)** — soft, rounded, friendly, vertical blue→yellow gradients, neon-yellow accents.
- **Copy voice:** **MojoVibe** — punchy, sensory, high-personality (not spec-sheet).
- **Signature ideas:** (a) the brand's own **"switched ON"** line → a **typewriter headline**; (b) later, an **ON/OFF day↔night switch** concept linking Energy & Sleep.

---

## 2. Design System (LOCKED)

### Colour tokens (paste into `:root`)
```css
:root{
  /* brand — sampled from pack + logo */
  --cyan:#12BEE9;        /* primary */
  --cyan-2:#0AA6D6;
  --cyan-deep:#0A6E93;   /* press / hard shadow */
  --navy:#082A45;        /* ink / body text */
  --navy-2:#0E3D5E;
  --volt:#E9EF12;        /* neon yellow accent */
  --volt-deep:#CBD100;
  /* Sprinkle-style soft section washes */
  --butter:#FAF6C6;
  --sky:#E3F4FC;
  --mist:#F1FAFE;
  --cream:#FCFBF4;       /* page background (warm, not stark white) */
  --paper:#FFFFFF;
  /* neutrals / semantic */
  --grey:#5C7285; --faint:#8A9AA8; --line:#E5EAEE;
  --good:#12A05C; --bad:#E0603F;
}
```

### Gradients — ALL top → bottom (Sprinkle style)
```css
--grad-section: linear-gradient(180deg,#C7E6FB 0%, #E9F3E6 52%, #FAF3AE 100%); /* ★ default soft section bg: sky → butter */
--grad-day:     linear-gradient(180deg,#E4F7FD, #C3EAF9);
--grad-electric:linear-gradient(180deg,#18C6EE, #0B3E62);   /* bold blue, dark sections */
--grad-volt:    linear-gradient(180deg,#F3F84A, #E9EF12 60%, #CBD100);
--grad-brand:   linear-gradient(180deg,#F3F84A 0%, #12BEE9 42%, #0AA6D6 100%); /* signature neon-yellow(volt)→cyan band; navy text readable; bars sit on the cyan lower half — used by FLOWSTATE */
```
`--grad-section` was the original default soft-section background.

> **⚠️ Direction change (2026-09-10):** the client wants **clean plain-white section backgrounds**, not the Sprinkle gradient. The hero background was switched to `var(--paper)` (#FFFFFF) and its neon glow removed. **Default new sections to white** unless a section specifically calls for a gradient/dark band. The gradient tokens stay available but are no longer the default.

> **⚠️ NO SHADOWS (client rule, 2026-09-15):** do **not** use `box-shadow` / drop-shadows on new or edited elements — no 3D hard-shadow buttons, no card lift shadows. Use **borders + hairline dividers** for structure instead (see the Feels §5a and Flow-state §5b sections, which are built shadow-free). The **older sections still contain the 3D `box-shadow:Xpx Ypx 0 0 var(--navy)` style** (hero cards, compare, ingredients, reviews, etc.) — a **site-wide shadow-removal pass is still pending** (§7). The buttons in §2 below are the OLD 3D style; the Add-to-Cart is already flattened (solid cyan, no gradient, no shadow).

### Typography — LOCKED
- **Everything = Urbanist** (headings + body). Switched from Montserrat to Urbanist for a more attractive, energetic feel per client feedback.
- Headings: Urbanist **800**, `letter-spacing:-.02em`. Body: Urbanist **400–600**.
- Google Fonts import: `Urbanist:ital,wght@0,400;0,500;0,600;0,700;0,800;1,400;1,600`.
- _(Was Bricolage Grotesque + Inter Tight → Montserrat → Urbanist.)_

### Buttons & pills
- **Primary CTA:** cyan pill, navy text, hard bottom shadow `box-shadow:0 6px 0 var(--cyan-deep)`, presses down on hover.
- **Accent CTA:** neon `--volt` bg, navy text, `box-shadow:0 6px 0 var(--volt-deep)`.
- **Ghost:** transparent, `inset 0 0 0 2px var(--navy)`.
- **Pills/chips:** white or tinted, `border-radius:100px`, small bold labels.
- Global radius: buttons/pills 100px; cards 16–24px.

---

## 3. Competitor learnings — steal these moves
_(Analysed from real saved pages: Hack Focus, Proper Wild, Sprinkle, Encha, Melo.)_

| Block | Best-in-class | The move |
|---|---|---|
| Hero/buy | **Sprinkle** | offer-stack in the buy box + subscription option + a customer quote |
| Dose explanation | **Proper Wild** | specs as **relatable multiples** ("½ a coffee", "2× an espresso") |
| Big idea | **Hack Daily** | a named concept ("Flow State") → ours = "switched ON" / the switch |
| Format mechanism | **Hack Daily / Melo** | "why a mint, not a pill/coffee" — bypasses digestion, ~4 min |
| **Ingredients** | **Sprinkle** | the **ingredient CAST** — each active as a character with a one-line job |
| "What's out" | **Melo** | a **"What we leave out"** exclusion strip (✕ sugar ✕ crash ✕ jitters…) |
| Taste | **Encha** | a dedicated **sensory / "how it tastes"** section |
| Ritual | **Encha / Sprinkle** | **"in 3 simple steps"** how-to-use block |
| Proof | **Sprinkle** | video + a big number + helpful-voted reviews |
| Risk reversal | **Hack Daily / Sprinkle** | **guarantee placed HIGH**, restated as a CTA in the buyer's voice |

---

## 4. Section map — CURRENT STATE
The full PDP is built. This is what's live in `index.html`, in DOM order:

1. **Hero** ✅ (§5) — gallery + typewriter + benefits + pack picker + Add to Cart + guarantee + rotating reviews
2. **Feels — "Less coffee. More adrenaline."** ✅ (§5a) — replaced the rejected comparison table; Encha-style editorial card
3. **Flow-state curve** ✅ (§5b) — replaced the rejected bar graph; Go Mints flow zone vs coffee/energy jitters→crash
4. **Melt — "Feel it switch on"** ✅ (§5d) — pixel-art power-up flipbook (format mechanism)
5. **Reviews** ✅ (§5e) — 5-card social-proof carousel with arrows + 4.8/5 summary
6. **Leave-out — "What's not inside"** ✅ (§5e) — exclusion grid on a brand-gradient panel
7. **Showcase** ✅ (§5e) — `sec.png` pack + 4 benefit icons on a brand band
8. **Ingredients — "What's inside"** ✅ (§5c) — 4 active cards (caffeine / L-theanine / B6 / B12)
9. **FAQ** ✅ (§5e) — tabbed accordion (Product / Usage / Shipping / General)
10. **Footer CTA + Footer** ✅ (§5e) — "Still running on coffee?" + full footer

**Not built / optional (from the original plan):** a Guarantee *ribbon* (a mini guarantee block already sits under Add-to-Cart in the hero), a dedicated Taste section (Encha flavour-profile layout is a candidate), a Ritual "3 steps" block, a Dosage/safety honesty block, and a Bundle/24-hr stack (Energy + Sleep). Add these only if the client asks — the page already reads as complete.

---

## 5. Hero section — AS BUILT ✅
Working code is the `SECTION: HERO` block in **`index.html`**. This describes what's actually shipped (differs from the original cloud spec — several client changes across 2026-09-10):

- **Layout:** 2-col grid — image **left**, content **right** on desktop; **stacks on mobile with the IMAGE FIRST (on top)**, then content.
- **Background:** **plain white** (`var(--paper)`). No gradient, no glow blob.
- **Font:** **Urbanist** (Google Fonts, weights 400–800). _(Was Montserrat, then switched to Urbanist for a more energetic feel.)_
- **Product image:** uses **`hero img.png`** (the 3-pack "MORE ENERGY / SHARPER FOCUS" banner). Full-width, flat: `border-radius:0`, no float animation. 4-image gallery with thumbnail strip below.
- **Content, in order:**
  - Rating row: ★★★★★ 4.8 · 70+ verified reviews  _(eyebrow pill removed per client)_
  - **TYPEWRITER headline:** a rotating word (`Crunch time` / `Limitless energy` / `Rocket fuel`) on line 1 + a static cyan **`in a mint.`** on line 2, blinking cyan caret, respects `prefers-reduced-motion`.
  - **Description line** below the headline (2 lines, MojoVibe voice): "Coffee's whole kick in a mint — melts in seconds, hits in four minutes flat. No jitters, no crash."
  - **4 benefit tiles** (2-col), cyan-fill icon circles w/ navy border: 6 Hours of Clean Energy · Zero Jitters, Zero Crash · Hits In Under 4 Min · Pocket-Sized Boost.
  - **Pack picker** (buy box):
    - Flavour circles Coffee + Peppermint. **Selected = muted others + cyan ring/glow + scale + bold cyan label + a ✓ badge** (`.flav-opt.active::after`). Unselected are dimmed (`opacity:.55`).
    - 3 tier rows: 1 Pack ₹250 · 2 Pack ₹450 (~~₹500~~ 10% off) · 4 Pack ₹499 (~~₹1,000~~ 50% off, Most Popular — default). Prices use `.tier-was` (struck MRP) + `.tier-now` (green pay-price). **Selected tier = solid cyan-tint fill + `inset 0 0 0 2.5px` cyan outline + filled ✓ radio.**
  - **Add to Cart** — **flat solid cyan** (`background:var(--cyan)`, navy border, **NO gradient, NO shadow**), full-width, updates price on tier select.
  - **Guarantee block** below Add to Cart (`.hero .guarantee`): a rotating circular **"MONEY-BACK · GUARANTEE · 14 DAYS"** seal (SVG, `@keyframes g-spin`) + "LOVE IT OR IT'S FREE" + the refund copy. _(The "Know more" link was removed.)_
  - **Rotating review card** (`.hero .reviews`): 3 short "Verified Buyer" reviews auto-cycling every 5s (clickable dots, gradient initials-avatar). The bottom volt→cyan line is a **5s progress bar** (`.rv-bar`, `scaleX` fill) restarting on each switch/dot-click. Fade via a `display:grid` stack so height never jumps.
  - _(Price line, "How it works" ghost button, reassurance chips all removed per client.)_
  - ⚠️ Hero cards still carry the **old 3D shadow** — include in the no-shadow pass.
- **Mobile (≤460px):** tier rows `flex-wrap:wrap` so prices drop to second line; benefit icons shrunk; `.copy` has `overflow:hidden` to prevent horizontal bleed.
- **Global chrome above it** (announcement bar + premium header) is NOT part of this section — see **§8**.

### Typewriter (as implemented, inside the hero section's `<script>`)
```js
var words=["Crunch time","Limitless energy","Rocket fuel"];   // line 1; line 2 is a static "in a mint."
// type char-by-char, hold ~1.3s, backspace, next word, loop.
// blinking caret via CSS @keyframes hero-blink. prefers-reduced-motion → show words[0] static.
```

---

## 5a. Feels — "Less coffee. More adrenaline." — AS BUILT ✅
`SECTION: FEELS` in **`index.html`**. **Replaced the rejected comparison table.** The client wanted quirky, relatable **high-adrenaline analogies** instead of a spec chart. Layout modeled on **Encha's** editorial product card. **No shadows.**

- **One flat card** (thin navy border, `border-radius:24px`, **no shadow**), image left / content right (stacks ≤820px).
- **Left:** the pack (`sec.png`) on a soft `--sky` panel.
- **Right:** headline "ENERGY + FOCUS, packed in a mint" + a rotating **"CLEAN ENERGY · NO CRASH"** SVG seal (top-right) + a short 2-line description, a hairline divider, then a compact 2-column area:
  - **INTENSITY** — 4-of-5 lightning bolts filled.
  - **ENERGY PROFILE** — small bar chart: Kick-in 95 · Focus 90 · Clean 85 · Smooth 88 · Lasting 92.
  - **WHAT YOU'LL FEEL** — the analogies as a **2-col** icon list: Rollercoaster kick · Laser focus · Cold-plunge clarity · Parachute landing · Pocket-sized.
- **De-dup with the flow curve (§5b):** this card = the *solo positive experience*; the curve owns the *crash/jitters-vs-competitors* story. So "crash" framing was pulled OUT of here (profile shows "Lasting" not "Crash").
- Original icons + copy (asset-free SVG). All values are placeholder wireframe numbers.

---

## 5b. Flow-state curve — AS BUILT ✅
`SECTION: FLOWSTATE` in **`index.html`**. **Replaced the rejected before/after bar graph.** Client asked for a **Hack-Daily-style flow-state line curve** — original artwork, "much better than theirs." **Light, editorial, NO shadows** (an earlier dark "SaaS dashboard" version was rejected).

- **Light band** (`background:#F4FAFD`, navy ink). **Compact** dark headline "Unlock your **flow state**" + small grey 2-line blurb + a legend (Go Mints solid cyan / Coffee & energy drinks dashed).
- **Inline-SVG line chart** (`.fs-chart`, `max-width:600px`, tall portrait `viewBox="0 60 620 560"` so it renders big on mobile):
  - Two thin cyan **flow-zone lines** (`.fc-thresh`, no filled band — the band read as "techy").
  - **Go Mints** = thick cyan line (`.fc-gomints`, `stroke-width:9`) holding steady low in the flow zone → a ringed hero endpoint (`.fc-ring` + dot) tagged "GO MINTS".
  - **Coffee & energy drinks** = dark dashed line (`.fc-others`) spiking up past **JITTERS** then diving below into **CRASH**, ending in a dark dot.
  - Zone labels JITTERS / FLOW ZONE / CRASH + the GO MINTS tag are **all one size** (`.fc-zone` & `.fc-tag` = 32px) — cyan for the good stuff (flow zone, go mints), grey for the bad (jitters, crash). No time-axis labels (they collided).
- **Scroll animation:** `IntersectionObserver` adds `.flowstate.in` → the Go Mints line **draws itself in** (`stroke-dasharray:1;pathLength=1;dashoffset 1→0`), the dashed line + dots fade in after. `prefers-reduced-motion` shows it instantly; no-IO fallback too.
- **Curve = SVG `<path>` d-strings** — to reshape the spike/crash, edit the path coords (keep Go Mints inside y 280–440 band; labels live in the clear upper band above the line). Went through a 3-round design-QA polish; watch for label/line overlaps when editing.

---

## 5c. Ingredients ("What's inside") — AS BUILT ✅
`SECTION: INGREDIENTS` in **`index.html`**. Layout modeled on Everyday Dose's "What's Inside" card row (chosen over Encha's single-item toggle — that's saved for the future Taste section §7).

- **White section**, centered headline "What's inside" + sub "Four clean actives. Nothing to hide."
- **4 cards** (Natural Caffeine / L-Theanine / Vitamin B6 / Vitamin B12), each = brand-gradient 3D icon circle + name + **dose pill** (exact mg + relatable multiple, e.g. "50 mg · ≈ ½ a coffee") + one-line **job** + italic **sensory line**. All-actives-visible (no tabs).
- **3D cards** (navy border + `5px 6px` hard shadow). Grid: 4-col → 2-col ≤920 → **horizontal scroll-snap carousel ≤560** (edge-bleed).
- **"See the full ingredients →"** underlined link below the row.
- Copy is original, in the daily-ritual voice. Icons are inline SVG (asset-free).

---

## 5d. "Feel it switch on" / power-up sprite — AS BUILT ✅
`SECTION: MELT` in **`index.html`**. The "format mechanism" move (§3): an **AI-generated pixel-art character power-up** (idle → pops a mint → sparks → charging aura → full-power burst → charged), the client's Dragon-Ball-style idea done as an **original mascot** (no franchise likeness). Earlier tries (mouth-melt SVG, CSS aura) were scrapped.

- **Asset pipeline:** client generates a 6-frame sprite sheet with AI (transparent PNG) → `powerup-sprite.png`. Processed by **`tools/process_sprite.py`** (Pillow+numpy+scipy; run: `python tools/process_sprite.py` from the project folder): edge-flood removes any white bg, detects the 6 frames (adaptive column threshold, equal-split fallback), trims each to its **exact pixel bbox** and re-centers into a uniform grid (center-x, **bottom-aligned** baseline). Exports both a combined `powerup.webp` **and 6 individual frames `powerup-0.webp … powerup-5.webp`** (each 343×520, ~300 KB total). Re-run it whenever the source art changes.
- **To replace ONE frame** (e.g. a redrawn pack frame as a full single image): trim + white-remove it and paste into the same 489×741→343×520 cell (bottom-aligned, center-x), save over `powerup-N.webp`. Then bump the cache-buster `?v=` in the flipbook JS. (This is how `2.png` replaced frame 1.)
- **Client share:** `powerup.gif` (295×423, ~217 KB) is an animated export of the sequence (dark bg, same dwell timing) for dropping into WhatsApp/email. Regenerate from the 6 frames with a short Pillow `save_all` GIF script if frames change.
- **Animation = a JS flipbook, NOT a sprite-sheet** (background-position windowing kept clipping a frame — the fix). `.melt .powerup` is a fixed box (220×334); inside is one `<img id="pu-frame">`; a small section script preloads all 6 frames then swaps `img.src` through `powerup-0…5.webp` on a **custom per-frame `dur[]`** (ms) — long dwell on the "holds the mint pack" frame (index 1: ~950ms) and a hold on "charged". `object-fit:contain` shows each frame **whole → cannot crop**. Box ratio (220/334) matches the frame ratio (343/520) so there's no letterboxing.
- Sits on a **dark radial-navy stage** (3D card) so the aura glows; "Kicks in ~4 min" badge; right column = 3 mechanism points (Works in ~4 min · No water needed · Skips the slow route). Headline "Feel it switch on".
- `prefers-reduced-motion` → the script just sets the final "charged" frame (no cycling).
- **If the source sheet changes:** re-run `process_sprite.py` (it re-exports the 6 frame files); the HTML/CSS/JS need **no edits** as long as it stays 6 frames — the flipbook just reloads the new `powerup-*.webp`. (Adjust `dur[]` only if you want different timing; the old combined `powerup.webp` is now unused.)

---

## 5e. Reviews · Leave-out · Showcase — AS BUILT ✅
All in **`index.html`**. (Older sections — still use the 3D shadow style; flag for the no-shadow pass.)

- **`SECTION: REVIEWS`** ("Hear it from them") — 4.8/5 summary with green star tiles + "based on 70+ reviews", then a horizontal **scroll-snap carousel** of 5 review cards (gradient initials avatar, star row, quote, name · Verified Buyer) with prev/next arrow buttons. Placeholder copy.
- **`SECTION: LEAVEOUT`** ("What's *not* inside") — one card, left copy + a **brand-gradient panel** listing 8 struck-through exclusions (✕ Sugar, Aspartame, Taurine, Artificial Colors, Gluten, Fillers, Synthetic Caffeine, Preservatives).
- **`SECTION: SHOWCASE`** — full-width brand-gradient band with `sec.png` pack + a 3×2 grid of 4 benefit icons (Natural Caffeine · L-Theanine · Zero Jitters · Pocket) joined by little squiggle connectors.

## 5f. FAQ · Footer — AS BUILT ✅
- **`SECTION: FAQ`** ("Got questions?") — **tabbed** accordion: 4 category tabs (Product / Usage / Shipping / General) filter `<details>` items via a small JS toggle; `+`/`−` markers. ~13 Q&As, placeholder copy.
- **`SECTION: FOOTER CTA`** — brand-gradient band, "Still running on coffee?" + a navy "Grab Your Pack" button (anchors to `#hero`).
- **`SECTION: FOOTER`** (`.site-footer`) — dark footer: logo + social icons, 3 link columns (Quick Links / Support / Information), copyright + "Designed by Harsh".

---

## 6. Copy bank

### Live in the hero (as built)
**Headline (typewriter):** rotating `Crunch time` / `Limitless energy` / `Rocket fuel` + static cyan **`in a mint.`**
**Description line:** "Coffee's whole kick in a mint — melts in seconds, hits in four minutes flat. No jitters, no crash."

**Rating row:** ★★★★★ 4.8 · 70+ verified reviews

**4 benefit tiles** (title-only, cyan icon circle each):
- **6 Hours of Clean Energy** (battery) · **Zero Jitters, Zero Crash** (thumbs-up) · **Hits In Under 4 Min** (clock) · **Pocket-Sized Boost** (pocket)

**Pack picker:** flavours Coffee / Peppermint · tiers **1 Pack ₹250** · **2 Pack ₹450** (~~₹500~~, 10% OFF, ₹225/ea) · **4 Pack ₹499** (~~₹1,000~~, 50% OFF, ₹125/ea — Most Popular, default). Button: `Add to Cart · ₹{selected total}`.

### Retired from the hero — reuse in other sections
_(Removed from the hero per client, but good copy for the mechanism / dosage / ingredient sections.)_
- **Master subhead:** "Coffee's whole kick, folded into a mint. It melts under your tongue and lands in **four minutes flat** — no water, no jitters, and zero 3 p.m. crash to sleep off later."
- **Dose points:** ~4 minutes, dissolves under the tongue · 50 mg clean caffeine + 75 mg L-theanine · 0 g sugar, 0 crash · pocket-sized.

**Real pack claims (from the box, use across sections):** Natural Caffeine to Keep You Going · L-Theanine to Enhance Focus · Vitamin B6 & B12 to Level Up · 12 mints · Coffee flavour · Nutraceutical.

**Guarantee:** 14-day no-jitters promise — "Use it two weeks. If you don't feel the difference, message us on WhatsApp and we'll refund your first order. Keep what's left."

### Voice — MojoVibe (LOCKED) + daily-consumable positioning
Go Mints is sold as a **daily/regular ritual**, not a one-off rescue. Every section's copy should quietly reinforce *"pop one every day."*

**Voice = punchy · sensory · second-person · rhythmic · a little cheeky. Short sentences. Never a spec-sheet.**
- **Do:** sensory verbs (melt, kick, hum, quiet down); micro-sentences ("Pop it, let it melt, get to work."); before/after time framing ("Week four you forget it was ever there.").
- **Don't:** clinical / lab / "trial" words; hedgy corporate phrasing; long compound sentences; feature-dumps.

**Daily-ritual line bank (reuse across sections):** "One mint, every morning." · "Make it a habit, not a rescue." · "The daily switch." · "Your 9 a.m. ritual." · "Keep one in every pocket." · "Same time, every day — that's when it compounds."

---

## 7. Open items / TODO

**The full PDP wireframe is built.** We're in a **client-revision loop** (see the feedback log in §0). Open items:

- [ ] **⭐ Site-wide NO-SHADOW pass** — the client banned shadows. Only Feels (§5a) & Flow-state (§5b) + the flat Add-to-Cart comply. **Everything else still has `box-shadow`** (hero cards/benefits/reviews/guarantee, compare→gone, ingredients cards, reviews carousel cards, showcase icons, footer button). Strip all `box-shadow` (and hard 3D offsets) → replace with borders/hairlines. Grep the `<style>` for `box-shadow` and `0 0 var(--navy)` / `0 0 var(--cyan-deep)`.
- [ ] **Header nav labels** still say How it works / Ingredients / Compare / Reviews / Sleep — "Compare" no longer exists; update to real section `id`s (`#feels`, `#flowstate`, `#ingredients`, `#reviews`, `#faq`).
- [ ] **Placeholder content to replace before launch:** all review text, the Energy-profile & Intensity numbers (§5a), the flow-curve shape is illustrative, FAQ answers, `hero img.png` (a full marketing banner with baked-in headline/stars — consider a clean transparent pack), and `sec.png`.
- [ ] **Optional new sections** if the client asks (§4 "not built"): dedicated Taste (Encha flavour-profile layout), Ritual "3 steps", Dosage/safety honesty, Bundle (Energy + Sleep).
- [ ] **Then:** convert the whole file to **Shopify Liquid sections** (§8).

---

_Note: this is the Energy + Focus PDP. There's a separate `gomints-sleep-pdp.html` / `gomints-energy-pdp.html` (older standalone mockups) — the live build is `index.html`._

---

## 8. Build conventions & current file structure (IMPORTANT for the next session)

The wireframe is built so it maps **1:1 to Shopify sections** with no rework. Follow these rules for every new section.

### File anatomy
`index.html` has, in order:
1. Top HTML comment documenting the convention.
2. `<meta charset="utf-8">` + `<meta name="viewport" content="width=device-width, initial-scale=1">` — **both required**; without the viewport tag mobile media queries never fire.
3. Google Fonts (Urbanist).
4. One `<style>` block split into two zones (see below).
5. Markup: GLOBAL header (announcement bar + `<header>` + drawer + drawer JS), then each `SECTION` block.

### The two CSS zones
- **`GLOBAL — THEME BASE`** → `:root` tokens, reset, typography, utilities (`.btn`/`.btn.cyan`/`.btn.ghost`, `.chip`, `.eyebrow`, `.stars`, `.wrap`), and the header. **Not scoped.** → becomes the theme's base CSS + header at conversion time. Reuse these across sections; don't redefine them.
- **`SECTION: X`** → one zone per visual section. **Every selector scoped under the section's root class** (e.g. `.hero .rate`, `.hero .points`) so the block lifts out into `sections/x.liquid` with zero collisions across the 12 sections.

### Rules for each NEW section (2→12)
1. Wrap markup in boundary markers:
   `<!-- ========= SECTION: NAME ========= -->` … `<!-- /SECTION: NAME -->`, with `<section class="name" id="name" data-section="name">`.
2. Put its CSS in a matching `/* SECTION: NAME */` zone, **all selectors scoped under `.name`**.
3. **Namespace keyframes** per section (e.g. `name-fade`, like the hero's `hero-blink`/`hero-float`).
4. Reuse GLOBAL utilities (`.btn`, `.chip`, `.eyebrow`, `.stars`, `.wrap`) — don't re-style them per section.
5. Any section-local JS goes in a `<script>` **inside** that `<section>`, commented as section JS.
6. Content = hardcoded placeholder copy (from §6 or new). It becomes `{% schema %}` settings/blocks later; repeatable items (points, chips, review cards, ingredient cards) are future **blocks** — keep them as clean sibling lists.
7. **Never put a literal `-->` inside an HTML doc-comment** — it closes the comment early and leaks text onto the page.
8. **NO shadows** (client rule) — structure with borders + hairline dividers, never `box-shadow`.
9. Keep it fast + mobile-first (test at real phone widths).

### Global header (GLOBAL zone) — as built
Premium DTC icon header, `<header class="site-header">` sticky, white translucent + blur:
- 3-column grid (`1fr auto 1fr`) → **hamburger** left, **centered logo** (`Go Mints™`, cyan), **account + cart** icons right (cart has a cyan `.cart-count` badge).
- Hamburger opens a **left slide-in drawer** (`#drawer`): scrim + panel with logo + close (×), nav links (How it works / Ingredients / Compare / Reviews / Sleep), and a footer (My account, Cart). Closes on ×, scrim click, or Escape; locks body scroll (`body.drawer-lock`). Toggle JS is right after the header markup.
- **Announcement bar** (`.promo`): **volt-yellow bg, navy text**, single line — `white-space:nowrap` + `font-size:clamp(9.5px,2.8vw,13px)`. Copy: `FREE SHIPPING ₹499+ · COD AVAILABLE · SHIPS IN 24 HRS`.

### Previewing / testing (avoid a known trap)
- Opening the file directly (double-click → `file://`) works for desktop viewing **but preview snapshots don't truly reflow**, and browser "**Fit to window**" device modes render desktop-width and just scale — so the mobile layout looks wrong even when it's correct.
- To test mobile properly: run a local server and use DevTools with a **specific device (e.g. 375px)**, not "Fit to window":
  ```
  cd "D:\CODE CONTAINERR\WEB DEV\GOMINT"
  python -m http.server 8777
  ```
  then open `http://127.0.0.1:8777/index.html`, F12 → device toolbar → iPhone SE / 375px.

### Shopify conversion (do this AFTER the full wireframe)
- GLOBAL zone → theme base CSS (`:root` tokens + utilities) + `sections/header` (announcement bar, header, drawer).
- Each `SECTION: X` block → `sections/x.liquid` with a `{% schema %}`; hardcoded text → settings; repeated items → blocks; `Add to cart` → a real product form; prices → `{{ product.price | money }}` etc.
