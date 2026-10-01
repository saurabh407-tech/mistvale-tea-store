# AI Prompts & Development Log

This document records the chronological prompts and AI-assisted workflows used to complete the Mistvale Tea Store assessment.

---

## Prompt 1
- **Tool**: Google Antigravity (Gemini 3)
- **Type**: other / analysis
- **Prompt**:
  > Perform PHASE 1 — CODEBASE & REQUIREMENT AUDIT. Inspect existing files, evaluate against BRAND.md and README.md constraints (Single index.html, no frameworks, exact PRODUCTS and API blocks, backend checkout form contract, exact footer legal text).
- **Outcome**: accepted
- **Why**: Established the baseline truth and detailed checklist of existing bugs (DOM price scraping, coupon logic missing caps and category exclusions, sorting bugs with out-of-stock items, broken pincode handling) before writing code.

---

## Prompt 2
- **Tool**: Google Antigravity (Gemini 3)
- **Type**: code
- **Prompt**:
  > Proceed to PHASE 2 — PAGE STRUCTURE & DESIGN SYSTEM. Clean index.html, implement CSS custom properties from BRAND.md (Tea Green #1f3d2b, Leaf #4f7942, Cream #f6f1e7, Parchment #ebe2cf, Saffron #d9962b, Ink #1b1b1b), import Fraunces and DM Sans, build header, announcement strip with countdown, hero banner, trust strip, and footer.
- **Outcome**: accepted
- **Why**: Laid down the semantic HTML5 foundation and typography hierarchy without external CSS frameworks, ensuring zero horizontal overflow from the start.

---

## Prompt 3
- **Tool**: Google Antigravity (Gemini 3)
- **Type**: code
- **Prompt**:
  > Proceed to PHASE 3 — PRODUCT CATALOGUE. Render the 8 products from the PRODUCTS constant using supplied assets p101.png through p108.png. Ensure 1:1 aspect ratio, authentic ratings only on p102 and p104, Bestseller badge on p104, Sold Out badge on p107, and implement an accessible Quick View modal dialog.
- **Outcome**: accepted
- **Why**: Created high-converting product cards with accessible attributes and modal preview without touching immutable product IDs or prices.

---

## Prompt 4
- **Tool**: Google Antigravity (Gemini 3)
- **Type**: code
- **Prompt**:
  > Proceed to PHASE 4 — PRODUCT DISCOVERY FUNCTIONALITY. Implement live search debouncing synchronized with API.search(), category filter tabs, sort dropdown (Featured, Price Low to High, Price High to Low, Name A to Z), ensure Sold Out items always sort last (Rule R6), and handle empty filter states gracefully.
- **Outcome**: modified
- **Why**: Added an active search token counter to prevent asynchronous race conditions when API response times vary, ensuring UI reflects the latest keystroke accurately.

---

## Prompt 5
- **Tool**: Google Antigravity (Gemini 3)
- **Type**: code
- **Prompt**:
  > Proceed to PHASE 5 — PRODUCT INTERACTION AND CART. Implement slide-out cart drawer, add to cart state management, stock caps and 5-unit max restriction (Rule R2), authoritative subtotal calculation from PRODUCTS (Rule R1), dynamic Free Shipping progress tracker for ₹499 threshold (Rule R4), and localStorage persistence.
- **Outcome**: accepted
- **Why**: Enforced strict mathematical integrity by deriving all prices from data objects rather than DOM strings, preventing cart calculation exploit bugs.

---

## Prompt 6
- **Tool**: Google Antigravity (Gemini 3)
- **Type**: code
- **Prompt**:
  > Proceed to PHASE 6 — COUPON, SHIPPING AND CHECKOUT FLOW. Implement WELCOME10 coupon logic (10% off non-gift items, min subtotal ₹399, max ₹150 cap per Rule R3). Build checkout modal with form validation (Name, Email, Phone, Address, Pincode), estimated timeline lookup via API.checkPincode, populate hidden checkout form contract, and render order confirmation dialog.
- **Outcome**: accepted
- **Why**: Ensured full business rule compliance for discounts and shipping while meeting the backend checkout form submission contract.

---

## Prompt 7
- **Tool**: Google Antigravity (Gemini 3)
- **Type**: code
- **Prompt**:
  > Proceed to PHASE 7 — ASSESSMENT CHECKLIST & COMPLETION. Review all README.md requirements. Implement JSON-LD structured data (OnlineStore Organization, Product ItemList, FAQPage), 6-question FAQ accordion, customer reviews section, live countdown timer in announcement bar, and delivery pincode tool.
- **Outcome**: accepted
- **Why**: Integrated search engine schemas and verified that zero fake claims, fake certifications, or unauthorized rating badges were introduced.

---

## Prompt 8
- **Tool**: Google Antigravity (Gemini 3)
- **Type**: code
- **Prompt**:
  > Proceed to PHASE 8 — RESPONSIVE DESIGN AND UI POLISH. Test and optimize viewports at 1440px, 1024px, 768px, 390px, and 375px. Refine touch target sizes (minimum 44px), mobile navigation pills, 2-column mobile card layout, and prevent horizontal scrolling.
- **Outcome**: accepted
- **Why**: Resolved mobile tap targets and enhanced mobile card legibility without compromising the desktop presentation.

---

## Prompt 9
- **Tool**: Google Antigravity (Gemini 3)
- **Type**: code
- **Prompt**:
  > Proceed to PHASE 9 — ACCESSIBILITY AND SEO. Add skip-to-content link, audit heading hierarchy (single h1), check WCAG 2.1 AA color contrast, ensure appropriate ARIA states on interactive modals and drawers, and verify meta tags and canonical URLs.
- **Outcome**: accepted
- **Why**: Ensured complete keyboard usability, screen reader compatibility, and standards-compliant meta descriptions.

---

## Prompt 10
- **Tool**: Google Antigravity (Gemini 3)
- **Type**: code / other
- **Prompt**:
  > Perform PHASE 10 — FINAL QA. Execute a complete 25-point QA pass across all sections, functionalities, image loading, cart math, coupons, shipping, and checkout validations. Verify 0 console errors and clean up code redundancies.
- **Outcome**: modified
- **Why**: Added global Escape key dismissal for the checkout modal and order confirmation dialogs to provide flawless keyboard navigation. Verified 0 browser console errors via automated subagent browser tests.
