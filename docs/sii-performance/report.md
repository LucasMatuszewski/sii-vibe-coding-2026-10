# sii.pl homepage - performance audit

- **Page:** https://sii.pl/ (homepage, Polish)
- **Date:** 2026-10-18
- **Tools:** Chrome 155.0.8059.80, Lighthouse 13.5.0 (CLI), Chrome DevTools MCP / CLI 1.11.0, CrUX field data (via DevTools trace summaries)
- **Audience:** developers and coding agents who will implement the fixes. Every item in the to-do list has evidence, a code example and a way to verify it.

---

## 1. Executive summary

1. **Real users are fine today.** CrUX field data (p75, URL scope, 28 days) passes all Core Web Vitals on phone and desktop: LCP 1.07 s / 1.12 s, INP 121 ms / 54 ms, CLS 0.00 / 0.02. Nothing here is an emergency.
2. **The page is fragile on slower devices.** In the lab, on a mid-range phone profile (4x CPU slowdown, ~1.6 Mbps), Lighthouse Performance is **49/100** (median of 3). LCP is 4.5 s and Total Blocking Time is **1.9 s**. Desktop scores **89/100**.
3. **The bottleneck is main-thread work, not the server or the network.** TTFB is about 220 ms (Cloudflare, good). On mobile, 75% of LCP time is *element render delay*: the hero image is downloaded early but cannot be painted while the main thread is busy.
4. **Three things create that main-thread work:**
   - a **2.5 MB (uncompressed) CSS bundle**: 259 KiB on the wire, 97% unused on this page, render-blocking. It produces about 42,000 style-rule objects in memory.
   - about **3 MB of uncompressed JavaScript**, including jQuery UI, Backbone and the WP REST client, which the homepage does not need.
   - **marketing tags**. After cookie consent they add about 1.6 MB and **about 18.6 s of main-thread time at 4x CPU**. The biggest are GTM/gtag (8.1 s), the Reddit pixel (7.6 s) and Facebook (1.5 s).
5. **Fonts are oversized.** 5 Lato weights at about 116 KiB each, 582 KiB in total, all at the highest fetch priority. A real subset with 2-3 weights would be about 60-90 KiB.
6. **Interactions are slow on mid-range phones.** At 4x CPU, opening the mobile menu takes 536 ms and closing search takes 480 ms; both should be under 200 ms. Almost all of it is *presentation delay*: style and layout on a 3,400-element DOM with 2.5 MB of CSS.
7. **No memory leak.** After 50 scripted interactions the JS heap changed by -143 kB and detached DOM is 3.3 kB. The heap is large (about 35 MB used, 63 MB snapshot) because of CSS rules and compiled JS, not because of leaks.

**Verdict:** focus on (a) the CSS bundle, (b) homepage JS that is not needed, and (c) tag governance in GTM. These three items move LCP, TBT and INP together. Section 9 has the to-do list, highest priority first.

---

## 2. Methodology and test conditions

This follows the measurement workflow from `addyosmani/web-quality-skills` (performance, core-web-vitals) and `cloudflare/skills@web-perf`, plus the Chrome DevTools MCP skills (`debug-optimize-lcp`, `memory-leak-debugging`):

- field data first, lab data for diagnosis
- at least 3 runs, reported as median and range
- conditions stated explicitly
- each trace insight analyzed only for the metric that fails

| # | Run | Browser / profile | Device + throttling | Cache | Consent state | Purpose |
|---|---|---|---|---|---|---|
| A | Lighthouse CLI x3 | Headless Chrome, clean temp profile, no extensions | Mobile: Moto G Power class, simulated 4x CPU, 150 ms RTT, ~1.6 Mbps | Cold | Banner shown, **no consent** (anonymous visitor) | Baseline score, budgets, opportunities |
| B | Lighthouse CLI x3 | Same | Desktop preset (no CPU slowdown, 40 ms RTT, 10 Mbps) | Cold | No consent | Desktop baseline |
| C | DevTools trace | **Lucas's real Chrome profile** (via `--autoConnect`) | Desktop 1920 px, no throttling | Cold + warm | Consented (all categories) | Real returning-visitor experience |
| D | DevTools trace | Real profile | Mobile emulation 412x915, Slow 4G, 4x CPU | Cold | Consented | **Discarded for attribution:** browser extensions used about 8.7 s of main thread |
| E | DevTools trace | **Isolated browser context** (no extensions), consent cookies copied | Mobile 412x915, Slow 4G, 4x CPU | Cold | Consented | Consented mobile visitor without extension noise |
| F | Interaction trace + Event Timing | Isolated context | Mobile, 4x CPU | Warm | Consented | INP per interaction |
| G | Heap snapshots x2 | Isolated context | Mobile viewport | Warm | Consented | Leak check (baseline, then 10x menu, slider and search) |
| H | LCP candidate log | Clean headless Chrome | Desktop 1350 px, and mobile with 4x CPU + Fast 4G | Cold | No consent | Root-cause check: slider gating vs main-thread contention |

