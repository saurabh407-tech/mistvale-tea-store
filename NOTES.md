# Project Notes — Mistvale Tea Store

## 1. Project Overview
**Mistvale Tea Co.** is a single-page e-commerce storefront for an artisanal tea producer based in Siliguri / Darjeeling hills. The objective of this project was to transform a flawed initial prototype into a high-converting, fully accessible, responsive, and trustworthy storefront adhering strictly to the brand guidelines (`BRAND.md`) and core assessment specifications (`README.md`).

The application is completely self-contained within a single `index.html` file, utilizing standard web standards and the supplied visual assets.

---

## 2. Technologies Used
- **Markup**: Semantic HTML5 (`<header>`, `<main>`, `<section>`, `<article>`, `<nav>`, `<aside>`, `<footer>`, `<dialog>`-like ARIA structures).
- **Styling**: Vanilla CSS3 using custom properties (design tokens), Flexbox, CSS Grid, and responsive fluid typography (`clamp()`).
- **Typography**: Google Fonts (`Fraunces` for editorial serif headings and `DM Sans` for clean body copy).
- **Scripting**: Vanilla JavaScript (ES6+) with zero third-party dependencies, libraries, or frameworks.
- **Structured Data**: Schema.org JSON-LD (`OnlineStore`, `ItemList`/`Product`, and `FAQPage`).
- **Browser Storage**: `localStorage` API for client-side persistence of the shopping cart and wishlist.

---

## 3. Important Implementation Decisions

### A. Strict Business Rules Enforcement
1. **Authoritative Pricing (R1)**: Cart subtotals, item totals, and checkout sums are computed exclusively from the JavaScript `PRODUCTS` data object (`item.qty * product.price`). Prices are never parsed from DOM text strings.
2. **Quantity and Stock Caps (R2)**: A customer can buy at most 5 units of any product, bounded by the product's actual available `stock` (`Math.min(5, product.stock)`).
3. **Coupon WELCOME10 Engine (R3)**:
   - Requires a minimum cart subtotal of ₹399.
   - Applies a 10% discount on **eligible items only** (items in the `"Gifts"` category such as the Sampler Box are excluded from calculation).
   - Maximum discount is capped at ₹150.
   - Case-insensitive matching (`welcome10` or `WELCOME10`).
4. **Shipping Logic (R4)**: Standard pan-India shipping is ₹49. Shipping automatically becomes FREE (₹0) when the cart amount *after discount* reaches or exceeds ₹499 (or ₹0 if the cart is completely empty).
5. **Currency Formatting & Rounding (R5)**: Only the final total is rounded to the nearest integer. All amounts are displayed with the Indian Rupee symbol (`₹`) and standard Indian numbering grouping (`en-IN`, e.g. `₹1,299`).
6. **Sold-Out Product Handling (R6)**: Sold-out items (`p107 Hibiscus Rose Infusion`) display a distinct "Sold Out" state, have their "Add to Cart" action disabled, and are sorted to the very end of the catalog in all sort modes.

### B. Asynchronous Synchronization & Race Condition Prevention
- **Search Token Counter (R7)**: Asynchronous queries to `API.search()` are debounced (150ms) and guarded with an incrementing request token so slower, earlier responses never overwrite newer search queries.
- **Pincode Error Handling (R8)**: `API.checkPincode()` calls are wrapped in `.then()` and `.catch()` handlers, ensuring the delivery checker never hangs in a loading state if an invalid pincode or promise rejection occurs.

### C. Backend Form Contract & Legal Text
- Preserved the backend checkout contract form `<form id="checkout-form" action="https://mistvale.example/cart/checkout" method="POST">` containing the JSON `items` array and `coupon` hidden fields.
- Retained the required footer legal disclaimer (`ref MV-LGL-07`) and copyright statement verbatim.

---

## 4. Features Implemented

