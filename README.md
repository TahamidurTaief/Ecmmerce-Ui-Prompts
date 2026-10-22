# GhorerBazar — Reusable Visual Implementation Specification

Reference: https://ghorerbazar.com/  
Audit date: 22 October 2026 (Asia/Dhaka)  
Purpose: give this entire Markdown to a coding agent to replace an existing storefront's visual system with the reference's layout, colors, spacing, components and motion, while retaining the project's own content and working business logic.

## 1. Instructions to the implementing agent

Implement this specification in the existing project. First inspect its framework, shared components, CSS cascade, routes and data contracts; then make the changes. Treat this document as the storefront visual authority. Replace conflicting old card/header/footer styles rather than adding a partial cosmetic layer. Reuse the current stack; do not rebuild the application or replace its backend.

Keep the target project's own brand/logo, products, descriptions, prices, currency rules, inventory, images, reviews, navigation destinations and contact details. Reproduce the reference's visual format using those data. All repeating content must remain dynamic. Keep cart, variants, checkout, authentication, wishlist, search, payments, analytics, permissions and SEO functional. Preserve SSR, canonical URLs, metadata, structured data, sitemap and robots behavior. Do not copy the reference's tracking IDs, phone numbers, merchant identity, customer testimonials or third-party chat account.

Apply this design consistently wherever the mapped component occurs: homepage, search results, collections, recommendations, related products, wishlist and other storefront grids. Admin styling is outside this storefront migration unless explicitly requested.

Mandatory project-specific footer credit on every public page: **Developed by TaiefLab**, with TaiefLab linked to **https://taieflab.com**. If the target project provides `taieflablogo.png`, use it beside “Developed by”, with accessible alt text and the same destination. This credit is a user-requested addition, not a claim about the reference footer.

## 2. Evidence and accuracy boundaries

- **Measured:** live desktop DOM/computed styles at a 1363 × 936 CSS-pixel viewport; content area excluding scrollbar was 1348px wide. Homepage, Honey collection and Sundarban Honey product detail were inspected. Homepage footer and sticky category navigation were visually checked after scrolling; the empty right-side cart drawer was also observed.
- **Source verified:** component stylesheets, homepage inline overrides, responsive media queries, banner intrinsic dimensions and slider configuration.
- **Not visually verified:** mobile viewport rendering, every hover/focus state, populated-cart states, checkout, signed-in account pages, all informational pages and every possible campaign popup. Responsive values below come from source rules, not a mobile screenshot. Do not describe these as end-to-end audited.
- Values described as an implementation choice are deliberate adaptations, not measured reference facts. Exact final parity requires matching viewport, content lengths, font files and image assets and completing the QA matrix below.
- CSS declarations can conflict. The final values in sections 3–16 take precedence over default declarations in the source appendix. Never use an earlier default when a measured override is specified.

## 3. Final color tokens

| Role | Final value | Application |
| --- | --- | --- |
| Primary | `#F48721` | CTA, prices, icon accents, pagination, badges |
| Secondary | `#041F1E` | Category navbar, secondary CTA/hover |
| Main homepage canvas | `#FBF9F5` | Warm near-white section background |
| Surface | `#FFFFFF` | Header, cards, footer, drawers |
| Search surface | `#F5F5F5` | Desktop search field |
| Title | `#222831` | Headings, labels |
| Paragraph | `#252A34` | Main paragraph token |
| Body | `#666666` | Secondary text and footer copy |
| Border | `#CCCCCC` | Standard cards, footer separator |
| Fine separator | `#EEEEEE` | Header/menu/section separators |
| Old price | `#AAAAAA` | Struck-through regular price |
| Success | `#34BE82` | Save percentage badges |
| Alert | `#FF1818` | Stock-out treatment |
| Top-selling badge | `#FF3F33` | Best Selling corner flag |
| Savings pill | `#BFDB38` | Top-selling monetary saving |
| Pale CTA fill | `#FFF2EA` | Top-selling secondary CTA override |
| Collection canvas | `#F2F4F8` | Collection `.main-menu` surface |

Do not substitute a generic green grocery theme. The defining combination is warm off-white + orange + very dark green + white.

```css
.storefront-theme {
  --gb-primary: #f48721;
  --gb-secondary: #041f1e;
  --gb-canvas: #fbf9f5;
  --gb-surface: #fff;
  --gb-title: #222831;
  --gb-paragraph: #252a34;
  --gb-body: #666;
  --gb-border: #ccc;
  --gb-success: #34be82;
  --gb-danger: #ff1818;
  color: var(--gb-body);
  font-family: 'Open Sans', sans-serif;
  font-size: 14px;
  line-height: 1.6;
}
.storefront-theme *, .storefront-theme *::before,
.storefront-theme *::after { box-sizing: border-box; }
```

Scope tokens to the storefront. Adapt selectors to the target stack. Do not globally set `html { font-size:10px }` in an existing project: the reference uses a 10px rem base, so convert its rem units to pixels or local tokens before transplanting CSS. Otherwise a 1.2rem reference gap will incorrectly become 19.2px in a 16px-root application.

## 4. Typography and layout foundation

| Element | Desktop | Source mobile rule |
| --- | --- | --- |
| Body | Open Sans, 14px/22.4px, 400 | Preserve readable body size |
| Ordinary section heading | 20px/24px, 700 | 16px, 600 at ≤767px |
| Top Selling heading | 28px/33.6px, 600 | 20px at ≤767px |
| Featured Categories heading | 22px, 500 | 18px at ≤767px |
| Ordinary card title | Open Sans, 16px/19.2px, 500 | 14px; fixed 34px title slot |
| Ordinary card price | 16px/25.6px, 600 | Verify inherited final value |
| Section action link | 12px/14.4px, 500, uppercase, 0.7px tracking | 12px |
| Footer heading | 16px/20.8px, 500 | Same source base |
| Footer links | 14px, 400 | Same source base |

The ordinary card wrapper/price declares `Poppins, sans-serif`, whereas headings/buttons resolve to Open Sans. Only Open Sans was explicitly linked as a Google font on the inspected homepage; a declared family alone does not prove Poppins was downloaded. For strict reproduction verify actual font availability before adding a new Poppins import; do not silently alter the price glyph metrics.

Desktop container measured `max-width:95%`, centered with 20px inner padding. At 1348px content width: outer container 1280.59px, left margin 33.70px, usable width 1240.59px, content left edge 53.70px. Use fluid sizing rather than hardcoding these coordinates. Main sections use **16px vertical padding**, not large 64–100px landing-page spacing. Reference grid rows use -10px outer horizontal margins and 10px column padding (20px gutters). Small-screen container must be checked against current source/visual QA rather than assuming desktop 95% at every breakpoint.

Spacing vocabulary: 4, 6, 8, 10, 12, 16, 20, 24, 28, 32, 40px. Radius vocabulary: 4px ordinary controls/cards, 6px top-selling buttons, 8px hero/brand/top-selling, 12px PDP panel, 16px testimonials, 20px category tiles, circles for social/arrow/avatar. No universal pill radius, heavy shadows, glass navbar or invented gradients.

## 5. Homepage section order

1. White main header with logo, search and utility actions.
2. Full-width dark-green category navigation.
3. Two-column promotional hero: rotating large banner + smaller static banner.
4. Featured Categories horizontal slider.
5. Top Selling Products: four special cards in two columns/two rows at desktop.
6. Our Brands slider.
7. All Natural Honey product slider.
8. Premium Dates product slider.
9. Wide promotional banner slot.
10. Cooking Essentials product slider.
11. Organic Certified product slider.
12. Just For You product grid + Load More.
13. Testimonials slider.
14. White multi-column footer + copyright/payment strip.
15. Floating cart, chat/contact and back-to-top affordances; mobile bottom navigation.

Map these section types onto target content. Category names are sample reference content, not mandatory grocery labels for unrelated stores. If a target has no genuine reviews/brands, do not fabricate them. Retain the section schema for real data.

## 6. Header and navigation

### Desktop main row

White surface; 16px top/bottom padding and 1px #EEE bottom border. Measured main row height about 87px including border. Logo image max width 170px and max height 48px, intrinsic aspect preserved (`object-fit:contain`); measured reference logo about 119.56 × 48px. Use the target logo with the same bounding constraints.

Search maximum width 446px, measured height 47px, fill #F5F5F5, radius 8px. Input 14px Open Sans; padding `9px 42px 9px 16px`; transparent 1px border at rest; primary border on focus. Search icon 20px, absolutely positioned right 14px/top 12px, transparent button. Utility actions: Track Order, Sign In, Wishlist, Cart, More. Outline icons 22px with 14px text underneath, 6px label offset. Cart count: 16px circle, 10px text, orange fill. More menu: absolute right-aligned white panel, min-width 220px, border #EEE, 14px menu text, rows `10px 12px`, icon gap 12px, divider #EEE. Hover accent orange; implement equivalent keyboard and touch toggles.

Search result dropdown: white, #E0E0E0 border, lower radius 8px, max-height 400px, vertical scroll, subtle `0 4px 6px rgba(0,0,0,.1)` shadow. Result row padding 12px, thumbnail 50px square/radius 6px, 12px text gap, 14px title clamped to two lines, orange 13px/600 price. Target search API remains authoritative.

### Category row

Background **#041F1E**, about 50.39px high at measured desktop. White 14px/400 labels; horizontal compact navigation; dropdown chevrons. Final inline override sets active/hover text **orange**, overriding the component stylesheet's earlier white hover declaration. No underline ornament on top-level links. Sticky row remains at viewport top after scrolling; the full white utility row scrolls away in the observed desktop state. Preserve the same sticky behavior without layout jump.