**Logged-in state:** sii.pl has no logged-in mode. The only link is "Zaloguj się do MySii", which goes to a separate application (`my.sii.pl`) that was out of scope. What differs between visitors is **cookie consent**: it adds 11 marketing vendors. That is why runs C-G use the consented state.

**Caveats:**

- CrUX is a 28-day aggregate of real Chrome users. It will not show a fix until about 4 weeks after deployment.
- Lab traces are single sessions. Use the medians from runs A and B for before/after comparisons.

---

## 3. Scorecard

### 3.1 Field data (CrUX p75, URL scope - what real users experience)

| Metric | Phone | Desktop | Good threshold | Status |
|---|---|---|---|---|
| LCP | 1,066 ms | 1,123 ms | <= 2,500 ms | Good |
| INP | 121 ms | 54 ms | <= 200 ms | Good |
| CLS | 0.00 | 0.02 | <= 0.10 | Good |
| TTFB | 425 ms | 396 ms | <= 800 ms | Good |

### 3.2 Lab data (Lighthouse 13.5, clean profile, median of 3 runs [min-max])

| Metric | Mobile | Desktop | Good | Mobile status |
|---|---|---|---|---|
| **Performance score** | **49** [41-50] | **89** [88-90] | >= 90 | Poor |
| FCP | 3,129 ms [3,101-3,136] | 802 ms [768-1,263] | <= 1,800 ms | Needs improvement |
| LCP | 4,495 ms [4,429-7,305] | 2,028 ms [1,507-2,063] | <= 2,500 ms | Poor |
| TBT (lab proxy for INP) | **1,901 ms** [1,875-2,049] | 89 ms [31-99] | <= 200 ms | Poor |
| CLS | 0.000 | 0.021 | <= 0.10 | Good |
| Speed Index | 4,033 ms [3,648-4,094] | 1,366 ms [1,284-1,540] | <= 3,400 ms | Needs improvement |
| TTFB | 220 ms [206-231] | 202 ms [107-237] | <= 800 ms | Good |
| Accessibility | 95 | 94 | | |
| Best Practices | 81 | 81 | | |
| SEO | 100 | 100 | | |

### 3.3 Field vs lab

| Field | Lab | Interpretation |
|---|---|---|
| Good (all CWV) | Poor on mobile (LCP 4.5 s, TBT 1.9 s) | The typical sii.pl visitor has a fast device (B2B, desktop-heavy), so p75 hides the slow tail. The page fails on mid-range Android phones and in the PageSpeed Insights score. Do not claim that users are currently failing CWV; do claim that the page has little safety margin. |

---

## 4. LCP deep dive

The LCP element is the hero image of the first slider slide:

- desktop: `1920x920_top-Remek-AIs.webp`
- mobile: `top-Remek-AI-mobs.webp` (24 KiB), selected through `<picture>`

The `<img>` already has `fetchpriority="high"` and `data-no-lazy="1"` (good), but no `width`/`height` and no `srcset`.

### 4.1 LCP subparts (ideal: TTFB ~40%, load delay <10%, load duration ~40%, render delay <10%)

| Context | LCP | TTFB | Load delay | Load duration | **Render delay** | Render delay share |
|---|---|---|---|---|---|---|
| Field, phone (CrUX p75) | 1,066 ms | 425 ms | 126 ms | 136 ms | 414 ms | 39% |
| Field, desktop (CrUX p75) | 1,123 ms | 396 ms | 83 ms | 129 ms | 717 ms | **64%** |
| Lab C: desktop, real profile, warm | 968 ms | 102 ms | 18 ms | 47 ms | 801 ms | **83%** |
| Lab E: mobile 4x/Slow 4G, consented, no extensions | 6,816 ms | 113 ms | 633 ms | 1,200 ms | 4,871 ms | **71%** |
| Lab D: mobile 4x/Slow 4G, real profile (with extensions) | 8,097 ms | 136 ms | 762 ms | 1,141 ms | 6,058 ms | 75% |

In every context the dominant subpart is **element render delay**: the image has been downloaded, but the browser cannot produce a frame that shows it. TTFB and download time are already small.

### 4.2 Root cause (verified in run H)

- **Hypothesis rejected:** the slider was suspected of hiding the hero until slick.js initializes. It does not: the theme keeps slide 1 visible before init (`.nsw-o-top-lead__overlay:not(.slick-initialized) .nsw-o-top-lead__slide:nth-child(n+2){display:none}`). On desktop (clean profile) the hero is painted at FCP (252 ms), long before slick initializes (760 ms).
- **Confirmed:** on mobile at 4x CPU, FCP happens at 744 ms but the hero image is painted only at **2,496 ms** (1.75 s later). The image is in cache by then. Slick initializes at 4,296 ms, so it is not the cause. The main thread is busy with:
  - style and layout for the 2.5 MB stylesheet: 1.9 s of "Style & Layout" in Lighthouse mobile
  - the synchronous `jquery.min.js` in `<head>`
  - inline scripts in the 624 KB HTML: the document itself accounts for 1.9 s of bootup time
  - GTM / gtag.js, which start immediately