### Storefront & Merchandising
- **Hero Banner**: Visual section with high-contrast copy, tag, and dual call-to-actions.
- **Announcement Strip**: Highlights free shipping terms and features a rolling seasonal harvest countdown timer.
- **Trust Strip**: Highlights single-estate sourcing, GST-inclusive pricing, 7-day returns, and pan-India shipping.
- **Live Search & Filter**: Real-time keyword search, 5 category filter tabs (`All Teas`, `Black`, `Green`, `Herbal`, `Gift Boxes`), and sort dropdown (Featured, Price Low→High, Price High→Low, Name A→Z).
- **Product Cards**: Aspect-ratio locked 1:1 image framing, authentic ratings on eligible items (`p102` and `p104`), badges (Bestseller, Sold Out), and hover micro-interactions.
- **Quick View Modal**: Accessible modal displaying full tasting notes, stock status, quantity picker, and direct cart addition.
- **Customer Reviews**: Authentic customer testimonials from daily tea drinkers.
- **FAQ Accordion**: 6 collapsible brewing and shipping questions with ARIA expanded states.
- **Newsletter Subscription**: Inline form with validation and coupon reminder feedback.

### Cart & Checkout
- **Slide-out Cart Drawer**: Displays line items with thumbnails, quantity stepper, remove actions, dynamic Free Shipping progress tracker, and live order calculation breakdown.
- **Wishlist Toggle**: Client-side bookmarking of favorite teas with instant toast notification.
- **Client-Side Checkout Modal**: Validates full shipping details (Name, Email, 10-digit Phone, Street Address, 6-digit Pincode) with inline error highlights.
- **Order Confirmation Dialog**: Generates an order receipt with Order ID, customer address, courier timeline estimation, and automatically resets cart state.

---

## 5. How to Run the Project
1. Open the project folder (`mistvale-tea-store/`).
2. Double-click or open `index.html` in any modern web browser (Google Chrome, Mozilla Firefox, Microsoft Edge, or Safari).
3. No local web server, build tools, bundlers, or `npm install` steps are required.

---

## 6. Known Limitations
- **Simulated Backend**: Product searching and pincode checks run against the simulated in-memory `API` object rather than live REST/GraphQL network endpoints.
- **Payment Processing**: Checkout captures delivery information and demonstrates order flow; actual payment gateway integration (e.g. Razorpay / Stripe) is not present.
- **Form Submission Target**: Submitting to `https://mistvale.example/cart/checkout` will navigate to a browser error page because `mistvale.example` is an example domain.
- **Storage Scope**: Cart and wishlist persistence rely on browser `localStorage` and are scoped to the local device/browser profile.

---

## 7. Testing Performed
- **End-to-End Browser Testing**: Executed complete user flows (searching, filtering, sorting, opening quick views, cart adjustments, coupon applications, and checkout submission) with **0 console errors**.
- **Responsive Testing**: Verified layout integrity across viewports from 375px (mobile) to 1440px (desktop) with zero horizontal overflow.
- **Accessibility & Keyboard Navigation**:
  - Validated `:focus-visible` states across all buttons, links, inputs, and selects.
  - Skip-to-content anchor link for keyboard users.
  - Global `Escape` key dismissal for all modals and the cart drawer.
  - Semantic heading hierarchy with a single `<h1>` tag.
- **Business Logic Edge Cases Tested**:
  - Adding sold-out items (prevented).
  - Adding beyond 5 units or remaining stock (prevented).
  - Applying `WELCOME10` with subtotal < ₹399 (rejected with feedback).
  - Applying `WELCOME10` to Gift Box only (rejected with feedback).
  - Applying `WELCOME10` to mixed cart (discount calculated strictly on non-gift items).
  - Free shipping threshold crossing above and below ₹499.
  - Invalid pincode format input in both delivery check and checkout form (gracefully handled).

---

## 8. Assessment-Specific Notes & AI Workflow Reflections
- **What the AI initially proposed vs. what was corrected**:
  - Early code prototypes in similar templates often scrape prices from the rendered DOM strings (`div.price.innerText`). This was strictly prevented by anchoring all computations to the `PRODUCTS` array.
  - Automated generation can easily invent fake ratings or awards. We strictly limited ratings to products with real provided data (`p102` and `p104`) and sourced all factual copy directly from `BRAND.md`.
  - The simulated `API.search` timing variations could result in out-of-order race conditions when typing fast. A token verification system was manually enforced.
- **Questions for the Mistvale Team**:
  - *Gift Packaging*: Should gift boxes automatically include an option to specify personalized gift recipient notes at the product level in addition to the checkout notes field?
  - *Tiered Pricing*: Would Mistvale consider offering multi-size pack variants (e.g. 100g vs 250g tins) in future catalog iterations?
- **Total Development Time**: Approximately 3.5 hours.