### Mobile rules

Reference CSS has layout changes at 1199.99px, 991px and 767px. Desktop main navigation/header controls also depend on shared CSS: inspect those rules during implementation rather than using unused `.h-*` template selectors as proof of rendered layout. At ≤767px logo is centered by the active logo rule. Mobile search uses a top slide-in search modal: starts at `top:-100%`, active `top:0`, 300ms; white surface, dark 50% backdrop, 12px search box padding, field radius 5px, orange submit button. Include close, Escape, backdrop dismissal, focus management and keyboard access as implementation requirements.

## 7. Hero and banner dimensions

Desktop Bootstrap-like split at large breakpoint: `col-lg-8` + `col-lg-4`; 20px gutters. Top offset 20px. Measured main slide 820 × 328px; side banner about 400.19 × 328.14px. Banner corner radius 8px. Main desktop artwork intrinsic dimensions: **1000 × 400** and one **1001 × 401**, approximately **2.5:1**. Side artwork: **500 × 410**, approximately **1.2195:1**. Main mobile assets include **501 × 281** and **500 × 279**, approximately **1.79:1**, but some slides reuse 1000 × 400 assets. Do not falsely claim all mobile banners share one ratio.

Use a per-banner desktop/mobile source (`picture` or equivalent). Width 100%, height auto, preserve each artwork's ratio; avoid stretching/cropping embedded text. Target admin should accept separate desktop and mobile images, destination, alt text, active state and ordering. Desktop hero split changes below 992px. Side-wrapper source margin-top becomes 16px there; verify visibility when no mobile side image is configured.

Hero slider: 1 slide visible, 20px slide spacing, loop enabled, autoplay delay **8000ms**, interaction does not permanently disable autoplay. Pagination bottom 16px/left 24px; mobile bottom 8px. Arrow controls 40px square on desktop/32px below 992px, hidden until hero hover, white/orange default and orange/white hover. Sliding wrapper transform was observed; use approximately 300ms transition where the slider default is retained. Do not add parallax/text entrance effects unless actually present in target assets.

Wide middle promo: initially measured zero-height because image was lazy/unloaded; later visually appeared after scrolling. Never use that zero as the intended banner height. Loaded display was a wide rounded campaign graphic. Intrinsic dimensions of this slot were not confirmed; retain asset aspect ratio and verify after loading. This is explicitly unresolved rather than an invented banner size.

## 8. Featured Categories

Centered heading, desktop bottom spacing 32px; mobile 16px. Image tile 142 × 142px desktop, **100 × 100px** at ≤767px, radius 20px, clipped overflow and `object-fit:cover`. Label below: 16px/500/1.2, 10px top margin, 2px bottom padding, single-line clamp, centered. Hover image scale **1.04** over **300ms**; label becomes orange.

32px circular orange previous/next buttons, white 18px arrow; positions -18px outside left/right slider edges; hover dark title color. Slider overflow hidden with 2px inner horizontal padding. Slider loop false; autoplay 3000ms, resumes after interaction and pauses on mouse enter. Breakpoints: ≥300px 3 visible/gap16; ≥768px 6/gap16; ≥1024px 6/gap16; ≥1320px 8/gap20. Keep fixed tile sizes and inspect any slide/tile width mismatch at intermediate sizes.

## 9. Ordinary product card — central replacement component

Replace the target's current reusable card everywhere with this component structure:

```text
ProductCard
  MediaLink (target product URL)
    PrimaryImage
    OptionalSecondaryImage
    FlagBadge (top left, optional)
    SavingBadge (top right, optional)
  ContentGroup
    ProductDetails
      ProductNameLink
      PriceRow (sale price + optional struck old price)
    ProductAction
      AddToCart OR QuantityStepper OR DisabledStockState
```

| Property | Desktop | ≤767px |
| --- | --- | --- |
| Card surface | white | white |
| Border/radius | 1px #CCC / 4px | same |
| Padding | 8px | same |
| Layout | flex column, space-between, full available height | same |
| Shadow | none at rest | none |
| Image | width 100%, square reference product assets, clipped media | same responsive width |
| Detail top padding | 12px | same source base |
| Title | 16px/500, 19.2px line-height, bottom margin 8px | 14px, fixed/min-height 34px |
| Price | orange 16px/600 | source inheritance; verify |
| Old price | #AAA, 16px/400, strike | same base |
| Price spacing | current-price right margin 10px | same base |
| Action top margin | 12px | 8px |
| CTA | full width, 40px height | 34px |
| CTA typography | 14px/600/1.2, Open Sans | 12px |
| CTA styling | 1px orange border, radius4, transparent fill/orange text | same |
| Hover CTA | orange fill/white text, 400ms | touch-safe equivalent |

Product image hover scale 1.04, 300ms ease. Optional secondary image absolute inset0/opacity0, fades to opacity1 on media hover. If only one image exists, do not apply a `last-child` rule that accidentally hides it; explicitly scope alternate images. No card lift, glow or large hover shadow was established.

Flag badge: top6/left6, orange fill, white 10px text, padding2px6px, radius4. Save badge: top6/right6, success-green fill, same typography/padding/radius. Both attach to the media's local coordinate space. Derive savings from actual target prices; hide when absent. Do not show fake discounts.

Quantity state occupies the same CTA footprint: absolute stepper top0/left0, full width, orange 1px border, radius4; minus/plus orange squares 40px desktop, 34px mobile; white 16px symbols; centered quantity 14px/700 with 0.7px tracking. Hidden state uses opacity0, visibility hidden and pointer-events none; transition 400ms; stepper buttons transition 200ms. Preserve target inventory and server validation, prevent duplicate requests and restore state on failure. Zero quantity returns to Add To Cart if target cart supports removal. Stock-out button red border/text, disabled interaction and no hover fill. Wishlist/product-icon controls are hidden by card source rules on mobile.

Grid/cell height depends on text and images; measured slider card about 232.19px wide ×347.22px high at desktop. Do not hardcode the height. Maintain aligned CTA baselines in each row without cutting names/prices. Long titles, missing images and undiscounted products must work.

## 10. Top Selling card — separate variant

Desktop 2-column grid, 24px gap, four reference entries. Each card measured about 608.30 ×300px; white surface (final override), radius8, flex row, 24px gap. Image300×300px with radius8; hover scale1.06 over400ms. Content padding12px12px12px0. Name20px/600/1.3, max2 lines, margin-bottom8. Sale price18px/700/1.5 orange, regular18px/500 struck/hints color. Saving pill #BFDB38, 12px/600, padding4px8px, radius99. Optional corner flag top-right #FF3F33, padding4px8px, radius `2px 8px`.

Actions margin-top30px/gap8px. Add/Buy button height40, padding12px16px, 14px/600, radius6. Filled primary Buy Now turns dark green on hover; outlined Add To Cart fills orange on hover. Source `.theme-btn.secondary` has a pale #FFF2EA override; final component mapping must follow which actual target action receives that variant.

768–1199px: one column. ≤767px: grid returns to base two columns, gap12; card wraps to vertical, gap0, image full width, content padding12. Title14px, prices14px, saving10px/padding2px4px. Action margin-top12; buttons30px high, 12px type. Match this unusual breakpoint behavior rather than assuming monotonic columns.

## 11. Section heads, product rails and recommendations

Ordinary section head: flex between, align center, border-bottom1px#EEE, padding-bottom12, margin-bottom20. Accent pseudo-element width40/height4/radius30, bottom-2px, orange. Heading20px/700; action12px/500 uppercase/0.7px tracking, underlined text, arrow17px, gap8. Mobile heading16px/600 and arrow14px. Top Selling uses the centered no-border/no-accent style6 variant (28px desktop,20px mobile), bottom spacing32px/16px.

Product rails: 20px inter-slide gap; wrapper vertical padding12px; 2 visible at ≥300 and ≥576, 3 at ≥768, 5 at ≥992. Honey autoplay3500ms; Dates3000ms; Cooking4500ms; Organic4000ms. All explicitly keep autoplay after interaction. Do not confuse delay with slide animation duration. Use isolated pagination/navigation per instance so controls do not manipulate another rail. Prevent clone duplicate IDs and duplicated cart requests.

Just For You: static responsive grid, source classes specify 2 columns base,3 at md,4 at lg,5 at xl (usual underlying framework thresholds768/992/1200; check the actual target adapter). Reference initially shows ten products and a Load More control. Keep pagination/data-fetch contract and style newly appended cards identically. No infinite-scroll substitution unless requested.

## 12. Brands and testimonials

Brand cards: white,1px#EEE border,radius8,vertical margin2px. Logo max124px wide/height88/object-fit contain; mobile max100px/height80. Source slider ≥300:2/gap16; ≥768:3/gap16; ≥1024:4/gap16; ≥1320:4/gap24. Autoplay3000ms, loopfalse, pause mouseenter, resume after interaction; clickable dots.

Testimonials: border1px#CCC,radius16,padding24,min-height224,margin-top1px. Text14px,clamp4 lines. Five-star rating row. Author flex/gap8,avatar60×60/circle/cover, name14px/500/1.2,role12px/400/margin-top4. Source slider ≥300:1/gap16; ≥768:3/gap16; ≥1024:3/gap16; ≥1320:3/gap20. Autoplay3000ms,loopfalse,pause mouseenter,clickable dots. Use real target testimonials only.

## 13. Footer