- **Desktop field:** render delay is 717 ms (64%). It has the same cause, made worse in consented sessions, where GTM starts 11 vendor tags during load.

**What fixes LCP:** less CSS (P0-1), less early JS (P0-2, P1-1), and tags moved off the critical path (P0-3). Compressing the hero image more does not help much, because load duration is already small.

---

## 5. JavaScript, main thread and third parties

### 5.1 Main-thread breakdown (Lighthouse mobile, clean, 4x CPU) - total 6.0 s

| Category | Time |
|---|---|
| Script evaluation | 2,581 ms |
| **Style & layout** | **1,903 ms** |
| Other | 595 ms |
| Script parsing & compile | 421 ms |
| Parse HTML & CSS | 301 ms |
| Rendering | 124 ms |
| Garbage collection | 35 ms |

### 5.2 Biggest scripts by CPU time (Lighthouse bootup-time, mobile)

| Script | Total CPU | Scripting |
|---|---|---|
| `https://sii.pl/` (HTML document, inline scripts, parse) | 1,909 ms | 42 ms |
| `gtm.js?id=GTM-NK7PKBQ` | 814 ms | 688 ms |
| `gtag/js?id=G-X97HN13VEE` (GA4) | 476 ms | 364 ms |
| `global.c160c083....min.js` (theme) | 454 ms | 331 ms |
| `slidersBuild.7006a34....min.js` (theme) | 345 ms | 110 ms |
| `gtag/js?id=AW-825630819` (Google Ads) | 334 ms | 285 ms |
| `gtag/js?id=AW-969578650` (Google Ads) | 304 ms | 250 ms |
| `complianz.min.js` (cookie banner) | 173 ms | 58 ms |
| `wp-api.min.js` (WP REST client) | 133 ms | 102 ms |
| `WebTracking.bundle.js` (Dynamics 365 Customer Insights) | 129 ms | 110 ms |

The longest single task is `jquery/ui/draggable.min.js` at **427 ms**. jQuery UI draggable, resizable, dialog, button, checkboxradio and controlgroup are all loaded on the homepage. So are Backbone, Underscore and `wp-api`.

### 5.3 Third parties - anonymous vs consented visitor

| Vendor | Anonymous transfer | Consented transfer | Consented main thread (4x CPU) |
|---|---|---|---|
| Google Tag Manager / gtag (GA4 + 3 Google Ads IDs) | 686 KiB | **1.1 MB** | **8,144 ms** |
| Reddit pixel | - | 25 KB | **7,589 ms** |
| Facebook pixel | - | 199 KB | 1,542 ms |
| Bing Ads (UET) | ~0 | 35 KB | 336 ms |
| Azure / Dynamics 365 WebTracking | 100 KiB | 103 KB | 258 ms |
| openai.com (`oaiq.min.js`) | - | 28 KB | 220 ms |
| Cloudflare Web Analytics | 10 KiB | 10 KB | 184 ms |
| LinkedIn Insight | - | 22 KB | 119 ms |
| Leadfeeder | - | 13 KB | 110 ms |
| Google/DoubleClick Ads | ~0 | 21 KB | 66 ms |
| Apollo.io | - | 2 KB | 11 ms |
| **Total** | **~797 KiB** | **~1.56 MB** | **~18.6 s (about 4.6 s on an unthrottled CPU)** |

The `<head>` of a consented visit has **29 async third-party scripts**, including **5 separate `gtag/js` loads** for one GA4 and several Google Ads IDs.

**Reddit outlier:** the Reddit pixel's main-thread time is unusually high (7.6 s at 4x CPU). Check it in GTM (trigger firing repeatedly? history-change trigger?) before deciding to keep it.

---

## 6. Payload vs performance budget

Budget guardrails are from `addyosmani/web-quality-skills/performance`, for a typical content page. Actuals are from the clean mobile Lighthouse run, measured as transfer size.

| Resource | Budget | Actual | Over budget | Uncompressed |
|---|---|---|---|---|
| Total page weight | 1,500 KiB | 2,207 KiB | +47% | - |
| JavaScript | 300 KiB | **1,014 KiB** (34 files) | **+238%** | 3,021 KiB |
| CSS | 100 KiB | 282 KiB (6 files) | +182% | **2,563 KiB** |
| Fonts | 100 KiB | **582 KiB** (5 files) | **+482%** | 583 KiB |
| Images (above the fold) | 500 KiB | ~259 KiB | OK | - |
| Third-party (anonymous) | 200 KiB | 797 KiB | +299% | - |
| Third-party (consented) | 200 KiB | ~1,560 KB | +680% | - |
| HTML document | - | 47 KiB | - | **595 KiB** (178 inline SVGs, 22 KiB inline JS) |
| DOM elements | < 1,500 | 3,408-3,576 | +127% | - |

### 6.1 Specific waste found

