# Test Project Outline – Module F – SwapLoop Public Website

## Competition time

Competitors will have **3 hours** to complete this module.

## Introduction

**SwapLoop** is a fictional Shanghai district project that offers safer ways to charge e-bike batteries **outdoors**. Delivery riders and private riders with compatible removable batteries can exchange them at **Battery Swap Cabinets**. E-bikes with built-in (integrated) batteries use monitored **E-bike Charging Bays**. Delivery companies can get limited priority access at busy times. The main message of the site is:

> Charge or swap — safely outside the home.

In **Module F**, you build the **public website** for SwapLoop. This module focuses on **design with HTML and CSS only**. The site explains the service, shows a few example stations, and presents offers for **individual riders** and **delivery fleets**.

Other modules in the same system (for background only — do **not** rebuild them):

| Module   | Role                                                  |
| -------- | ----------------------------------------------------- |
| Module B | SwapLoop Admin (website for staff)                    |
| Module C | Main Backend REST API                                 |
| Module D | Rider app (live availability, reservations, receipts) |

Live station filters, reservations, payments, and admin tools are **not part of this module**. Tell visitors that live availability is in the rider app; do not build app features here.

## General Description of Project and Tasks

Build a static website with several HTML pages. Summary (full details in [Requirements](#requirements)):

- **Five pages:** Home, How it works, Stations, For riders, For fleets
- **HTML + CSS only** — no JavaScript (CSS frameworks such as Tailwind or Bootstrap are allowed)
- Use the provided text, images, and icons under [`assets/`](./assets/) for all **required** content — do not change or replace the required wording
- **Responsive** layout at these fixed screen sizes: mobile (below `768px`), tablet (`768px`–`1023px`), desktop (`1024px` and above)
- Basic **accessibility** and **SEO** (search engine optimisation)

### Technology rules

| Allowed                                                                                       | Not allowed                                                    |
| --------------------------------------------------------------------------------------------- | -------------------------------------------------------------- |
| HTML5, CSS3                                                                                   | JavaScript (including `onclick` and JS frameworks)             |
| CSS frameworks (e.g. Tailwind, Bootstrap) as CSS only                                         | Calling Module C, Station Service, or any backend API          |
| SVG icons (inline or as image files)                                                          | Building the rider app or admin website                        |
| Clear multi-page HTML structure                                                               | Inventing live bay counts or fake “available now” data         |
| Text from [`assets/texts/copy-deck.md`](./assets/texts/copy-deck.md) for all required content | Changing or replacing required headlines, body text, or prices |

Use these **exact breakpoints** (browser window width):

| Name        | Range              |
| ----------- | ------------------ |
| **Mobile**  | below `768px`      |
| **Tablet**  | `768px`–`1023px`   |
| **Desktop** | `1024px` and above |

Use CSS media queries (or the matching screen sizes in a CSS framework). The layout must look clearly different in each of the three ranges.

### Vocabulary

Use these terms the same way as in Modules B–D (they also appear in the provided text file):

| Term                     | Meaning                                                      |
| ------------------------ | ------------------------------------------------------------ |
| **SwapLoop**             | Brand / product name                                         |
| **SwapLoop Station**     | Full service location (`SWAP`, `CHARGING`, or `HYBRID`)      |
| **Battery Swap Cabinet** | Machine that stores and charges swappable batteries          |
| **Battery Slot**         | One space that holds at most one battery                     |
| **E-bike Charging Bay**  | Space that charges a whole e-bike with an integrated battery |

Supported battery / connector types (do not invent other codes):

| Mode       | Supported types        | Voltage   |
| ---------- | ---------------------- | --------- |
| Swappable  | `SL-48`, `SL-60`       | 48V / 60V |
| Integrated | `GB-AC-48`, `GB-AC-60` | 48V / 60V |

Station types:

| Type       | Offers                    |
| ---------- | ------------------------- |
| `SWAP`     | Battery slots only        |
| `CHARGING` | E-bike charging bays only |
| `HYBRID`   | Both                      |

### Provided text (important)

The required wording in [`assets/texts/copy-deck.md`](./assets/texts/copy-deck.md) must appear on the site.

- Copy the required headlines, paragraphs, prices, station descriptions, titles, meta descriptions, and image `alt` text from that file. Do **not** change or replace those words and numbers.
- Small layout-only changes are allowed (for example splitting one paragraph into two lines, or putting table cells into a different layout).
- You may add **extra** text or visual elements of your own, as long as they do **not** replace, hide, or contradict the required content.
- Marking focuses on HTML/CSS design. The required text is provided so every competitor starts from the same content.

### Provided files

Use these provided assets (all files are available under [`assets/`](./assets/)):

| Path                                                                                     | Contents                                                                             |
| ---------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------ |
| [`assets/texts/copy-deck.md`](./assets/texts/copy-deck.md)                               | Page text, prices, station descriptions, titles, meta descriptions, image `alt` text |
| [`assets/images/logo-swaploop.svg`](./assets/images/logo-swaploop.svg)                   | SwapLoop logo for header / footer                                                    |
| [`assets/images/favicon.svg`](./assets/images/favicon.svg)                               | Favicon                                                                              |
| [`assets/images/hero-home.jpg`](./assets/images/hero-home.jpg)                           | Home hero image                                                                      |
| [`assets/images/station-haitang.jpg`](./assets/images/station-haitang.jpg)               | Haitang Garden East Gate photo                                                       |
| [`assets/images/station-canal-view.jpg`](./assets/images/station-canal-view.jpg)         | Canal View Delivery Hub photo                                                        |
| [`assets/images/station-morning-bridge.jpg`](./assets/images/station-morning-bridge.jpg) | Morning Bridge Charging Court photo                                                  |
| [`assets/images/og-default.jpg`](./assets/images/og-default.jpg)                         | Open Graph image for Home                                                            |
| [`assets/images/map-vignette.svg`](./assets/images/map-vignette.svg)                     | Optional stylised district map                                                       |
| [`assets/images/priority-windows.svg`](./assets/images/priority-windows.svg)             | Optional priority-windows diagram for fleets                                         |
| [`assets/icons/`](./assets/icons/)                                                       | SVG icons: swap, charge, hybrid, rider, fleet, safety                                |
| [`assets/videos/home-loop.mp4`](./assets/videos/home-loop.mp4)                           | Optional short Home loop video                                                       |

### Suggested time plan

| Block | Focus                                                 | Approx. time |
| ----- | ----------------------------------------------------- | ------------ |
| A     | Common header/footer/nav, Home, How it works          | ~1 hour      |
| B     | Stations + harder CSS techniques                      | ~1 hour      |
| C     | For riders, For fleets, accessibility/SEO, animations | ~1 hour      |

## Requirements

The sections below are the **minimum** requirements. You may add extra text or visual elements, as long as they do **not** overwrite, hide, or contradict these requirements. Required page text, titles, meta descriptions, and image `alt` text must still match [`assets/texts/copy-deck.md`](./assets/texts/copy-deck.md).

### Site structure and common layout

#### Pages

Deliver **exactly five** HTML pages. File names may vary, but every page must be reachable from the main navigation, and all internal links must work.

| Page         | Purpose                                               |
| ------------ | ----------------------------------------------------- |
| Home         | Introduce SwapLoop and link to riders and fleets      |
| How it works | Explain swap vs charge in clear steps                 |
| Stations     | Station types + **exactly three** example stations    |
| For riders   | Offers for individual riders (pay-as-you-go)          |
| For fleets   | Plans for delivery partners and priority time windows |

#### Skip to main content (accessibility)

For accessibility, every page starts with a “Skip to main content” link that jumps to the main content area. This helps keyboard and screen-reader users skip the repeated header and navigation on each page.

#### Header and logo

Every page has a site header. Show the **SwapLoop** name or logo clearly ([`assets/images/logo-swaploop.svg`](./assets/images/logo-swaploop.svg)). The brand should be easy to see at the top of the page, not only in small footer text.

#### Main navigation

Every page has main navigation to all five pages. Use the navigation labels from the text file. Make the current page clear so visitors know where they are.

#### Footer

Every page has a footer. Use the pilot credit and rider-app line from the text file. Also add links to For riders and For fleets so visitors can reach those offers from any page.

#### Responsive common layout

The same header, navigation, and footer must work at all three breakpoints. On mobile, a simple stacked or compact menu is fine. Use CSS only — no JavaScript.

### Home

#### Hero

At the **top of the page** (before scrolling), introduce SwapLoop. Show the brand, the main headline, the supporting sentence, and the two main links or buttons (**For riders** / **For fleets**) from the text file. Use [`assets/images/hero-home.jpg`](./assets/images/hero-home.jpg) as the main large image, with the provided `alt` text. Keep this first screen clear: brand, one message, short support text, and the two audience links.

#### Why outdoor charging

Lower on the page, explain why outdoor charging matters. Use the “why outdoor charging” paragraph from the text file. Keep a calm, practical tone — do not replace this text with a dramatic story about accidents.

#### Three-step preview

Show a short preview of how SwapLoop works in three steps, using the three-step teaser from the text file (find a station → reserve in the rider app → swap or charge and collect). Add a clear link to the full How it works page so visitors can read more.

#### Featured station teaser

Preview one real example station: Haitang Garden East Gate (`HYBRID`). Use the featured station teaser text from the text file, and link to the Stations page. You may show [`assets/images/station-haitang.jpg`](./assets/images/station-haitang.jpg) here as a supporting image.

#### Optional home video

You may place a short looping video on Home using [`assets/videos/home-loop.mp4`](./assets/videos/home-loop.mp4). Put it **below the hero**, in the lower part of the page — for example after “Why outdoor charging” or after the three-step preview. Do **not** put it in the first screen in place of the required hero image, headline, and buttons.

### How it works

#### Intro

Start the page with the How it works intro from the text file. It should make clear that SwapLoop supports two paths: battery swap for compatible removable packs, and charging bays for e-bikes with integrated batteries.

#### Three steps

Show the full three-step process from the text file: find a station → reserve in the rider app → swap a compatible battery **or** charge in a bay and collect the bike. Present the steps as a **horizontal timeline on desktop** and a **vertical timeline on mobile/tablet** (CSS layout only).

#### Comparison

Compare the two options side by side using the comparison content from the text file: removable batteries (`SL-48` / `SL-60`) versus integrated batteries (`GB-AC-48` / `GB-AC-60`). Use a table or two-column layout so visitors can see who each option is for and what happens at the station.

#### Safety note

Show the safety note from the text file: batteries are checked before they go back into use, charging bays are monitored, and partner priority never skips compatibility or safety checks.

#### Rider app line

End with (or clearly include) the rider-app line from the text file: live reservations and availability are in the SwapLoop rider app. This page explains the service; it does not implement reservations.

### Stations

#### Station types

Explain the three station types (`SWAP`, `CHARGING`, `HYBRID`) using the text from the text file and the icons in [`assets/icons/`](./assets/icons/) (`icon-swap`, `icon-charge`, `icon-hybrid`).

Implement this section as **CSS-only tabs** (no JavaScript). Do not build a full city directory of every station.

#### Featured stations

Show **exactly three** featured stations as richer sections (not a huge list). For each station, use the photo, type label, address, opening hours, “who it is for” text, and `alt` text from the text file. Do not invent live slot counts or “available now” numbers.

| Name                          | Type       | Hours       | Image                                                                      |
| ----------------------------- | ---------- | ----------- | -------------------------------------------------------------------------- |
| Haitang Garden East Gate      | `HYBRID`   | 00:00–24:00 | [`station-haitang.jpg`](./assets/images/station-haitang.jpg)               |
| Canal View Delivery Hub       | `SWAP`     | 05:00–23:30 | [`station-canal-view.jpg`](./assets/images/station-canal-view.jpg)         |
| Morning Bridge Charging Court | `CHARGING` | 06:00–22:00 | [`station-morning-bridge.jpg`](./assets/images/station-morning-bridge.jpg) |

#### Coverage

Include the coverage sentence from the text file: some neighbourhoods are **“not yet covered”**, the network is expanding, and visitors should check the rider app for live stations. Do not call uncovered areas “unsafe” or “non-compliant”.

#### Coming soon (optional)

You may add the Jade Lane “coming soon” line from the text file as a short note about future coverage. Do not turn it into a fourth full station profile with invented details.

#### Optional map

You may add the stylised district map [`assets/images/map-vignette.svg`](./assets/images/map-vignette.svg) to support the station story. This is a static image, not a live map.

#### Featured stations — desktop image layout

On **desktop** (`1024px` and above), each featured station section must use an overlapping image layout: the station photo overlaps the text block or extends to the edge of the content area (a “bleed” / overlap effect). On mobile and tablet, a simpler stacked layout (image above or below text) is required.

### For riders

#### Intro

Open with the For riders intro from the text file. This page is for private riders and delivery riders who pay as individuals (pay as you go), not for company fleet subscription plans.

#### Pay-as-you-go prices

Show the pay-as-you-go prices in a clear table (or similar layout) using the exact amounts from the text file (whole yuan, CNY). Do not change these numbers.

On **desktop**, the price table (or price comparison block) must use a **sticky column or sticky table header** that stays visible while the visitor scrolls the rest of the For riders content.

| Service           | Compatibility | Price |
| ----------------- | ------------- | ----- |
| Battery swap      | `SL-48`       | ¥5    |
| Battery swap      | `SL-60`       | ¥7    |
| E-bike bay charge | `GB-AC-48`    | ¥3    |
| E-bike bay charge | `GB-AC-60`    | ¥4    |

#### Pay per use

Highlight the pay-per-use / no monthly contract line from the text file so visitors understand there is no required subscription for public riders.

#### Payment

Include the Alipay marketing line from the text file as product information only. Do not build a real payment form or connect to a payment API.

#### Compatibility checklist

Show the compatibility checklist from the text file so riders can check whether their pack or connector is `SL-48`, `SL-60`, `GB-AC-48`, or `GB-AC-60` before they visit a station.

#### Monthly plan note (optional)

You may add the monthly-plan note from the text file (plans may come later; this site publishes pay-as-you-go only). Do not invent extra named consumer plans or different prices.

#### Rider app link

Add the rider-app button or link using the exact link text from the text file, so visitors know where to register, reserve, and view receipts.

### For fleets

#### Intro

Open with the For fleets intro from the text file. This page is for delivery companies that want Partner plans, quotas, overage pricing, and optional priority access at busy times.

#### Partner plans

Present **Partner starter** and **Partner fleet** clearly (two columns, two blocks, or a comparison table). Use the plan details from the text file exactly:

| Plan                | Monthly base price | Included uses | Extra use (overage)        |
| ------------------- | ------------------ | ------------- | -------------------------- |
| **Partner starter** | ¥2,000             | 150 / month   | ¥6 per use above the limit |
| **Partner fleet**   | ¥5,000             | 400 / month   | ¥5 per use above the limit |

#### Volume discounts

Show the volume discounts on **extra uses only** (the monthly base price stays the same). Use the numbers from the text file in a small table or simple graphic — not a long list of repeated cards:

- Partner fleet: 0–499 → 0%; 500–999 → 10%; 1000+ → 20%
- Partner starter: 0–199 → 0%; 200–399 → 5%; 400+ → 15%

#### Priority time windows

Explain priority time windows with the text from the text file, including the example hours **11:00–14:00** and **17:00–20:00**. Make clear that a share of capacity is offered first to that partner’s riders, and that priority never skips compatibility or safety checks. You may also show [`assets/images/priority-windows.svg`](./assets/images/priority-windows.svg).

#### Funding

Include the funding sentence from the text file: a district safety grant helps start stations, and delivery partners also help fund the network. This is not a tax and not a score for people or businesses.

#### Example partners (optional)

You may list the fictional partner names from the text file as examples only. Do not use real company logos or real brand names.

#### Contact

Provide the static contact action from the text file (for example a `mailto:` link to `partners@swaploop.example`, or a contact form that does not send data). No JavaScript submit and no backend.

### Harder CSS and animation

#### Sticky site header

The site header stays fixed (sticky) at the top of the viewport while the page scrolls, on all pages, at all three breakpoints.

#### CSS-only station type tabs

On the Stations page, the station types section uses CSS-only tabs as described under [Station types](#station-types).

#### How it works timeline

On the How it works page, the three steps use the responsive timeline layout described under [Three steps](#three-steps).

#### Overlapping station images

On the Stations page, featured stations use the desktop overlapping image layout described under [Featured stations — desktop image layout](#featured-stations--desktop-image-layout).

#### Sticky prices on For riders

On the For riders page, the pay-as-you-go prices use the desktop sticky behaviour described under [Pay-as-you-go prices](#pay-as-you-go-prices).

#### Transitions and animations

- Main navigation links and main buttons/links must have a visible hover transition and a visible focus transition.
- The Home hero must play a soft fade-in (or fade-up) entrance animation when the page loads.
- When `prefers-reduced-motion: reduce` is set, turn off or simplify the hero entrance animation and any other non-essential motion. Hover/focus feedback may remain.

### Accessibility

- Correct heading order on each page; one `h1` that matches the page topic (use the page title / headline from the text file).
- Use the provided `alt` text from the text file for informative images; empty `alt` (or CSS background) for decorative images.
- Clear **focus** styles for links and controls; do not remove focus outlines unless you replace them with something equally clear.
- Colour must not be the only way to show station type or plan difference (also use text and/or icons).
- Enough contrast for text and controls (aim for WCAG AA).

### SEO

- Use the `<title>` and meta description values from the text file on each page.
- Use `header`, `nav`, `main`, and `footer`, and use the navigation / link labels from the text file.
- On Home at least, add basic Open Graph tags using the Home title and meta description from the text file, and [`assets/images/og-default.jpg`](./assets/images/og-default.jpg) (`og:title`, `og:description`, `og:image`).
- Use [`assets/images/favicon.svg`](./assets/images/favicon.svg) as the favicon.

## Assessment

Experts mark the work by hand against the marking scheme, using a current desktop browser (Chrome or Firefox) and a narrow mobile window. They will check:

- required pages and correct use of the provided text (terms, prices, three stations)
- responsive layout at mobile (below `768px`), tablet (`768px`–`1023px`), and desktop (`1024px`+)
- accessibility and SEO items listed above
- harder CSS techniques and animation / `prefers-reduced-motion`
- overall visual quality and clear branding (no need to match a fixed design pixel by pixel)

HTML/CSS validators may be used as extra evidence. Judgement marks follow the marking scheme.

## Mark distribution

| WSOS SECTION | Description | Points |
| ------------ | ----------- | ------ |
| 1 | Work organization and self-management | 1.75 |
| 2 | Communication and interpersonal skills | 1 |
| 3 | Design Implementation | 7.75 |
| 4 | Front-End Development | 6.5 |
| **Total** | | **17** |

Section 3 covers layout, visual design, and content presentation. Section 4 covers responsive behaviour, required CSS techniques, motion, and structural front-end requirements. Exact aspects are in [`marking/marking-scheme.json`](./marking/marking-scheme.json).