Final background **white**, not stylesheet's default #F5F5F5. Desktop about column occupies4/12; remaining8/12 contains four link groups: Information, Shop By, Support, Consumer Policy. Use target links in these structural roles. Main top area padding32px0 72px. Widgets margin-top40px. Footer logo max170px wide/48px high/contain. About text14px/1.5,margin-top20. Contacts margin-top28;16px/1.5;row bottom7px;icon right8px. Social margin-top28;40px circular #FBF9F5 buttons,orange16px icons,8px horizontal gap; orange fill/white on hover300ms. App-download group margin-top32, links gap12/margin-top20; show only real app destinations.

Widget headings16px/500/1.3,margin-bottom16; links14px/400/1.2, row margin-bottom8, gray → orange hover. Bottom strip border-top1px#CCC,padding16px0,flex between;copyright14px/1.5;payment image max-width50% desktop,100% tablet/mobile. Keep actual supported payment brands only.

768–1199.99px bottom strip stacks column-reverse,centered,payment margin-bottom12. ≤767px top padding16px0 56px;bottom column-reverse/centered,padding-bottom80 to clear dock;copyright margin-top8. Verify target footer columns at actual mobile widths rather than guessing their grid classes.

**Always include “Developed by TaiefLab” linking to https://taieflab.com**, in the footer bottom area, readable at14px or the existing footer body scale. Place below/alongside copyright without displacing the payment strip; wrap gracefully on mobile. With official logo, constrain width automatically and height20–24px (implementation choice), preserve aspect ratio. Do not use white credit text on the white footer.

## 14. Mobile bottom navigation and floating UI

Source bottom dock visible below768px only. Fixed bottom0/left0,width100%,orange background,white icons/uppercase labels,equal-flex items. Source markup contains Home, Menu, Cart and Search; inspect remaining target adapter destinations rather than inventing a fifth item. Icon16px,label10px,margin-top8,tracking0.025em,line-height1. Default padding10px40px; ≤479px later override9px10px; safe-area bottom `max(10px,env(safe-area-inset-bottom))`. Z-index in reference1010101—replace excessive raw values with an intentional target stacking scale while preserving overlay order. Dock should never cover product actions, footer credit or keyboard-focused fields.

Desktop right floating cart: compact orange count/icon block with white price strip, fixed on right around vertical middle; opens a right-side white drawer. Observed empty drawer about400px wide at desktop with a dimmed backdrop, Shopping Cart heading/Close and centered empty illustration/message. Exact drawer padding/motion timing not measured: inspect target implementation and confirm visually; don't invent precise values as reference facts. Reuse target cart drawer state and support Escape/focus restore/body scroll lock.

Back-to-top is orange circular outlined treatment near bottom-right. Chat/contact launcher is orange near lower-right, with contact choices; use target integrations. Avoid copying source account IDs or importing third-party scripts just to reproduce the launcher.

## 15. Collection page extension

Inspected Honey collection: category title + breadcrumb row,12px vertical padding, about52.8px high. Pale #F2F4F8 collection canvas. Desktop left sidebar about310px outer/290px panel width, with right results area about930.6px. Source grid gutter20px. Panels white,radius4,padding12,margin-bottom6. Filter groups: category checkboxes, price-range slider, brands and product flag. Heading uppercase with orange underline/divider. Toolbar white,radius4,padding12,height62px in desktop measurement; sort selector left,page-size selector right. Four ordinary product cards per desktop row in observed results (do not use homepage's five-card rail count here). Pagination and result count below. Keep real filtering/query parameters and accessible range controls. Exact responsive sidebar/drawer interaction is pending mobile visual verification.

## 16. Product detail extension

Inspected product page includes breadcrumb; white product panel; gallery with vertical thumbnails; title, price/old price/save badge; quantity; two-column Add to Cart/Buy Now then WhatsApp/Call; brand; Description/Customer Reviews tabs; review form; related-product grid/rail; shared footer.

Measured desktop product panel radius12,padding24,white,about1260.59px outer width. Gallery and details each about586.30px; details padding2px0 0 10px. Main title24px/500/1.2,margin-bottom16. Main price26px/600/1.2 orange,margin-bottom4. Thumbnail selection has orange border; image area light border and white background. Add orange,Buy dark green,WhatsApp green,Call blue as observed. Exact green/blue button hex values and all mobile/PDP styles were not extracted; verify those before asserting exact parity. Product images and thumbnails must follow target data; gallery controls must work with one or many images. Related products reuse the ordinary card. Keep functional tabs/reviews, accessible forms and target validation.

## 17. Motion inventory

| Component | Trigger | Verified behavior |
| --- | --- | --- |
| Hero | timer/swipe/dots | horizontal slide;8000ms delay;loop |
| Categories | timer/arrows/swipe | 3000ms delay;no loop;pause hover |
| Category image | hover | scale1.04/300ms |
| Ordinary image | hover | scale1.04/300ms;optional alternate-image opacity |
| Top-selling image | hover | scale1.06/400ms |
| Card CTA/stepper | hover/state | 400ms color/opacity/visibility |
| Quantity buttons | hover | 200ms color |
| Brands/testimonials | timer/swipe/dots | 3000ms delay;no loop;pause hover |
| Product rails | timer | individual3000–4500ms delay;300ms wrapper transition observed |
| Search modal | open/close | top position/300ms |
| Footer social | hover | background/text300ms |
| Sticky category row | scroll | fixed top state observed |
| Cart bounce | source function exists | final `.cart-bounce {animation:none}` disables it |

Do not introduce a cart bounce merely because a JS function adds its class. Do not apply animate.css entrance animations globally just because that dependency exists. Implement `prefers-reduced-motion`: disable autoplay and transforms when requested (accessibility adaptation, not verified source behavior). Pause autoplay while keyboard focus is inside an interactive slider. Avoid animating layout dimensions unnecessarily.

## 18. Integration procedure and acceptance gates

1. Inventory old components/tokens and identify each mapping. Create one theme entry point and reusable primitives for container, section head, cards, controls and badges.
2. Implement exact tokens/layout/header/footer; preserve target data binding.
3. Implement ordinary card then top-selling variant, rails, category tiles, brands and testimonials. Wire target cart and async state.
4. Adapt collection and PDP. Apply theme consistently to target search/wishlist/cart/checkout forms using the same tokens; those uninspected page layouts must not be described as exact source copies.
5. Remove or scope conflicting old CSS, check computed values after all styles load, prevent hydration differences. Avoid copying global `.container`, `.product`, `.title`, `html` rules unscoped into unrelated areas.
6. Verify at360,390,430,768,1024,1363 and1440px widths. Mobile rendered parity remains a required gate because this audit used source rules there.
7. Screenshot compare at the same viewport/content conditions, with banners loaded, autoplay paused at same slide and matching card counts. Check top, each rail, footer, hover, drawer and mobile dock. Report any unresolved measurements explicitly.
8. Run appropriate existing lint/build/tests, then validate card links, search, filters, add/remove/update cart, stock/variants, checkout handoff, tabs, load-more, sticky navigation, keyboard and responsive overflow. Do not place real orders or change production infrastructure for a visual migration.

Acceptance checklist:

- [ ] Main warm canvas, orange accents,dark-green navbar and white footer match final tokens.
- [ ] Header/search/logo bounds and navbar sticky state match.
- [ ] Banner desktop/mobile source selection and aspect ratios correct; no text cropping.
- [ ] Card padding8,border1,radius4 and mobile CTA34px correct.
- [ ] Both ordinary and top-selling card variants implemented.
- [ ] Section spacing16px and heading accent rules correct.
- [ ] Per-slider breakpoints/gaps/timers isolated and correct.
- [ ] No heavy shadows,unrequested pill cards or invented motion.
- [ ] Mobile dock safe-area/keyboard/footer clearance verified.
- [ ] Real target data and all storefront functions preserved.
- [ ] Every footer displays Developed by TaiefLab → https://taieflab.com.
- [ ] SSR/SEO preserved; no horizontal overflow or layout shift from image loading.
- [ ] Final report says what changed,how to test,and remaining parity gaps.

## 19. Copyable task instruction

> Apply this entire specification to the current project's public storefront. Inspect the repository first, map existing components to the reference component types, then implement the final measured design tokens, responsive rules, layouts and motion. Replace conflicting old visual styles consistently while preserving existing content, APIs, routes, business rules, SSR and SEO. Use the project's own branding and assets. Implement both ordinary and top-selling card variants and shared header/footer. Every public footer must contain Developed by TaiefLab linked to https://taieflab.com. Treat the source appendix as supporting evidence, apply final overrides, convert the reference's10px-root rem units safely, and do not claim unverified mobile/checkout states are exact. Complete responsive and functional QA and report what changed,how to test,and unresolved differences.

## 20. Source reference index

- Homepage: https://ghorerbazar.com/
- Collection inspected: https://ghorerbazar.com/collections/honey-2
- Product inspected: https://ghorerbazar.com/products/sundarban-honey
- Base CSS: https://ghorerbazar.com/assets/css/style.css
- Shared demo CSS: https://ghorerbazar.com/assets/css/demo.css
- Header: https://ghorerbazar.com/assets/css/home-page/header/header-style-7.css
- Footer: https://ghorerbazar.com/assets/css/home-page/footer/footer-style-3.css
- Ordinary card: https://ghorerbazar.com/assets/css/home-page/product-card/card-style-6.css
- Top selling: https://ghorerbazar.com/assets/css/home-page/top-selling-product/top-selling-product.css

## 21. Component CSS source appendix