| Item | Evidence | Saving |
|---|---|---|
| `public2.1792065785088.bundle.css` | 259 KiB transfer, 97% unused on homepage; render-blocking, 1,922 ms est. on mobile | ~225 KiB; FCP/LCP -1.1 s (Lighthouse estimate) |
| Lato fonts: Regular, Medium, Heavy, Bold, Light | ~116 KiB each despite `subset-` prefix, all `VeryHigh` priority | ~490-520 KiB |
| Unused JS (GTM/gtag x4, WebTracking, theme `global.js`) | Lighthouse unused-javascript | ~329 KiB |
| Below-the-fold JPGs served at 1280 px to 412 px slots | e.g. `Marcin-Mosiolek-ABSL-Summit-2026.jpg` 685 KB saving, `Siitalks_AI.jpg` 166 KB, `Sii-Talks-episode-2.jpg` 124 KB | ~1 MB (ImageDelivery insight) |
| `WebTracking.bundle.js` | Cache TTL **0 s**, 100 KiB, re-downloaded every visit | 100 KiB per repeat visit |
| WooCommerce blocks CSS, block-library CSS, simple-lightbox CSS, wp-emoji | Loaded on a page that has no shop, no blocks gallery, no emoji | ~20 KiB + 4 requests |

---

## 7. Interactivity (INP)

Measured with the Event Timing API in run F (isolated context, mobile, 4x CPU, consented). Worst event per interaction:

| Interaction | Duration | Input delay | Processing | **Presentation delay** | Status |
|---|---|---|---|---|---|
| Open mobile menu (hamburger) | **536 ms** | 3 ms | ~0 ms | 533 ms | Poor (> 500 ms) |
| Close search overlay | 480 ms | 8 ms | ~0 ms | 472 ms | Needs improvement |
| Slider "next" (1st) | 432 ms | 44 ms | ~0 ms | 388 ms | Needs improvement |
| Slider "next" (2nd slider) | 352 ms | 6 ms | ~0 ms | 346 ms | Needs improvement |
| Open submenu, back, close menu, open search | < 16 ms or not recorded | | | | Good |

In the separate interaction trace, the worst `pointerdown` was **679 ms**: input delay 44 ms, processing 481 ms, presentation delay 154 ms.

**Diagnosis:** JS handlers are cheap. The time goes into the *next frame*: style recalculation and layout across about 3,500 elements against roughly 42,000 CSS rules. Forced synchronous layouts make it worse. The ForcedReflow insight names:

- `slidersBuild.js:116` `observerCallback`: 55-74 ms
- jQuery dimension getters: about 60 ms
- theme `global.js`: `countLines` 12 ms, `shouldFloat` 11 ms
- `complianz.min.js`: 34 ms

---

## 8. Memory

Run G: isolated context, mobile viewport, consented. Snapshot taken after load, then again after 10 repetitions of: open/close menu, slider next, open/close search (50 interactions).

| Metric | Baseline | After 50 interactions | Delta |
|---|---|---|---|
| `performance.memory.usedJSHeapSize` | 35.4 MB | 35.5 MB | +0.1 MB |
| Heap snapshot total | 63.34 MB | 63.35 MB | **-142.8 kB** (diff of allocated vs freed) |
| V8 compiled code | 13.9 MB | 13.3 MB | |
| Strings | 4.0 MB | 4.0 MB | |
| DOM elements | 3,576 | 3,576 | 0 |
| Detached DOM retained | - | 3.3 kB (26 objects) | negligible |
| Objects retained by event handlers | - | 96.8 kB shallow | normal |

**Verdict: no leak.** The objects that grew are `PerformanceEventTiming`, long-animation-frame and resource-timing entries, plus accessibility-tree objects created by the DevTools snapshots. These are expected.

**Where memory goes:**

- 41,871 `StyleRule` objects (7.4 MB retained) and 34,392 `CSSStyleRule` objects (4.5 MB), all from the oversized CSS bundle
- 13 MB of compiled JS code from about 3 MB of source
- one object shape `{id, parent, previous, children, data, selector, hash, region, metadata}` with 9,221 instances (4.4 MB). It looks like a session-recording DOM mirror, probably Microsoft Clarity. This is a hypothesis: confirm with retainers before acting.

A 35 MB JS heap is high for a content homepage. It is not a leak, but on low-RAM phones it raises the risk of tab discards. P0-1 and P0-3 are the levers.

---

## 9. Prioritized to-do list

Priority = (expected impact on LCP/TBT/INP x confidence) / effort.

- **P0:** do first; largest and most certain impact.
- **P1:** high value.
- **P2:** worthwhile.
- **P3:** hygiene.

Verify every item with the same lab protocol as runs A/B: 3 runs, compare medians. Field confirmation in CrUX takes about 28 days.

