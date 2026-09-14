# Go Mints — Website Redesign · Project Handoff & Build Guide
_Clear Growth · v1 · single source of truth for the GoMints PDP rebuild_

---

## 0. How to use this file (READ FIRST — new session / new machine)

This is the single source of truth. A new Claude Code session with **no prior memory** should be able to continue from here alone.

- **Project folder:** `D:\CODE CONTAINERR\WEB DEV\GOMINT` (open Claude Code here — local session, full read/write).
- **The wireframe file:** `gomintshero.html` — the growing PDP wireframe. **5 sections built so far**, in this DOM order: **Hero → Compare → Flow-state → Ingredients → Melt/power-up.** Open it in a browser to preview.
- **What's left:** the guarantee ribbon (§4 #2) + sections 6→12 (§4), then the Shopify Liquid conversion. See **§7** for the live TODO.
- **Assets in the folder:** `hero img.png` (3-pack banner, hero); `coffee icon.png` / `peppermint.png` (flavour circles); `powerup-sprite.png` (AI source sheet) + `powerup-0…5.webp` (the 6 power-up frames) + `powerup.gif` (client-share clip); `tools/process_sprite.py` (the sprite processor). Plus saved competitor pages (Hack Focus, Proper Wild, Sprinkle, Encha, Melo, Instant Hydration's `Energy+ Electrolyte Drink Mix.html`) for reference.

**First message to paste in the new session:**
> _"Read GOMINTSBUILDGUIDE.md. gomintshero.html already has Hero, Compare, Flow-state, Ingredients and the Melt/power-up sections built. Continue from §7's TODO — build the remaining sections in order, following the design system (§2), the build conventions (§8), the daily-ritual voice (§6), and matching the as-built sections (§5–§5d)."_

### The plan in one line
Build the **full multi-section wireframe** in `gomintshero.html` first (hardcoded placeholder copy is fine), **then** convert the whole thing to **Shopify Liquid sections**. The file is already structured so that conversion is clean — see **§8**.

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

> **⚠️ Direction change (2026-09-10):** the client wants **clean plain-white section backgrounds**, not the Sprinkle gradient. The hero background was switched to `var(--paper)` (#FFFFFF) and its neon glow removed. **Default new sections to white** unless a section specifically calls for a gradient/dark band (e.g. the "mechanism" or an electric CTA band may still use `--grad-electric`). The gradient tokens stay available but are no longer the default.

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

## 4. Section build order (the master map)
Build **section by section**, finishing each before the next.

1. **Hero** ✅ (done — see §5)
2. **Guarantee ribbon** — 14-day no-jitters promise (moved HIGH)
3. **Reframe vs the old habit + comparison** ✅ (done — see §5a)
4. **Flow-state / before-after** ✅ (done — see §5b) — "before a mint" vs "minutes later" bar chart (Crash/Jitters/Stress drop, instant).
   - **+ Melt / "Feel it switch on"** ✅ (done — see §5d) — the format-mechanism section: a pixel-art character **power-up flipbook** (eats mint → charges up). Currently placed after Ingredients in the DOM; reorder freely.
5. **Ingredient CAST** ✅ (done — see §5c) — "What's inside" 4-card row; doses as multiples + sensory lines
6. **"What we leave out"** — exclusion strip
7. **Taste** — sensory section
8. **The ritual** — "how to pop it" in 3 steps
9. **Proof** — video wall + big number + review wall (+ named lab)
10. **Dosage / safety honesty** — "how many can I have" (say the fear out loud)
11. **Bundle / 24-hour stack** — AOV (Energy + Sleep)
12. **FAQ** → footer

---

## 5. Hero section — AS BUILT ✅
Working code is the `SECTION: HERO` block in **`gomintshero.html`**. This describes what's actually shipped (differs from the original cloud spec — several client changes across 2026-09-10):

- **Layout:** 2-col grid — image **left**, content **right** on desktop; **stacks on mobile with the IMAGE FIRST (on top)**, then content.
- **Background:** **plain white** (`var(--paper)`). No gradient, no glow blob.
- **Font:** **Urbanist** (Google Fonts, weights 400–800). _(Was Montserrat, then switched to Urbanist for a more energetic feel.)_
- **Product image:** uses **`hero img.png`** (the 3-pack "MORE ENERGY / SHARPER FOCUS" banner). Full-width, flat: `border-radius:0`, no float animation. 4-image gallery with thumbnail strip below.
- **Content, in order:**
  - Rating row: ★★★★★ 4.8 · 70+ verified reviews
  - _(Eyebrow pill was **removed** per client.)_
  - **Headline with a TYPEWRITER rotating word:** `Switch on your` + types **beast mode. → second wind. → crunch time. → laser focus. → all-nighter.** with a blinking cyan caret. Respects `prefers-reduced-motion`.
  - **6 benefit points** with cyan→volt gradient icon circles (3D shadow), 2-col grid. Icons: battery, thumbs-up, clock, crosshair, pocket, shield.
  - **Melo-style pack picker** (integrated into hero buy box):
    - Flavor circles: Coffee + Peppermint (images: `coffee icon.png`, `peppermint.png`)
    - 3 tier rows: 1 Pack ₹250, 2 Pack ₹450 (10% off), 4 Pack ₹499 (50% off, Most Popular — default active)
    - Radio-button selection, per-unit pricing, discount + "Most Popular" badges
  - **Add to Cart** button — cyan→volt gradient, updates price on tier selection
  - **Rotating review card** directly below Add to Cart (`.hero .reviews`): 3 short "Verified Buyer" reviews auto-cycling every 5s (clickable dots, initials-avatar in brand gradient, 3D navy border + hard shadow). The bottom volt→cyan line is a **5s progress bar** (`.rv-bar`, `scaleX` fill) that restarts on each switch/dot-click. Fade via a `display:grid` stack so height never jumps. Reviews are placeholder copy → future schema blocks.
  - _(Price line, "How it works" ghost button, and reassurance chips all **removed** per client.)_
- **Mobile (≤460px):** tier rows `flex-wrap:wrap` so prices drop to second line; benefit icons shrunk; `.copy` has `overflow:hidden` to prevent horizontal bleed.
- **Global chrome above it** (announcement bar + premium header) is NOT part of this section — see **§8**.

### Typewriter (as implemented, inside the hero section's `<script>`)
```js
var words=["beast mode.","second wind.","crunch time.","laser focus.","all-nighter."];
// type char-by-char, hold ~1.3s, backspace, next word, loop.
// blinking caret via CSS @keyframes hero-blink. prefers-reduced-motion → show words[0] static.
```

---

## 5a. Compare section — AS BUILT ✅
Working code is the `SECTION: COMPARE` block in **`gomintshero.html`**.

- **Layout:** Single vertical **3D card** (thick navy border + hard offset `box-shadow:7px 9px 0 0 var(--navy)`, rounded corners 22px, white bg, max-width 720px centered).
- **Inside the card, top to bottom:**
  - **Headline:** "Quit the 3 PM crash / _without the trade-offs._" (cyan accent on second line)
  - **Comparison table:** Go Mints vs Coffee vs Energy Drinks, 6 feature rows
- **Go Mints column** has its own **3D highlight** — green gradient background (`#B8F0D0 → #DCF8A0`), thick navy `border-left`/`border-right`, rounded top/bottom on header/last row. Stands out from the table like a separate raised strip.
- **Icons:** green circle ✓ for wins, navy circle ✕ for losses.
- **Features compared:** Zero Jitters, Kicks In Under 4 Min, No Sugar Crash, Fits In Your Pocket, No Calories, Clean Energy + Focus. _(Coffee gets a ✓ on No Calories for honesty/credibility.)_
- **Mobile (≤460px):** smaller padding/fonts/icons, card shadow reduced to `5px 7px`, fits within viewport.

---

## 5b. Flow-state / before-after section — AS BUILT ✅
`SECTION: FLOWSTATE` in **`gomintshero.html`**. Layout modeled on the Proper Wild before/after bar chart; **wording deliberately avoids "trial"/"clinical"**. Framed as an **instant** shift (Go Mints works in ~4 min) — "Before a mint" vs "Minutes later", NOT a multi-week transformation (that would contradict the instant-onset positioning).

- **Signature volt→cyan gradient band** (`background:var(--grad-brand)`, navy ink) — neon-yellow glow fading into cyan, matching the button/icon gradient; a deliberate colored break between the white hero/compare sections. Bars sit on the cyan lower half so white/green stay readable.
- **2-col:** left = uppercase headline "Unlock your flow state" + a flow-state blurb + "See how it works" 3D CTA (white pill, navy border/shadow). Right = the chart. Stacks to 1-col ≤820px.
- **Chart:** Y-axis label "Level of discomfort" (rotated) + `0.0–3.0` scale with 4 navy gridlines; 3 categories (**Crash / Jitters / Stress**), each a **pair** of bars — **Before a mint** = white, **Minutes later** = green `#7BDC3E`, both navy-outlined. Legend pills top-right. Discomfort drops after → after-bars shorter.
- **Layout mechanics:** `.plot` is `position:relative` fixed-height; `.grid` (gridlines) and `.groups` (bars) are absolutely stacked with matching `inset-left` for the y-numbers; `.cats` sits under `.plot-col` with the same `padding-left` so labels align to bar groups.
- **Scroll animation:** `IntersectionObserver` adds `.flowstate.in` at 30% in view → bars grow 0→`--h` (per-bar % of the 0–3 scale) with staggered `--d`. Fires once; `prefers-reduced-motion` disables it; no-IO fallback shows bars immediately.
- ⚠️ **Placeholder data** — the before/after numbers are illustrative wireframe values; swap in real figures (or soften to a qualitative claim) before launch.

---

## 5c. Ingredients ("What's inside") — AS BUILT ✅
`SECTION: INGREDIENTS` in **`gomintshero.html`**. Layout modeled on Everyday Dose's "What's Inside" card row (chosen over Encha's single-item toggle — that's saved for the future Taste section §7).

- **White section**, centered headline "What's inside" + sub "Four clean actives. Nothing to hide."
- **4 cards** (Natural Caffeine / L-Theanine / Vitamin B6 / Vitamin B12), each = brand-gradient 3D icon circle + name + **dose pill** (exact mg + relatable multiple, e.g. "50 mg · ≈ ½ a coffee") + one-line **job** + italic **sensory line**. All-actives-visible (no tabs).
- **3D cards** (navy border + `5px 6px` hard shadow). Grid: 4-col → 2-col ≤920 → **horizontal scroll-snap carousel ≤560** (edge-bleed).
- **"See the full ingredients →"** underlined link below the row.
- Copy is original, in the daily-ritual voice. Icons are inline SVG (asset-free).

---

## 5d. "Feel it switch on" / power-up sprite — AS BUILT ✅
`SECTION: MELT` in **`gomintshero.html`**. The "format mechanism" move (§3): an **AI-generated pixel-art character power-up** (idle → pops a mint → sparks → charging aura → full-power burst → charged), the client's Dragon-Ball-style idea done as an **original mascot** (no franchise likeness). Earlier tries (mouth-melt SVG, CSS aura) were scrapped.

- **Asset pipeline:** client generates a 6-frame sprite sheet with AI (transparent PNG) → `powerup-sprite.png`. Processed by **`tools/process_sprite.py`** (Pillow+numpy+scipy; run: `python tools/process_sprite.py` from the project folder): edge-flood removes any white bg, detects the 6 frames (adaptive column threshold, equal-split fallback), trims each to its **exact pixel bbox** and re-centers into a uniform grid (center-x, **bottom-aligned** baseline). Exports both a combined `powerup.webp` **and 6 individual frames `powerup-0.webp … powerup-5.webp`** (each 343×520, ~300 KB total). Re-run it whenever the source art changes.
- **To replace ONE frame** (e.g. a redrawn pack frame as a full single image): trim + white-remove it and paste into the same 489×741→343×520 cell (bottom-aligned, center-x), save over `powerup-N.webp`. Then bump the cache-buster `?v=` in the flipbook JS. (This is how `2.png` replaced frame 1.)
- **Client share:** `powerup.gif` (295×423, ~217 KB) is an animated export of the sequence (dark bg, same dwell timing) for dropping into WhatsApp/email. Regenerate from the 6 frames with a short Pillow `save_all` GIF script if frames change.
- **Animation = a JS flipbook, NOT a sprite-sheet** (background-position windowing kept clipping a frame — the fix). `.melt .powerup` is a fixed box (220×334); inside is one `<img id="pu-frame">`; a small section script preloads all 6 frames then swaps `img.src` through `powerup-0…5.webp` on a **custom per-frame `dur[]`** (ms) — long dwell on the "holds the mint pack" frame (index 1: ~950ms) and a hold on "charged". `object-fit:contain` shows each frame **whole → cannot crop**. Box ratio (220/334) matches the frame ratio (343/520) so there's no letterboxing.
- Sits on a **dark radial-navy stage** (3D card) so the aura glows; "Kicks in ~4 min" badge; right column = 3 mechanism points (Works in ~4 min · No water needed · Skips the slow route). Headline "Feel it switch on".
- `prefers-reduced-motion` → the script just sets the final "charged" frame (no cycling).
- **If the source sheet changes:** re-run `process_sprite.py` (it re-exports the 6 frame files); the HTML/CSS/JS need **no edits** as long as it stays 6 frames — the flipbook just reloads the new `powerup-*.webp`. (Adjust `dur[]` only if you want different timing; the old combined `powerup.webp` is now unused.)

---

## 6. Copy bank

### Live in the hero (as built)
**Headline:** `Switch on your` + rotating: `beast mode.` `second wind.` `crunch time.` `laser focus.` `all-nighter.`

**Rating row:** ★★★★★ 4.8 · 70+ verified reviews

**6 benefit tiles** (title-only, gradient 3D icon each):
- **6 Hours of Clean Energy** (battery)
- **Zero Jitters, Zero Crash** (thumbs-up)
- **Hits In Under 4 Min** (clock)
- **Sharpens Your Mind** (crosshair/focus)
- **Pocket-Sized Boost** (pocket)
- **100% Safe, Non-Addictive** (shield-check)

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
**Done (2026-09-10):**
- [x] **Fonts** → Urbanist (switched from Montserrat for a more energetic feel).
- [x] **Hero image** → using `hero img.png` (3-pack banner), full-width + flat. 4-image gallery with thumbnails.
- [x] **Hero background** → plain white (gradient removed).
- [x] **Eyebrow pill removed** from hero.
- [x] **Premium DTC header built** (hamburger drawer + centered logo + account + cart) — see §8.
- [x] **Announcement bar** forced to a single line (nowrap + responsive font).
- [x] **File structured for Shopify** (section-scoped CSS zones + boundary markers) — see §8.
- [x] **Benefit icons** → cyan→volt gradient fill, navy border, white SVG strokes, 3D shadow.
- [x] **Add to Cart button** → cyan→volt gradient (was solid cyan).
- [x] **"How it works" ghost button removed** from hero.
- [x] **Melo-style pack picker** built and integrated into hero buy box (flavor circles + tier rows + dynamic pricing).
- [x] **Reassurance chips removed** from below Add to Cart.
- [x] **Mobile layout overflow fixed** — tier rows flex-wrap, `.copy` overflow:hidden.
- [x] **Compare section built** (§5a) — 3D card with us-vs-them table, Go Mints vs Coffee vs Energy Drinks.

**Still open:**
- [ ] Rotating words updated to: beast mode / second wind / crunch time / laser focus / all-nighter — confirm or swap.
- [ ] `hero img.png` is a full marketing banner with its own headline + stars baked in (duplicates some page messaging). Optional: replace with a transparent single-pack render later if the client prefers.
- [ ] **Build sections 2, 6→12 in order** (§4) using the design system (§2) + conventions (§8). **Done:** 1 Hero, 3 Compare, 4 Flow-state, 4+ Melt/power-up, 5 Ingredients. **Next up:** 2 Guarantee ribbon, 6 "What we leave out", 7 Taste (use the Encha layout — see §5c), 8 Ritual, 9 Proof, 10 Dosage, 11 Bundle, 12 FAQ.
- [ ] **Header nav** currently lists How it works / Ingredients / Compare / Reviews / Sleep — update the anchors once more sections have real `id`s to jump to.
- [ ] After the full wireframe is done: **convert to Shopify Liquid sections** (§8).

---

_Note: earlier cloud-session artifact links (palette board / live hero) are stale — the real source is now `gomintshero.html` in this folder._

---

## 8. Build conventions & current file structure (IMPORTANT for the next session)

The wireframe is built so it maps **1:1 to Shopify sections** with no rework. Follow these rules for every new section.

### File anatomy
`gomintshero.html` has, in order:
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
8. Keep it fast + mobile-first (test at real phone widths).

### Global header (GLOBAL zone) — as built
Premium DTC icon header, `<header class="site-header">` sticky, white translucent + blur:
- 3-column grid (`1fr auto 1fr`) → **hamburger** left, **centered logo** (`Go Mints™`, cyan), **account + cart** icons right (cart has a cyan `.cart-count` badge).
- Hamburger opens a **left slide-in drawer** (`#drawer`): scrim + panel with logo + close (×), nav links (How it works / Ingredients / Compare / Reviews / Sleep), and a footer (My account, Cart). Closes on ×, scrim click, or Escape; locks body scroll (`body.drawer-lock`). Toggle JS is right after the header markup.
- **Announcement bar** (`.promo`): navy, single line — `white-space:nowrap` + `font-size:clamp(9.5px,2.8vw,13px)`. Copy: `FREE SHIPPING ₹499+ · COD AVAILABLE · SHIPS IN 24 HRS`.

### Previewing / testing (avoid a known trap)
- Opening the file directly (double-click → `file://`) works for desktop viewing **but preview snapshots don't truly reflow**, and browser "**Fit to window**" device modes render desktop-width and just scale — so the mobile layout looks wrong even when it's correct.
- To test mobile properly: run a local server and use DevTools with a **specific device (e.g. 375px)**, not "Fit to window":
  ```
  cd "D:\CODE CONTAINERR\WEB DEV\GOMINT"
  python -m http.server 8777
  ```
  then open `http://127.0.0.1:8777/gomintshero.html`, F12 → device toolbar → iPhone SE / 375px.

### Shopify conversion (do this AFTER the full wireframe)
- GLOBAL zone → theme base CSS (`:root` tokens + utilities) + `sections/header` (announcement bar, header, drawer).
- Each `SECTION: X` block → `sections/x.liquid` with a `{% schema %}`; hardcoded text → settings; repeated items → blocks; `Add to cart` → a real product form; prices → `{{ product.price | money }}` etc.