These are retrieved component declarations for precise lookup. They include inactive template selectors and defaults overridden by the homepage. They are evidence, not a drop-in stylesheet. Port relevant declarations into scoped target components; final specifications above win. Dependencies on base CSS, custom properties and the10px rem base must be resolved. Mobile rules below are source verified only.

### header-style-7.css

```css
/*=============================================================================================
  Start Header Area
==============================================================================================*/
.header.style-7 {
    position: relative;
    width: 100%;
}

.header.style-7 ul.live_search_box{
  top: 48px;
}

.header.style-7 .header-right i {
    font-size: 20px;
    line-height: 0;
}
.header.style-7 .header-right i,
.header.style-7 .cart-dropdown > a i {
    font-size: 22px;
    transition: all 0.3s ease;
    font-weight: 500;
    color: var(--title-color);
}
.header.style-7 .header-right a:hover i {
    color: var(--primary-color);
}
.header.style-7 .header-action-label {
    font-size: 14px;
    color: var(--title-color);
    font-weight: 400;
    margin-top: 6px;
    display: block;
    transition: all 0.3s ease;
}

.header.style-7 .menu > li > a {
    font-weight: 400;
    color: var(--white-color);
}
.header.style-7 .menu > li.active > a:not(.menu-title),
.header.style-7 .menu > li:hover > a:not(.menu-title) {
    color: var(--white-color) !important;
}
.header.style-7 .menu .submenu {
    padding: 1rem 0;
}

.header.style-7 .menu > li .megamenu,
.header.style-7 .menu > li > .submenu {
    left: 0;
}

.header.style-7 .active-underline > li > a::before {
    display: none;
}

.header.style-7 .header-right a:hover .header-action-label {
    color: var(--primary-color);
}

.header.style-7 .header-menu-btn {
    position: relative;
    cursor: pointer;
    padding-bottom: 12px;
    top: 6px;
    z-index: 3;
}
.header.style-7 .header-menu-btn-text:hover {
    color: var(--primary-color);
}
.header.style-7 .header-menu-btn .submenu {
    display: none;
    position: absolute;
    top: 100%;
    right: 0;
    background-color: #fff;
    padding: 0;
    min-width: 220px;
    z-index: 999;
    margin: 0;
    border: 1px solid #eee;
}
.header.style-7 .header-menu-btn:hover .submenu {
    display: block;
}
.header.style-7 .header-menu-btn .submenu li {
    list-style: none;
    border-bottom: 1px solid #eee;
}
.header.style-7 .header-menu-btn .submenu li:last-child {
    border-bottom: 0;
}
.header.style-7 .header-menu-btn .submenu li a {
    text-decoration: none;
    color: var(--title-color);
    display: flex;
    font-size: 14px;
    line-height: 120%;
    padding: 10px 12px;
    transition: all 0.3s ease;
    align-items: center;
    gap: 12px;
}
.header.style-7 .header-menu-btn .submenu li a:hover {
    color: var(--primary-color);
}
.header.style-7 .header-menu-btn .submenu li img {
    width: 22px;
    height: 22px;
    object-fit: contain;
}
.header.style-7 .cart-dropdown .cart-count {
    width: 16px;
    height: 16px;
    right: -6px;
    font-size: 10px;
    line-height: 17px;
}
.header.style-7 .header-search.hs-expanded {
    max-width: 44.6rem;
    background: #f5f5f5;
    border-radius: 8px;
}
.header.style-7 .header-search input.form-control {
    border: none;
    padding-right: 42px;
    font-size: 14px;
    border-radius: 8px;
    border: 1px solid transparent;
    color: var(--title-color);
}

.header.style-7 .header-search input.form-control:focus {
    border: 1px solid var(--primary-color);
}

.header.style-7 .header-search.hs-round .btn-search {
    background: transparent !important;
    color: var(--paragraph-color);
    position: absolute;
    right: 14px;
    top: 12px;
    width: initial;
    min-width: initial;
}
.header.style-7 .header-search .btn-search i {
    font-size: 20px;
}
.header.style-7 .header-search.hs-round .btn-search:hover {
    color: var(--primary-color);
}

.header.style-7 .header-middle {
    padding: 16px 0px;
    border-bottom: 1px solid #eee;
}
.header.style-7 .header-bottom {
    padding: 0;
    background: var(--primary-color);
}
.header.style-7 .inner-wrap {
    justify-content: space-between;
}

.header.style-7 .header-top-wrapper {
    background: var(--primary-color);
    padding: 16px 0px;
}

.header.style-7 .header-inner {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 80px;
    width: 100%;
}
.header.style-7 .header-wrapper {
    display: flex;
    align-items: center;
    gap: 28px;
    width: 100%;
}
.header.style-7 .h-category-btn {
    font-size: 24px;
    color: #e7f6f1;
}
.header.style-7 .h-logo img {
    max-width: 180px;
    height: 48px;
    object-fit: contain;
}
.header.style-7 .h-search-widget {
    display: flex;
    align-items: center;
}
.header.style-7 .h-search-form {
    position: relative;
}
.header.style-7 .h-search-form input {
    font-family: var(--font-family);
    width: 500px;
    height: 48px;
    padding: 15px 120px 15px 15px;
    border-radius: 6px;
    font-size: 14px;
    font-weight: 400;
    line-height: 120%;
    background: #f7f7f7;
    border: none;
}
.header.style-7 .h-search-form button {
    font-family: var(--font-family);
    background: var(--primary-color);
    color: var(--white-color);
    padding: 10px 20px;
    display: flex;
    align-items: center;
    gap: 4px;
    font-weight: 500;
    line-height: 40px;
    position: absolute;
    right: 4px;
    top: 4px;
    height: 40px;
    border-radius: 4px;
    transition: all 0.3s ease;
}
.header.style-7 .h-search-form button:hover {
    background: var(--secondary-color);
    color: var(--white-color);
}
.header.style-7 .h-contact {
    display: flex;
    align-items: center;
    gap: 8px;
}
.header.style-7 .h-contact i {
    font-size: 36px;
    color: var(--white-color);
}
.header.style-7 .h-contact-info span {
    font-size: 14px;
    font-weight: 500;
    line-height: 120%;
    display: block;
    color: #e7f6f1;
}
.header.style-7 .h-contact-info p {
    font-size: 20px;
    font-weight: 600;
    line-height: 130%;
    color: #e7f6f1;
    margin: 0;
    margin-top: 4px;
    transition: all 0.3s ease;
}
.header.style-7 .h-contact:hover .h-contact-info p {
    color: var(--secondary-color);
}
.header.style-7 .header-actions {
    display: flex;
    align-items: center;
    gap: 18px;
}
.header.style-7 .header-actions-group {
    display: flex;
    align-items: center;
    gap: 12px;
}
.header.style-7 .header-actions .btn {
    color: var(--white-color);
    position: relative;
    margin-left: 12px;
}
.header.style-7 .header-actions .btn i {
    font-size: 20px;
    color: var(--white-color);
}
.header.style-7 .header-actions .btn .label {
    position: absolute;
    top: -2px;
    left: -12px;
    width: 20px;
    height: 20px;
    background: var(--white-color);
    color: var(--title-color);
    border-radius: 100%;
    font-size: 13px;
    font-weight: 600;
    line-height: 19px;
    display: inline-block;
    text-align: center;
}
.header.style-7 .h-account-btn {
    background: var(--white-color);
    padding: 8px 12px;
    color: var(--primary-color);
    display: flex;
    align-items: center;
    gap: 4px;
    font-size: 14px;
    font-weight: 500;
    line-height: 120%;
    border-radius: 30px;
    transition: all 0.3s ease;
    white-space: nowrap;
}
.header.style-7 .h-account-btn:hover {
    background: var(--secondary-color);
    color: var(--white-color);
}
.header.style-7 .header-bottom-wrapper {
    background: #f5f5f5;
    padding: 0;
}
.header.style-7 .header-bottom-wrapper.fixed {
    background: var(--white-color);
}
.header.style-7 .header-bottom-inner {
    display: flex;
    align-items: center;
    justify-content: space-between;
}
.header.style-7 .header-item-wrapper {
    display: flex;
    align-items: center;
    gap: 24px;
}
.header.style-7 .h-category-btn {
    background: transparent;
    color: var(--white-color);
    display: flex;
    justify-content: space-between;
    align-items: center;
    gap: 24px;
    border-radius: 4px 4px 0px 0px;
    font-size: 14px;
    font-weight: 700;
    line-height: 120%;
    letter-spacing: 0.7px;
    text-transform: uppercase;
    height: 40px;
    padding: 0;
}
.header.style-7 .h-category-btn i {
    font-size: 20px;
}
.header.style-7 .header-nav ul {
    padding: 0;
    margin: 0;
}
.header.style-7 .header-nav li {
    display: inline-block;
    margin-right: 24px;
}
.header.style-7 .header-nav li:last-child {
    margin: 0;
}
.header.style-7 .header-nav li a {
    font-size: 16px;
    font-weight: 500;
    line-height: 150%;
}
.header.style-7 .header-nav li a:hover {
    color: var(--primary-color);
}
.header.style-7 .header-nav li .submenu li a {
    font-weight: 500;
    font-size: 15px;
}
.header.style-7 .h-flash-btn {
    border-radius: 4px;
    background: var(--white-color);
    padding: 6px 10px;
    display: flex;
    align-items: center;
    gap: 6px;
    font-size: 14px;
    font-style: normal;
    font-weight: 600;
    line-height: 120%;
    letter-spacing: 0.7px;
    text-transform: uppercase;
    color: var(--primary-color) !important;
}
.header.style-7 .h-flash-btn i {
    font-size: 24px;
}

@media only screen and (min-width: 1200px) and (max-width: 1372px) {
    .header.style-7 .header-inner {
        gap: 48px;
    }
    .header.style-7 .h-search-form input {
        width: 400px;
    }
}

@media only screen and (max-width: 1199.99px) {
    .header.style-7 .header-inner {
        display: flex;
    }
    .header.style-7 .header-wrapper {
        gap: 24px;
        justify-content: space-between;
        width: auto;
    }
    .header.style-7 .h-logo img {
        max-width: 164px;
        height: auto;
    }
    .header.style-7 .h-category-btn {
        font-size: 22px;
    }
}

@media only screen and (min-width: 992px) and (max-width: 1199px) {
    .header.style-7 .h-search-form input {
        width: 300px;
    }
}

@media only screen and (max-width: 991px) {
    .header.style-7 .header-bottom-wrapper {
        padding: 8px 0px;
    }
    .header.style-7 .header-nav {
        display: none;
    }
    .header.style-7 .h-logo img {
        max-width: 180px;
        height: auto;
    }
    .header.style-7 .h-flash-btn {
        padding: 8px;
        font-size: 12px;
    }
    .header.style-7 .h-flash-btn i {
        font-size: 18px;
    }
    .header.style-7 .h-search-widget,
    .header.style-7 .h-contact {
        display: none;
    }
    .header.style-7 .h-account-btn {
        background: transparent !important;
        padding: 0;
        color: var(--white-color);
        font-size: 0;
    }

    .header.style-7 .h-account-btn i {
        font-size: 20px;
        position: relative;
        top: 0;
    }
}

@media only screen and (max-width: 767px) {
    .header.style-7 .logo {
        display: flex;
        justify-content: center;
    }
    .header.style-7 .h-logo img {
        max-width: 132px;
        height: auto;
    }
    .header.style-7 .header-middle .header-right > :not(:last-child) {
        margin-right: 6px;
    }
}
/*=============================================================================================
  End Header Area
==============================================================================================*/

```