| ID | Task | Metric(s) | Expected impact (lab, mobile) | Effort | Owner |
|---|---|---|---|---|---|
| **P0-1** | Split the 2.5 MB CSS bundle, inline critical CSS, defer the rest | FCP, LCP, INP, memory | FCP/LCP -1.0-1.5 s, Style & Layout -50%+, heap -7 MB | M-L | Theme (sii-sage) |
| **P0-2** | Stop loading unused WP/jQuery UI/Backbone scripts on the front end; move jQuery out of `<head>` | TBT, LCP | TBT -400-600 ms, ~15 fewer requests | S | Theme / WP |
| **P0-3** | GTM governance: one Google tag, delay non-essential pixels, fix the Reddit tag | TBT, INP, LCP (consented) | Consented main thread -50-70% | S-M | Marketing + dev |
| **P1-1** | Fonts: 2-3 weights, real Latin + Latin-Ext subset, preload one | FCP, LCP, bytes | -490 KiB, less bandwidth contention | S | Theme |
| **P1-2** | Cut presentation delay: DOM size, SVG sprite, `content-visibility`, remove forced reflows | INP | Menu/search INP from ~500 ms to < 200 ms at 4x CPU | M | Theme |
| **P1-3** | Dynamics 365 WebTracking: cache it, load after idle, remove `unload` (bfcache) | TBT, repeat visits, back/forward | Restores bfcache: instant back/forward | S (config) | Marketing ops |
| **P2-1** | Responsive images (`srcset`/`sizes`, WebP/AVIF) for news/event cards | Bytes, LCP on slow networks | ~1 MB less on mobile | S | Content / theme |
| **P2-2** | Hero slider: only slide 1 high priority, other slides lazy; add `width`/`height` | LCP (bandwidth), CLS safety | Frees ~170 KiB of high-priority bandwidth | S | Theme |
| **P2-3** | Reduce HTML weight (595 KB raw): SVG sprite, lazy mega-menu markup | Parse time, memory | -200-300 KB raw HTML | M | Theme |
| **P3-1** | Add RUM with `web-vitals` attribution build to GA4 | Observability | Per-segment p75, INP attribution | S | Dev |
| **P3-2** | Lighthouse CI budget in the deploy pipeline | Regression guard | Prevents backsliding | S | Dev |
| **P3-3** | Speculation Rules (`moderate`) for top navigation links | Next-page LCP | Near-instant next navigations | S | Dev |
| **P3-4** | A11y fixes: contrast of `.nsw-m-title-block__subtitle`; heading order (`h4.nsw-o-mobile-fixed-layer__title`) | Accessibility 95 -> 100 | | XS | Theme |
| **P3-5** | Ship the theme's `global.js` actually minified (13 KiB savings) | Bytes | Small | XS | Build |

### P0-1 - Split the CSS bundle and inline critical CSS

**Evidence:**

- `public2.1792065785088.bundle.css`: 259 KiB gzip, 2,414 KiB uncompressed
- 225 KiB (97%) unused on the homepage
- render-blocking, with a 1,170 ms FCP saving estimated on mobile
- 1.9 s of Style & Layout
- 42k style-rule objects in memory
- INP presentation delay of 350-530 ms

**How:**

1. Split the Sage/webpack build per template: `home.css`, `offer.css`, `blog.css`, `career.css`, and so on, plus a small `base.css`. Enqueue each one only where it is used.
2. Purge unused selectors at build time with PurgeCSS against the theme's Blade/PHP templates, using a safelist for classes added by JS (`slick-*`, `is-*`, `-active`, Complianz `cmplz-*`).
3. Inline the above-the-fold CSS (header, hero, first section), keeping it at or under 14 KiB compressed, and load the rest without blocking.

```php
// functions.php (theme) - per-template CSS + non-blocking load
add_action('wp_enqueue_scripts', function () {
    wp_dequeue_style('sage/public2'); // the monolithic bundle handle - check real handle name
    wp_enqueue_style('sii/base', asset('styles/base.css')->uri(), [], null);
    if (is_front_page()) {
        wp_enqueue_style('sii/home', asset('styles/home.css')->uri(), ['sii/base'], null);
    }
}, 20);

// Load non-critical stylesheets without blocking render
add_filter('style_loader_tag', function ($html, $handle) {
    if (in_array($handle, ['sii/home'], true)) {
        $html = str_replace("rel='stylesheet'", "rel='stylesheet' media='print' onload=\"this.media='all'\"", $html);
    }
    return $html;
}, 10, 2);
```

```js
// postcss.config.js - purge unused selectors (Sage / webpack)
module.exports = {
  plugins: [
    require('@fullhuman/postcss-purgecss')({
      content: ['./resources/views/**/*.php', './resources/scripts/**/*.js'],
      safelist: { standard: [/^slick-/, /^is-/, /^-/, /^cmplz-/, /^nsw-.*--/], greedy: [/slick/] },
    }),
  ],
};
```

**Verify:**

- Lighthouse mobile x3: `render-blocking-insight` < 300 ms, `unused-css-rules` < 50 KiB, Style & Layout < 900 ms
- heap snapshot `StyleRule` count < 10k
- menu-open INP < 250 ms at 4x CPU

### P0-2 - Dequeue scripts the homepage does not need; get jQuery out of the critical path

**Evidence:**

The critical chain on the homepage loads all of the following, mostly from plugins that enqueue globally:

- `jquery-ui/core`, `mouse`, `resizable`, `draggable`, `controlgroup`, `checkboxradio`, `button`, `dialog`
- `underscore`, `backbone`, `api-request`, `wp-api`
- `wp-emoji-release`
- `wc-blocks.css`, `block-library/style.min.css`

`draggable.min.js` is the longest task on the page (427 ms). `jquery.min.js` is parser-blocking in `<head>`.

**How:**

1. Find which plugin enqueues each handle: Query Monitor -> Scripts.
2. Dequeue each handle on templates that do not use it:

```php
add_action('wp_enqueue_scripts', function () {
    if (is_admin()) return;
    if (is_front_page()) {
        foreach (['jquery-ui-dialog', 'jquery-ui-draggable', 'jquery-ui-resizable', 'jquery-ui-button',
                  'jquery-ui-checkboxradio', 'jquery-ui-controlgroup', 'jquery-ui-mouse',
                  'wp-api', 'wp-api-request', 'backbone', 'underscore'] as $h) {
            wp_dequeue_script($h);
        }
        wp_dequeue_style('wc-blocks-style');
        wp_dequeue_style('wp-block-library');
    }
}, 100);

// Emoji script and styles are not needed
remove_action('wp_head', 'print_emoji_detection_script', 7);
remove_action('wp_print_styles', 'print_emoji_styles');

// Move jQuery to the footer (test inline scripts that call jQuery first)
add_action('wp_default_scripts', function ($scripts) {
    if (!is_admin()) {
        $scripts->add_data('jquery', 'group', 1);
        $scripts->add_data('jquery-core', 'group', 1);
        $scripts->add_data('jquery-migrate', 'group', 1);
    }
});
```

3. Make sure the theme bundles (`global.js`, `slidersBuild.js`) use `defer` and that no inline `jQuery(...)` runs before them. WP 6.3+ supports `wp_enqueue_script(..., ['strategy' => 'defer'])`.

**Verify:**

- `network-dependency-tree-insight` no longer lists jQuery UI, Backbone or wp-api
- long tasks list has no `draggable.min.js`
- TBT (mobile median) drops by at least 300 ms
- manual QA: newsletter form, search, cookie banner and mega menu still work

### P0-3 - Tag governance in GTM (GTM-NK7PKBQ)

**Evidence:**

- consented visit: 11 vendors, 29 async scripts in `<head>`, 5 `gtag/js` loads
- GTM/gtag: 8.1 s main thread at 4x CPU
- Reddit pixel: 7.6 s
- Facebook: 1.5 s
- anonymous visit: GTM/gtag is already 686 KiB and 1.9 s of CPU

**How:**

1. **One Google tag.** Configure GA4 (G-X97HN13VEE) and the Google Ads accounts as *destinations* of a single Google tag, not separate `gtag/js` loads. The Ads accounts loaded in the trace are AW-825630819 and AW-969578650; conversion IDs 312209121 and 760443738 also appear. This removes 3-4 downloads of about 150-190 KiB each.
2. **Delay non-essential marketing pixels** until after the page has loaded and is idle. This covers Reddit, Facebook, LinkedIn, Bing UET, Leadfeeder, Apollo, OpenAI and Clarity. In GTM, change their triggers from "All Pages - Page View" / "Consent Initialization" to a custom event fired like this:

```html
<script>
  // Fire a GTM event once the page is idle after load (or after the first interaction)
  function pushIdle() { (window.dataLayer = window.dataLayer || []).push({ event: 'page_idle' }); }
  addEventListener('load', () => ('requestIdleCallback' in window
    ? requestIdleCallback(pushIdle, { timeout: 4000 })
    : setTimeout(pushIdle, 3000)), { once: true });
</script>
```

3. **Investigate the Reddit tag.** 7.6 s of CPU means it is either firing many times (history-change or element-visibility triggers) or it is misconfigured. Use GTM Preview to count how many times it fires. Remove it if the Reddit Ads campaign is not active.
4. **Consider server-side GTM** (sGTM on Cloud Run or Stape) for GA4, Ads, Meta CAPI and LinkedIn CAPI. This moves vendor logic off the visitor's CPU.
5. Run a quarterly tag audit and remove vendors that have no active campaign.

**Verify:**

- consented mobile trace (run E protocol): `ThirdParties` main-thread total < 5 s at 4x CPU, with GTM < 3 s
- anonymous Lighthouse: `bootup-time` for googletagmanager.com < 700 ms in total
- conversion tracking still fires (GTM Preview, Google Ads tag diagnostics)

### P1-1 - Fonts

**Evidence:**

- 5 files: `subset-Lato-{Regular,Medium,Heavy,Bold,Light}.woff2`
- 115-118 KiB each, 582 KiB in total
- all requested at about 770 ms at `VeryHigh` priority on mobile, where they compete with the hero image and the CSS

**How:**