### hero-style-7.css

```css
/*============================================================================================
   Start Hero css
==============================================================================================*/
.hero.style-7 .intro-slide-wrapper,
.hero.style-7 .intro-banner-wrapper {
    margin-top: 2rem;
}
.hero.style-7 .banner img {
    border-radius: 8px;
}

.hero.style-7 .pg-inner .swiper-pagination {
    bottom: 16px;
    left: 24px;
    transform: none;
    text-align: left;
}

/* Slider Controls  */
.hero.style-7 .swiper-theme .swiper-button-prev {
    left: 0;
}
.hero.style-7 .swiper-theme .swiper-button-next {
    right: 0;
}
.hero.style-7 .swiper-button-next::before,
.hero.style-7 .swiper-button-prev::before {
    display: none;
}
.hero.style-7 .swiper-theme .swiper-button-prev,
.hero.style-7 .swiper-theme .swiper-button-next {
    width: 40px;
    height: 40px;
    border-radius: 0;
    background: var(--white-color);
    color: var(--primary-color);
    opacity: 0;
    visibility: hidden;
}
.hero.style-7 .swiper-theme .swiper-button-prev:hover,
.hero.style-7 .swiper-theme .swiper-button-next:hover {
    background: var(--primary-color);
    color: var(--white-color);
}
.hero.style-7:hover .swiper-theme .swiper-button-prev,
.hero.style-7:hover .swiper-theme .swiper-button-next {
    opacity: 1;
    visibility: visible;
}

@media (max-width: 991px) {
     .d-sm-none {
        display: none;
    }
    .hero.style-7 .intro-banner-wrapper {
        margin-top: 16px;
    }
    .hero.style-7 .intro-banner-wrapper .mb-4 {
        margin: 0 !important;
    }
    .hero.style-7 .swiper-theme .swiper-button-prev,
    .hero.style-7 .swiper-theme .swiper-button-next {
        width: 32px;
        height: 32px;
        font-size: 16px;
    }

    .hero.style-7 .pg-inner .swiper-pagination {
        bottom: 8px;
    }
}

/*============================================================================================
     End Hero css
  ==============================================================================================*/

```

### category-style-3.css

```css
/*============================================================================================
   Start Category Area
=============================================================================================*/

.category-section-head {
    margin-bottom: 32px;
    text-align: center;
}
.category-section-head h3 {
    font-weight: 500;
    font-size: 22px;
    margin-bottom: 12px;
}
.category-section-head p {
    text-align: center;
    margin: 0;
}

.category.style-3 .category-inner {
    position: relative;
}

.category.style-3 .swiper.category-slider {
    overflow: hidden;
    padding: 0px 2px;
}
.category.style-3 .swiper-slide {
    position: relative;
}
.category.style-3 .category-img {
    width: 142px !important;
    height: 142px !important;
    border-radius: 20px;
    margin: auto;
}
.category.style-3 .category-img img {
    width: 142px !important;
    height: 142px !important;
    border-radius: 20px;
    overflow: hidden;
    object-fit: cover;
    transition: all 0.3s ease;
}
.category.style-3 .category-img {
    overflow: hidden;
    transition: all 0.3s ease;
}
.category.style-3 .swiper-slide:hover .category-img img {
    transform: scale(1.04);
}
.category.style-3 .category-info {
}
.category.style-3 .category-info p {
    border-radius: 4px;
    display: -webkit-box;
    -webkit-line-clamp: 1;
    -webkit-box-orient: vertical;
    overflow: hidden;
    font-size: 16px;
    font-style: normal;
    font-weight: 500;
    line-height: 120%;
    margin: 0;
    text-align: center;
    margin-top: 10px;
    color: var(--title-color);
    transition: all 0.3s ease;
    padding-bottom: 2px;
}
.category.style-3 .swiper-slide:hover .category-info p {
    color: var(--primary-color);
}

/* Slider Controls  */
.category.style-3 .swiper-button-next,
.category.style-3 .swiper-button-prev {
    padding: 0;
    z-index: 99;
    border: none;
    background-color: transparent;
    transition: all 0.3s;
    background: var(--primary-color);
    width: 32px;
    height: 32px;
    border-radius: 100%;
    font-size: 18px;
    color: var(--white-color);
}

.category.style-3 .swiper-button-next::before,
.category.style-3 .swiper-button-prev::before {
    display: none;
}

.category.style-3 .swiper-button-next:hover,
.category.style-3 .swiper-button-prev:hover {
    background: var(--title-color);
    color: var(--white-color);
}

.category.style-3 .swiper-button-prev,
.category.style-3 .swiper-rtl .swiper-button-next {
    left: -18px;
    right: auto;
}
.category.style-3 .swiper-button-next,
.category.style-3 .swiper-rtl .swiper-button-prev {
    right: -18px;
    left: auto;
}
@media only screen and (max-width: 767px) {
    .category-section-head {
        margin-bottom: 16px;
    }
    .category-section-head h3 {
        font-size: 18px;
        margin-bottom: 0;
    }
    .category.style-3 .category-img img {
        width: 100px !important;
        height: 100px !important;
    }
    .category.style-3 .category-img {
        width: 100px !important;
        height: 100px !important;
    }
}

/*============================================================================================
   End Category Area
=============================================================================================*/

```

### top-selling-product.css