1. Reduce to 3 weights at most (for example 400, 700 and one display weight). Map Light to Regular and Heavy to Bold in CSS where the design allows it.
2. Re-subset to the characters actually used (Latin + Latin Extended-A for Polish):

```bash
pyftsubset Lato-Regular.ttf --unicodes="U+0000-00FF,U+0100-017F,U+2013-2014,U+2018-201E,U+2026,U+20AC" --flavor=woff2 --layout-features="kern,liga" --output-file=Lato-Regular-latin-ext.woff2
```

3. Preload only the font used above the fold, and keep `font-display: swap`:

```html
<link rel="preload" href="/wp-content/themes/sii-sage/resources/fonts/Lato-Regular-latin-ext.woff2" as="font" type="font/woff2" crossorigin>
```

```css
@font-face { font-family: "Lato"; src: url(.../Lato-Regular-latin-ext.woff2) format("woff2"); font-weight: 400; font-display: swap; unicode-range: U+0000-017F, U+2013-20AC; }
```

**Verify:** Font transfer < 100 KiB in total; at most 1 font request at `VeryHigh` priority.

### P1-2 - Interaction responsiveness (INP presentation delay)

**Evidence:**

- menu open 536 ms, search close 480 ms, slider 352-432 ms at 4x CPU; all presentation delay
- DOM: 3,408-3,576 elements, of which 178 are inline `<svg>`
- forced reflow in `slidersBuild.js:116` and `global.js` (`countLines`, `shouldFloat`)

**How:**

1. Keep the closed mega menu and off-screen sections out of style and layout work:

```css
.nsw-o-mobile-menu:not(.is-open),
.nsw-o-search-mobile:not(.is-open) { content-visibility: hidden; }
.home main > section:nth-of-type(n+3) { content-visibility: auto; contain-intrinsic-size: auto 800px; }
```

2. Replace repeated inline SVG icons with a sprite and `<use>`. This cuts DOM nodes and HTML size:

```html
<svg class="icon" aria-hidden="true"><use href="/wp-content/themes/sii-sage/dist/icons.svg#angle-right"></use></svg>
```

3. In `slidersBuild.js` `observerCallback` and `global.js` `countLines`, batch DOM reads before writes. Better still, compute layout once with a `ResizeObserver` and write in `requestAnimationFrame`:

```js
// before: read-after-write in a loop (forced reflow)
items.forEach(el => { el.style.height = 'auto'; heights.push(el.offsetHeight); });
// after: write all, then read all in the next frame
items.forEach(el => { el.style.height = 'auto'; });
requestAnimationFrame(() => { const heights = items.map(el => el.offsetHeight); /* ... */ });
```

4. In menu, search and slider click handlers, toggle a single class on a high-level container rather than many inline styles. If a handler still does a lot of work, yield after the visual update with `await scheduler.yield()`, with `setTimeout` as a fallback.

**Verify:** Re-run run F: every interaction under 200 ms at 4x CPU; `ForcedReflow` insight under 20 ms in total.

### P1-3 - Dynamics 365 Customer Insights WebTracking

**Evidence:**

- `cxppeur1rdrect01sa02cdn.blob.core.windows.net/.../WebTracking.bundle.js`: 100 KiB, `Cache-Control` TTL **0**
- registers an `unload` listener, which:
  - makes the page **ineligible for the back/forward cache** (Lighthouse `bf-cache`)
  - triggers a deprecation warning (Best Practices 81)
  - causes the console error "Permissions policy violation: unload is not allowed"
- `ws-tracking.js` (mktdplp102cdn) is the longest critical chain (820 ms)

**How:**

- Load the Dynamics script from GTM after `page_idle` (see P0-3).
- Ask the Dynamics admin to switch to the current loader, which uses `pagehide` / `visibilitychange`, or self-host and cache the bundle for 1 day or more.
- Optionally block `unload` site-wide so that no script can opt the page out of bfcache:

```
Permissions-Policy: unload=()
```

The console reports "Permissions policy violation: unload is not allowed", so a policy may already apply on some responses. Confirm that from the response headers. Either way, Lighthouse reports that the main-frame `unload` handler blocks bfcache, so removing that listener is the fix.

**Verify:**

- Lighthouse `bf-cache` audit passes
- Best Practices returns to 92 or above
- DevTools > Application > Back/forward cache > "Test" reports success

### P2-1 - Responsive images for news and event cards

**Evidence:** The ImageDelivery insight reports about 1 MB of savings. For example, `Marcin-Mosiolek-ABSL-Summit-2026.jpg` is 1280x573 but displayed at 412x246, and the JPG could be WebP.

**How:** Render cards with `wp_get_attachment_image($id, 'medium_large', false, ['sizes' => '(max-width: 768px) 100vw, 33vw', 'loading' => 'lazy', 'decoding' => 'async'])`. That outputs `srcset` automatically. Enable WebP/AVIF output (WP 6.5+ supports AVIF uploads) or use an image CDN such as Cloudflare Images / Polish.

**Verify:** ImageDelivery insight under 100 KB of savings.

### P2-2 - Hero slider loading

**Evidence:** On mobile, 3 hero images are requested at `High` priority in the first 600 ms: `top-Remek-AI-mobs.webp`, `top2_mobiles-2.webp`, and `main_page_top_optimized.jpg.webp` (145 KiB). The hero `<img>` has no `width`/`height`.

**How:**

- Keep `fetchpriority="high"` only on slide 1.
- Use `loading="lazy" fetchpriority="low"` on slides 2+. Slick's `lazyLoad: 'ondemand'` with `data-lazy` also works.
- Add `width="1920" height="920"` to the desktop image and the matching values on each `<source>`.

**Verify:** In the network list, only 1 image at High priority before LCP.

### P2-3 - HTML weight

**Evidence:** The document is 595 KiB uncompressed (47 KiB Brotli). It contains 178 inline SVGs and the full mega menu markup for desktop and mobile.

**How:**

- SVG sprite (P1-2).
- Render the mobile menu markup only once; do not duplicate it between desktop and mobile.
- Consider fetching deep mega-menu levels on first open.

**Verify:** Raw HTML under 300 KiB; DOM under 2,000 elements.

### P3 items (short)

- **RUM:** add `web-vitals` (attribution build) and send LCP, INP and CLS with their `attribution` to GA4 as events. This enables p75 per device, page and consent segment, and shows which element causes slow INP in the field.
- **Lighthouse CI:** add `lighthouserc.json` with assertions: `largest-contentful-paint` <= 3000 (mobile), `total-blocking-time` <= 600, `resource-summary:script:size` <= 500000, `resource-summary:font:size` <= 120000. Run it on every deploy against staging.
- **Speculation Rules:** prerender top navigation links with `moderate` eagerness, and exclude `/wp-admin`, cart/checkout and outbound links. Gate analytics on `document.prerendering` / `prerenderingchange`.
- **A11y:** fix the contrast of `p.nsw-m-title-block__subtitle`; change `h4.nsw-o-mobile-fixed-layer__title` to the correct heading level.
- **Build:** `global.c160c083....min.js` is not fully minified (13 KiB savings). Check the Terser config.

---

## 10. Verification plan

1. **Before changes:** keep this report's medians as the baseline (section 3.2).
2. **After each P0/P1 item:**
   - re-run `run-lighthouse` (3x mobile + 3x desktop, clean profile) and the run E consented mobile trace
   - compare medians and keep the range
   - a change counts as an improvement only if the new median is outside the old range
3. **Manual QA after each deploy:** cookie banner (accept/reject), mega menu, mobile menu, search, slider, newsletter form, language switch, and the GA4/Ads conversion check in GTM Preview.
4. **Field:** watch CrUX (PageSpeed Insights or the CrUX dashboard) and RUM (P3-1) for 28 days after deploy.

### Proposed budgets (mobile lab, median of 3)

| Metric | Today | Target after P0 | Target after P1 |
|---|---|---|---|
| Performance score | 49 | >= 70 | >= 85 |
| LCP | 4.5 s | <= 3.0 s | <= 2.5 s |
| TBT | 1.9 s | <= 800 ms | <= 300 ms |
| JS transfer | 1,014 KiB | <= 600 KiB | <= 400 KiB |
| CSS transfer | 282 KiB | <= 80 KiB | <= 60 KiB |
| Font transfer | 582 KiB | 582 KiB | <= 100 KiB |
| INP (run F, 4x CPU) | 536 ms | <= 350 ms | <= 200 ms |

---

## 11. Reproduce

Raw data lives in `raw/`, which git ignores: it is about 500 MB, and the real-profile traces contain tracking IDs.

- **Lighthouse:** `raw/lighthouse/{mobile,desktop}-{1,2,3}.report.{json,html}`, and `raw/lighthouse-summary.json` with medians and ranges
- **Traces** (open in DevTools > Performance > Load profile): `raw/traces/*.json.gz`
- **Heap snapshots** (open in DevTools > Memory > Load): `raw/memory/*.heapsnapshot`

```bash
chrome-devtools start --autoConnect
```

```bash
pnpm dlx lighthouse@13.5.0 https://sii.pl/ --preset=desktop --output=json --output=html --output-path=./desktop-1
```

```bash
chrome-devtools performance_start_trace 1 --reload true --autoStop true --ignoreCache true --filePath trace.json.gz
```

Sources: [web.dev Core Web Vitals](https://web.dev/articles/vitals), [Lighthouse performance scoring](https://developer.chrome.com/docs/lighthouse/performance/performance-scoring), [LCP breakdown](https://developer.chrome.com/docs/performance/insights/lcp-breakdown), [Chrome DevTools MCP](https://github.com/ChromeDevTools/chrome-devtools-mcp), skills: `addyosmani/web-quality-skills` (performance, core-web-vitals), `cloudflare/skills@web-perf`, ChromeDevTools `debug-optimize-lcp`, `memory-leak-debugging`.