```css
/*============================================================================================
   Start Top Selling Product
==============================================================================================*/
.top-selling-product .section-head.style-6 {
    margin-bottom: 32px;
}
.tp-product-wrapper {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 24px;
}

.tp-product-card {
    display: flex;
    align-items: center;
    gap: 24px;
    border-radius: 8px;
    position: relative;
    background: #f5f5f5;
}

.tp-product-badge {
    border-radius: 2px 8px;
    background: #ff3f33;
    color: var(--white-color);
    display: flex;
    align-items: center;
    gap: 4px;
    font-size: 12px;
    font-weight: 700;
    line-height: 120%;
    position: absolute;
    top: 0;
    right: 0;
    padding: 4px 8px;
}
.tp-product-badge i {
    position: relative;
    top: 2px;
}

.tp-product-img {
    display: block;
    width: 300px;
    min-width: 300px;
    overflow: hidden;
    border-radius: 8px;
}

.tp-product-img img {
    border-radius: 8px;
    width: 100%;
    transition: all 0.4s ease;
}
.tp-product-card:hover img {
    transform: scale(1.06);
}
.tp-product-content {
    width: 100%;
    padding: 12px 12px 12px 0px;
}

.tp-product-title {
    color: var(--title-color);
    font-style: normal;
    font-size: 20px;
    font-weight: 600;
    line-height: 130%;
    margin-bottom: 8px;
    display: -webkit-box;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;
    overflow: hidden;
}
.tp-product-price {
    display: inline-grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 8px 6px;
    align-items: center;
}
.tp-product-price .disct-price {
    font-size: 18px;
    font-style: normal;
    font-weight: 700;
    line-height: 150%;
    color: var(--primary-color);
}
.tp-product-price .main-price {
    color: var(--hints-color);
    font-size: 18px;
    font-weight: 500;
    line-height: 120%;
    text-decoration-line: line-through;
}
.tp-product-price .save-price {
    border-radius: 99px;
    background: #bfdb38;
    font-size: 12px;
    font-weight: 600;
    line-height: 120%;
    color: var(--title-color);
    padding: 4px 8px;
    text-align: center;
}
.tp-product-action {
    margin-top: 30px;
    display: flex;
    align-items: center;
    gap: 8px;
}

.tp-product-action .theme-btn {
    padding: 12px 16px;
    color: var(--white-color);
    font-size: 14px;
    font-weight: 600;
    line-height: 120%;
    display: flex;
    align-items: center;
    gap: 6px;
    background: var(--primary-color);
    border-radius: 6px;
    transition: all 0.3s ease;
    height: 40px;
}
.tp-product-action .theme-btn i {
    position: relative;
    top: 2px;
}
.tp-product-action .theme-btn:hover {
    background: var(--secondary-color);
}
.tp-product-action .theme-btn.secondary {
    background: transparent;
    color: var(--primary-color);
    border: 1px solid var(--primary-color);
}
.tp-product-action .theme-btn.secondary:hover {
    background: var(--primary-color);
    color: var(--white-color);
}
.product-section-button {
    text-align: center;
    margin-top: 24px;
}
.product-section-button .btn {
    margin: auto;
    text-transform: capitalize;
}

.top-selling-product .quantity-container {
    position: relative;
    display: inline-block;
    width: auto;
}
.top-selling-product .open-quantity-btn {
    font-family: var(--font-family);
    border: 1px solid var(--primary-color) !important;
    color: var(--primary-color);
    width: 100%;
    cursor: pointer;
    font-size: 14px;
    font-weight: 600;
    border-radius: 6px;
    text-align: center;
    transition: all 0.4s ease;
    opacity: 1;
    visibility: visible;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 5px;
    padding: 12px 16px;
    background: transparent;
    height: 40px;
    line-height: 120%;
}
.top-selling-product .open-quantity-btn:hover {
    background: var(--primary-color);
    color: var(--white-color);
    border: 1px solid transparent;
}
.top-selling-product .open-quantity-btn.hidden {
    opacity: 0;
    pointer-events: none;
}
.top-selling-product .open-quantity-btn i {
    position: relative;
    top: 1px;
}
.top-selling-product .quantity-box {
    display: flex;
    align-items: center;
    background-color: transparent;
    border: 1px solid var(--primary-color);
    border-radius: 4px;
    padding: 4px 0;
    gap: 10px;
    transition: all 0.4s ease;
    height: 40px;
    opacity: 1;
    visibility: visible;
    width: 100%;
    justify-content: space-between;
    position: absolute;
    top: 0;
    left: 0;
    overflow: hidden;
}
.top-selling-product .quantity-box.hidden {
    opacity: 0;
    visibility: hidden;
    pointer-events: none;
}

.top-selling-product .decrease-btn,
.top-selling-product .increase-btn {
    border: none;
    font-size: 16px;
    color: var(--white-color);
    cursor: pointer;
    width: 40px;
    height: 40px;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: all 0.2s ease;
    background: var(--primary-color);
}
.top-selling-product .decrease-btn {
    border-radius: 4px 0px 0px 4px;
}
.top-selling-product .increase-btn {
    border-radius: 0px 4px 4px 0px;
}
.top-selling-product .decrease-btn:hover,
.top-selling-product .increase-btn:hover {
    background-color: var(--secondary-color);
    color: var(--white-color);
}

.top-selling-product .quantity {
    color: var(--title-color);
    font-size: 14px;
    font-weight: 700;
    line-height: 110%;
    letter-spacing: 0.7px;
    text-transform: uppercase;
    text-align: center;
    min-width: 20px;
    transition: all 0.2s ease;
}

@media only screen and (min-width: 768px) and (max-width: 1199px) {
    .tp-product-wrapper {
        grid-template-columns: repeat(1, 1fr);
    }
}

@media only screen and (max-width: 767px) {
    .top-selling-product .section-head.style-6 {
        margin-bottom: 16px;
    }
    .tp-product-wrapper {
        gap: 12px;
    }
    .tp-product-card {
        gap: 0;
        flex-wrap: wrap;
    }
    .tp-product-badge {
        font-weight: 600;
    }
    .tp-product-img {
        width: 100%;
        min-width: 100%;
    }
    .tp-product-title {
        font-size: 14px;
        line-height: 120%;
    }

    .tp-product-action {
        margin-top: 12px;
    }

    .tp-product-price .save-price {
        font-size: 12px;
        padding: 4px;
        text-align: center;
    }
    .tp-product-action .theme-btn {
        padding: 8px;
        font-size: 12px;
    }

    .tp-product-content {
        padding: 12px;
    }
    .tp-product-price {
        display: flex;
        flex-wrap: wrap;
        gap: 6px;
    }
    .tp-product-price .disct-price,
    .tp-product-price .main-price {
        font-size: 14px;
    }
    .tp-product-price .save-price {
        font-size: 10px;
        padding: 2px 4px;
    }
    .tp-product-action .theme-btn i {
        font-size: 14px;
        display: block;
        top: 0;
    }
    .tp-product-action .theme-btn {
        padding: 0;
        width: 100%;
        height: 30px;
        text-align: center;
        justify-content: center;
    }
    .product-section-button .btn {
        padding: 8px 16px;
    }
    .d-sm-none {
        display: none !important;
    }
    .top-selling-product .open-quantity-btn {
        font-size: 12px;
        padding: 4px 16px;
        height: 30px;
    }
    .top-selling-product .quantity-container {
        width: 100%;
    }
    .top-selling-product .quantity-box {
        height: 30px;
    }
}

/*============================================================================================
   End Top Selling Product
==============================================================================================*/

```

### brand-style-3.css

```css
/*============================================================================================
   Start Brand CSS 
==============================================================================================*/
.brand-section.style-3 .brands-wrapper {
}
.brand-section.style-3 .brand-card {
    text-align: center;
    border: 1px solid #eee;
    background: var(--white-color);
    margin: 2px 0px;
    border-radius: 8px;
}
.brand-section.style-3 .swiper-slide.brand-card img {
    max-width: 124px;
    height: 88px;
    object-fit: contain;
}
.brand-section.style-3 .swiper-slide {
    display: flex;
    justify-content: center;
}

@media only screen and (max-width: 767px) {
    .brand-section.style-3 .swiper-slide.brand-card img {
        max-width: 100px;
        height: 80px;
    }
}

/*============================================================================================
   End Brand CSS
==============================================================================================*/

```

### card-style-6.css

```css
/*============================================================================================
   Start Card CSS
==============================================================================================*/
.product.style-6 {
    position: relative;
    font-family: Poppins, sans-serif;
    font-size: 1rem;
    color: #020101;
    transition:
        box-shadow 0.3s,
        opacity 0.3s;
    padding: 8px;
    border: 1px solid var(--border-color);
    border-radius: 4px;
    background: var(--white-color);
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    height: 100%;
}

.product.style-6 .flag-name {
    position: absolute;
    top: 6px;
    left: 6px;
    background: var(--primary-color);
    color: var(--white-color);
    font-size: 10px;
    padding: 2px 6px;
    border-radius: 4px;
}
.product.style-6 .save-label {
    position: absolute;
    top: 6px;
    right: 6px;
    background: var(--success-color);
    color: var(--white-color);
    font-size: 10px;
    padding: 2px 6px;
    border-radius: 4px;
}

.swiper-slide:has(.product.style-6) {
    height: unset;
}

.product.style-6 .product-price {
    display: block;
    text-overflow: ellipsis;
    overflow: hidden;
    white-space: nowrap;
}
.product.style-6 .product-media {
    position: relative;
    margin-bottom: 0;
    transition: box-shadow 0.35s;
    overflow: hidden;
}

.product.style-6 .offer-countdown {
    position: absolute;
    bottom: 10px;
    left: 50%;
    transform: translateX(-50%);
    margin-top: 0;
    padding: 5px 20px;
    padding-bottom: 10px;
    border-radius: 5px;
    background: rgba(255, 255, 255, 0.5); /* semi-transparent */
    backdrop-filter: blur(5px); /* now blur will show */
    -webkit-backdrop-filter: blur(5px); /* for Safari support */
}

.product.style-6 .product-media img {
    display: block;
    width: 100%;
    transition: all 0.3s ease;
}
.product.style-6:hover .product-media img {
    transform: scale(1.04);
}
.product.style-6 .product-media img:last-child {
    position: absolute;
    opacity: 0;
    left: 0;
    right: 0;
    bottom: 0;
    top: 0;
    object-fit: cover;
}
.product.style-6 .product-media img:first-child {
    position: relative;
    opacity: 1;
}
.product.style-6 .product-media:hover img:last-child {
    opacity: 1;
}
.product.style-6 .product-label-group {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    position: absolute;
    left: 2rem;
    top: 2rem;
    max-width: 11rem;
    z-index: 1;
}
.product.style-6 .product-label {
    display: inline-block;
    text-align: center;
    margin-bottom: 0.5rem;
    font-size: 1.2em;
    letter-spacing: -0.005em;
    color: #fff;
    line-height: 1;
    padding: 0.6rem 1rem;
    border-radius: 0.3rem;
    text-transform: uppercase;
}
.product.style-6 .product-label.label-new {
    background: var(--secondary-color);
    padding: 0.6rem 1rem;
}
.product.style-6 .product-label.label-discount {
    background: var(--secondary-color);
}
.product.style-6 .product-label.label-hot {
    background: #799b5a;
}

.product.style-6 .product-sales-wrapper {
    position: relative;
    margin-bottom: 1.4rem;
}
.product.style-6 .product-sales-wrapper .progress-bar {
    height: 1rem;
    background: #eee;
    margin-bottom: 0.7rem;
}
.product.style-6 .product-sales-wrapper .product-sales-count {
    color: #666;
    font-size: 1.3rem;
}
.product.style-6 .product-sales-wrapper .product-sales-count mark {
    background-color: transparent;
    color: inherit;
}
.product.style-6 .btn-product {
    display: flex;
    align-items: center;
    justify-content: center;
    flex: 1;
    padding: 1.14em 0;
    line-height: 1.2;
    background-color: var(--secondary-color);
    color: #fff;
    font-size: 1.4em;
    font-weight: 600;
    letter-spacing: -0.025em;
    text-transform: uppercase;
    transition: opacity 0.3s;
}
.product.style-6 .btn-product:active,
.product.style-6 .btn-product:hover {
    color: #fff;
}
.product.style-6 .btn-product.btn-link {
    display: inline-block;
    font-weight: 600;
    font-size: 1.3rem;
    line-height: 1.6;
    flex: none;
    background-color: transparent;
    padding: 0;
}
.product.style-6 .btn-product.btn-link:after {
    margin-top: 0;
}
.product.style-6 .btn-product.btn-sm {
    padding: 0.693em 0;
}
.product.style-6 .btn-wishlist {
    line-height: 1;
}
.product.style-6 .btn-wishlist i:before {
    margin: 0;
}
.product.style-6 .btn-cart {
    font-weight: 600;
}
.product.style-6 .btn-cart.btn-link {
    color: var(--secondary-color);
}
.product.style-6 .btn-cart.btn-link:active,
.product.style-6 .btn-cart.btn-link:hover {
    color: var(--secondary-color);
}
.product.style-6 .btn-cart:not(.btn-product-icon) i.w-icon-cart {
    font-size: 1.8rem;
    margin-bottom: 0.4rem;
}
.product.style-6 .btn-product-icon {
    margin: 0;
    border-radius: 4px;
    min-width: 40px;
    height: 40px;
    color: var(--primary-color);
    border-color: var(--primary-color);
}
.product.style-6 .btn-product-icon:hover {
    color: var(--white-color);
}
.product.style-6 .product-details {
    position: relative;
    padding-top: 1.2rem;
    background-color: #fff;
    transition: box-shadow 0.3s;
}
.product.style-6 .product-name {
    margin-bottom: 0.8rem;
    font-size: 16px;
    font-weight: 500;
    white-space: initial;
    overflow: hidden;
    text-overflow: ellipsis;
}
.product.style-6 .product-name a {
    color: inherit;
}
.product.style-6 .product-name a:hover {
    color: var(--secondary-color);
}
.product.style-6 .product-card-action {
    margin-top: 12px;
    display: flex;
    align-items: center;
    gap: 16px;
}

.product.style-6 .product-card-action .order-btn {
    font-family: var(--font-family);
    border: 1px solid var(--primary-color) !important;
    color: var(--primary-color);
    width: 100%;
    cursor: pointer;
    font-size: 14px;
    font-weight: 600;
    border-radius: 4px;
    text-align: center;
    transition: all 0.4s ease;
    opacity: 1;
    visibility: visible;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 5px;
    padding: 8px;
    background: transparent;
    height: 40px;
    line-height: 120%;
}
.product.style-6 .product-card-action .order-btn:hover {
    background: var(--primary-color);
    color: var(--white-color);
}
.product.style-6 .quantity-container {
    position: relative;
    display: inline-block;
    width: 100%;
}
.product.style-6 .open-quantity-btn {
    font-family: var(--font-family);
    border: 1px solid var(--primary-color) !important;
    color: var(--primary-color);
    width: 100%;
    cursor: pointer;
    font-size: 14px;
    font-weight: 600;
    border-radius: 4px;
    text-align: center;
    transition: all 0.4s ease;
    opacity: 1;
    visibility: visible;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 5px;
    padding: 8px;
    background: transparent;
    height: 40px;
    line-height: 120%;
}
.product.style-6 .open-quantity-btn:hover {
    background: var(--primary-color);
    color: var(--white-color);
    border: 1px solid transparent;
}
.product.style-6 .open-quantity-btn.hidden {
    opacity: 0;
    pointer-events: none;
}
.product.style-6 .open-quantity-btn i {
    position: relative;
    top: 1px;
}
.product.style-6 .quantity-box {
    display: flex;
    align-items: center;
    background-color: transparent;
    border: 1px solid var(--primary-color);
    border-radius: 4px;
    padding: 4px 0;
    gap: 10px;
    transition: all 0.4s ease;
    height: 40px;
    opacity: 1;
    visibility: visible;
    width: 100%;
    justify-content: space-between;
    position: absolute;
    top: 0;
    left: 0;
    overflow: hidden;
}
.product.style-6 .quantity-box.hidden {
    opacity: 0;
    visibility: hidden;
    pointer-events: none;
}

.product.style-6 .decrease-btn,
.product.style-6 .increase-btn {
    border: none;
    font-size: 16px;
    color: var(--white-color);
    cursor: pointer;
    width: 40px;
    height: 40px;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: all 0.2s ease;
    background: var(--primary-color);
}
.product.style-6 .decrease-btn {
    border-radius: 4px 0px 0px 4px;
}
.product.style-6 .increase-btn {
    border-radius: 0px 4px 4px 0px;
}
.product.style-6 .decrease-btn:hover,
.product.style-6 .increase-btn:hover {
    background-color: var(--secondary-color);
    color: var(--white-color);
}

.product.style-6 .quantity {
    color: var(--title-color);
    font-size: 14px;
    font-weight: 700;
    line-height: 110%;
    letter-spacing: 0.7px;
    text-transform: uppercase;
    text-align: center;
    min-width: 20px;
    transition: all 0.2s ease;
}

.product.style-6 .product-card-action .order-btn.stock-out {
    border: 1px solid var(--alert-color) !important;
    color: var(--alert-color);
    cursor: no-drop;
}
.product.style-6 .product-card-action .order-btn.stock-out:hover {
    background: transparent;
}

.variant-colors {
    margin-top: 6px;
    display: flex;
    gap: 6px;
    flex-wrap: wrap;
}

.color-dot {
    width: 14px;
    height: 14px;
    border-radius: 50%;
    border: 1px solid #ddd;
    display: inline-block;
    cursor: pointer;
}

.color-dot:hover {
    border-color: #000;
}
@media (max-width: 767px) {
    .product.style-6:not(.product-list) {
        font-size: 0.8rem;
    }
    .product.style-6 .product-card-action {
        justify-content: center;
        gap: 12px;
        margin-top: 8px;
    }
    .product.style-6 .btn-wishlist {
        display: none;
    }
    .product.style-6 .quantity-container {
        width: 100%;
    }
    .product.style-6 .open-quantity-btn {
        font-size: 12px;
        height: 34px;
    }
    .product.style-6 .quantity-box {
        height: 34px;
    }
    .product.style-6 .decrease-btn,
    .product.style-6 .increase-btn {
        width: 34px;
        height: 34px;
    }
    .product.style-6 .product-name {
        min-height: 34px;
        height: 34px;
        font-size: 14px;
    }
    .product.style-6 .btn-product-icon {
        display: none;
    }
    .product.style-6 .product-card-action .order-btn {
        height: 34px;
        font-size: 12px;
    }
    .product.style-6 .product-card-action .order-btn i {
        font-size: 0;
    }
}

/*============================================================================================
   End Card CSS
==============================================================================================*/

```

### product-section-style-4.css

```css
/*============================================================================================
   Start Product Section CSS
==============================================================================================*/
.product-slider.style-4 .swiper-container.product-deals-wrapper {
    padding: 12px 0px;
}
/*============================================================================================
   End Product Section CSS
==============================================================================================*/

```

### testimonial-style-1.css

```css
/*============================================================================================
   Start Testimonial CSS
==============================================================================================*/
.testimonial-style-1 .testimonial-card {
	border: 1px solid var(--border-color);
	border-radius: 16px;
	padding: 24px;
	min-height: 224px;
	margin-top: 1px;
}
.testimonial-style-1 .testimonial-author {
  display: flex;
  align-items: center;
  gap: 8px;
}
.testimonial-style-1 .testimonial-author-thumbnail img {
  width: 60px;
  height: 60px;
  border-radius: 100%;
  object-fit: cover;
}
.testimonial-style-1 .testimonial-info {
  margin: 0;
}
.testimonial-style-1 .testimonial-info h5 {
  margin: 0;
  font-weight: 500;
  font-size: 14px;
  line-height: 120%;
}
.testimonial-style-1 .testimonial-info {
  display: block;
}
.testimonial-style-1 .testimonial-info span {
  font-size: 12px;
  font-weight: 400;
  display: block;
  margin-top: 4px;
}
.testimonial-style-1 .testimonial-text {
  font-size: 14px;
  display: -webkit-box;
  -webkit-line-clamp: 4;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

/*============================================================================================
   End Testimonial CSS
==============================================================================================*/

```

### footer-style-3.css

```css
/*=============================================================================================
    Start Footer Area
===============================================================================================*/
.footer.style-3 {
    background-color: #f5f5f5;
    background-size: cover;
    background-position: center;
    background-repeat: no-repeat;
}
.footer.style-3 .footer-top {
    padding: 32px 0px 72px;
    border: none;
}
.footer.style-3 .footer-logo img {
    max-width: 170px;
    height: 48px;
    object-fit: contain;
}
.footer.style-3 .f-about-text {
    color: var(--body-color);
    font-size: 14px;
    font-weight: 400;
    line-height: 150%;
    margin-top: 20px;
    margin-bottom: 0;
}
.footer.style-3 .footer-contact {
    margin-top: 28px;
    padding: 0;
    list-style: none;
}
.footer.style-3 .footer-contact li a {
    color: var(--body-color);
    display: inline-flex;
    align-items: center;
    font-weight: 400;
    line-height: 150%;
    font-size: 16px;
}
.footer.style-3 .footer-contact li a:hover {
    color: var(--primary-color);
}
.footer.style-3 .footer-contact li {
    color: var(--body-color);
    align-items: center;
    font-weight: 400;
    line-height: 150%;
    font-size: 16px;
    padding-bottom: 7px;
}

.footer.style-3 .footer-contact li i {
    margin-right: 8px;
}

.footer.style-3 .footer-contact li:last-child {
    margin: 0;
}
.footer.style-3 .footer-social {
    margin-top: 28px;
    padding: 0;
    margin-bottom: 0;
}
.footer.style-3 .footer-social li {
    display: inline-block;
    margin-right: 8px;
}
.footer.style-3 .footer-social li a {
    width: 40px;
    height: 40px;
    background: var(--white-color);
    border-radius: 100%;
    text-align: center;
    line-height: 44px;
    color: var(--primary-color);
    display: block;
    font-size: 16px;
    transition: all 0.3s ease;
}
.footer.style-3 .footer-social li a:hover {
    background: var(--primary-color);
    color: var(--white-color);
}
.footer.style-3 .footer-social li:last-child {
    margin: 0;
}
.footer.style-3 .footer-widget {
    margin-top: 40px;
}
.footer.style-3 .footer-widget-title {
    font-size: 16px;
    font-weight: 500;
    line-height: 130%;
    color: var(--title-color);
    margin-bottom: 16px;
}
.footer.style-3 .footer-widget-list {
    padding: 0;
    margin: 0;
}
.footer.style-3 .footer-widget-list li {
    margin-bottom: 8px;
    list-style: none;
}
.footer.style-3 .footer-widget-list li:last-child {
    margin: 0;
}
.footer.style-3 .footer-widget-list li a {
    font-size: 14px;
    font-weight: 400;
    line-height: 120%;
    color: var(--body-color);
}
.footer.style-3 .footer-widget-list li a:hover {
    color: var(--primary-color);
}
.footer.style-3 .f-widget-text {
    font-size: 14px;
    font-weight: 400;
    line-height: 120%;
    color: var(--body-color);
}
.footer.style-3 .footer-newsletter {
    position: relative;
    margin-top: 16px;
    padding: 0;
}
.footer.style-3 .footer-newsletter input {
    width: 100%;
    height: 48px;
    padding: 16px 50px 16px 16px;
    border-radius: 8px;
    font-size: 14px;
    font-weight: 500;
    line-height: 120%;
    color: #626571;
    background: #fff;
    border: none;
    text-align: left;
}
.footer.style-3 .footer-newsletter button {
    position: absolute;
    top: 0;
    right: 0;
    width: 48px;
    height: 48px;
    background: var(--primary-color);
    border-radius: 0px 8px 8px 0px;
    border: none;
    cursor: pointer;
}
.footer.style-3 .footer-download-app {
    margin-top: 32px;
}
.footer.style-3 .footer-download-app h4 {
    font-size: 16px;
    font-style: normal;
    font-weight: 400;
    line-height: 150%;
    color: var(--body-color);
    margin-bottom: 4px;
}
.footer.style-3 .footer-download-app .title {
    font-size: 16px;
    font-weight: 500;
    color: var(--title-color);
    margin-bottom: 6px;
}
.footer.style-3 .footer-download-app p {
    font-size: 14px;
    font-weight: 400;
    line-height: 120%;
    color: var(--body-color);
    margin: 0;
}
.footer.style-3 .f-download-app-links {
    display: flex;
    align-items: center;
    gap: 12px;
    margin-top: 20px;
}
.footer.style-3 .footer-bottom {
    display: block;
    padding: 0;
    background: transparent;
}
.footer.style-3 .footer-bottom-widget {
    display: flex;
    align-items: center;
    justify-content: space-between;
    border-top: 1px solid var(--border-color);
    padding: 16px 0px;
}
.footer.style-3 .f-copyight-text {
    font-size: 14px;
    font-weight: 400;
    line-height: 150%;
    color: var(--body-color);
    margin: 0;
}
.footer.style-3 .f-copyight-text a {
    color: var(--white-color);
}
.footer.style-3 .f-copyight-text a:hover {
    color: var(--primary-color);
}
.footer.style-3 .footer-payment-img {
    max-width: 50%;
}
.footer.style-3 .getcommerce-copyright {
    display: block;
    line-height: 120%;
    margin-top: 2px;
}
.footer.style-3 .getcommerce-copyright a {
    color: var(--primary-color);
    font-weight: 500;
}

@media only screen and (min-width: 768px) and (max-width: 1199.99px) {
    .footer.style-3 .footer-bottom-widget {
        text-align: center;
        display: flex;
        flex-direction: column-reverse;
    }
    .footer.style-3 .footer-payment-img {
        margin-bottom: 12px;
        text-align: center;
        max-width: 100%;
    }
}

@media only screen and (max-width: 767px) {
    .footer.style-3 .footer-top {
        padding: 16px 0px 56px;
    }
    .footer.style-3 .footer-bottom-widget {
        display: flex;
        text-align: center;
        flex-direction: column-reverse;
        padding-bottom: 80px;
    }
    .footer.style-3 .f-copyight-text {
        margin-top: 8px;
    }
    .footer.style-3 .footer-payment-img {
        max-width: 100%;
    }
}

/*=============================================================================================
    End Footer Area
===============================================================================================*/

```

### style-1.css

```css
/*============================================================================================
   Start Bottom Navigation CSS 
==============================================================================================*/
.sticky-footer.style-1 {
    display: flex;
    -webkit-box-align: center;
    -ms-flex-align: center;
    align-items: center;
    padding: 0 4rem;
    background: #fff;
    position: fixed;
    bottom: 0;
    left: 0;
    width: 100%;
    z-index: 1010101;
    background: var(--primary-color);
}
.sticky-footer.style-1 > * {
    -webkit-box-flex: 1;
    -ms-flex: 1;
    flex: 1;
}
.sticky-footer.style-1 .cart-dropdown {
    position: relative;
}
.sticky-footer.style-1 .cart-dropdown .dropdown-box {
    top: auto;
    bottom: 100%;
    right: 0;
    min-width: 34rem;
    padding: 2rem 3rem 3rem;
    margin-bottom: 2.1rem;
    -webkit-transform: translateY(20px);
    transform: translateY(20px);
    -webkit-transition: opacity 0.3s, visibility 0.3s, -webkit-transform 0.3s;
    transition: opacity 0.3s, visibility 0.3s, -webkit-transform 0.3s;
    transition: transform 0.3s, opacity 0.3s, visibility 0.3s;
    transition: transform 0.3s, opacity 0.3s, visibility 0.3s,
        -webkit-transform 0.3s;
}
.sticky-footer.style-1 .cart-dropdown:hover .dropdown-box {
    -webkit-transform: translateY(0);
    transform: translateY(0);
    opacity: 1;
    visibility: visible;
}
.sticky-footer.style-1 .products {
    border: none;
}
.sticky-footer.style-1 .product-name a {
    padding: 0;
}
.sticky-footer.style-1 .cart-total {
    padding-top: 1rem;
}

.sticky-footer.style-1 .sticky-link {
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    -webkit-box-orient: vertical;
    -webkit-box-direction: normal;
    -ms-flex-direction: column;
    flex-direction: column;
    -webkit-box-align: center;
    -ms-flex-align: center;
    align-items: center;
    color: var(--title-color);
}
.sticky-footer.style-1 .sticky-link i,
.sticky-footer.style-1 .cart-dropdown .sticky-link i {
    font-size: 16px;
    color: var(--white-color);
}
.sticky-footer.style-1 .cart-icon-widget p {
    margin-top: 14px;
    position: relative;
}
.sticky-footer.style-1 .sticky-link p {
    margin-top: 8px;
    margin-bottom: 0;
    font-size: 1rem;
    line-height: 1;
    letter-spacing: 0.025em;
    text-transform: uppercase;
    color: var(--white-color);
}
.sticky-footer.style-1 .sticky-link.search-toggle p {
    margin-top: 1rem;
}
.sticky-footer.style-1 .sticky-link:hover {
    color: var(--white-color);
}
.sticky-footer.style-1 .sticky-link:hover i {
    color: inherit;
}

@media (min-width: 768px) {
    .sticky-footer.style-1 {
        display: none;
    }
}
@media (max-width: 479px) {
    .sticky-footer.style-1 {
        padding: 0;
    }
    .sticky-footer.style-1 .hs-toggle .input-wrapper {
        right: 1rem;
    }
    .sticky-footer.style-1 .cart-dropdown .dropdown-box {
        min-width: 30rem;
        right: -5.4rem;
    }
}

/*============================================================================================
   End Bottom Navigation CSS 
==============================================================================================*/

```

### Final homepage overrides (selected)

```css
:root { --primary-color:#f48721; --secondary-color:#041f1e; }
main {background-color:#fbf9f5;}
.tp-product-card {background:#fff;}
.header.style-7 .header-bottom {background:var(--secondary-color);padding:0;}
.header.style-7 .menu > li.active > a:not(.menu-title),
.header.style-7 .menu > li:hover > a:not(.menu-title) {color:var(--primary-color)!important;}
.footer.style-3 {background:#fff;}
.footer.style-3 .footer-social li a {background:#fbf9f5;}
.tp-product-action .theme-btn.secondary {background:#fff2ea;}
.section-padding {padding:16px 0;}
.cart-bounce {animation:none;}
```
