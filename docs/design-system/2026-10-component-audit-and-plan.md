# Shilpangon Design-System-v1: component audit and implementation plan

**Status:** Audit and proposal only. **Nothing in Figma has been changed.** Every Figma change below waits for team approval.
**Source file:** Design-System-v1 (`vgzcpBGmqSlC2d9qIL6JZK`). I inspected all 33 pages read-only on 2026-10-11.
**Brief:** *Shilpangon Design System Review & Component Expansion Brief for Claude*.

Labels used in this document:
- **[Observed]** is a fact read from the Figma file, with node IDs.
- **[Recommendation]** is my design advice.
- **(proposed)** marks any number that is not already in the file.

---

## 1. Executive verdict

**Reuse as-is (do not rebuild):** Button, Input Field, Search/Field, Chip, Badge, App Bar, Bottom Navigation, Tabs, Checkbox/Radio/Toggle, Avatar, OTP, Feedback/Alert, Selection/Choice Card, Progress (Step Indicator, Bar, Dots), Icon Wrapper and the 119-icon library. The library is in better shape than the brief assumes. Search, filter chips, tabs, step progress and a search empty/error/loading state already exist.

**Build now (P0, 7 items):**
1. `Card/Summary Tile`: promote `WF/Summary Tile`.
2. `List/Order Row`: promote `WF/Recent Order Row`.
3. `List/Link Row`: promote `WF/Link Row`.
4. **Extend, don't duplicate:** generalize `Search/Feedback State` into a shared feedback state, adding a compact Empty variant taken from `WF/Empty State`.
5. **Extend, don't duplicate:** turn `Search/Result Item` into `Card/Product` with buyer/seller, availability and loading variants.
6. `Input/Upload`: hi-fi SE-04 already repeats a local "Upload / …" row 3 times.
7. `Feedback/Snackbar`, plus a **Warning** type added to `Feedback/Alert`.

**Consolidate (no new components):** merge `Search/Filter Chip` into `Chip Type=Filter`. Bind Bangla text styles in Button, Bottom Navigation, Chip and Badge. Add radius variables.

**Build when the flow is drawn (P1):** Dialog, Quantity Stepper, Bottom Sheet, Skeleton primitive, Order Status Tracker.

**Do not build now:** Tutorial Card and Workshop Card (no skill-sharing wireframes exist yet). Also skip a separate Verification Status component (Badge + Alert already covers it), a new Step Indicator (it exists), and a generic Icon Button (`App Bar/Action` covers current needs).

---

## 2. What exists today [Observed]

| Page | Component / set (node) | Variants / notes |
|---|---|---|
| 02.1 Buttons | `Button` (350:810) | 48 variants: Size S/M/L (40/48/56 px tall) × Type Primary/Secondary/Outline/Ghost/Destructive × State Default/Pressed/Disabled, plus Primary Loading. Text bound to `English/base-regular` and `English/sm-regular` (Inter). |
| 02.2 Input Fields | `Input Field` (339:500) | Size Small/Default/Large × State Default/Focus/Filled/Error/Success/Disabled (18) |
| 02.3 Checkbox & Radio | `Selection/Checkbox`, `Selection/Radio`, `Selection/Radio Check` | 24 px visual, 48 px touch target documented |
| 02.4 Search | `Search/Field` (494:98) | Default, Focused, Filled (clear ×), Results (back + clear), Disabled. 52 px tall |
|  | `Search/Filter Chip` (494:139) | Selected False/True. **Duplicates Chip Type=Filter** |
|  | `Search/Result Item` (494:142) | Single component, 163×261: square image, title (2 lines), seller, price. **This is a product card in all but name** |
|  | `Search/Feedback State` (494:210) | Type No Results / Loading / Error: icon circle, title, body, optional button |
|  | `Search/Recent Item`, `Search/Suggestion Item` | 52 px rows |
| 02.5 Avatar | `Avatar` (174:3422) | Size XS–XL × Content Icon/Initials/Image |
| 02.6 Toggle | `Selection/Toggle` | On/Off/Disabled |
| 02.7 Progress | `Progress/Step Indicator` (1–6 of 6), `Progress/Bar` (S/M/L × 1–6), `Progress/Strength Meter`, `Progress/Onboarding Dots` | Used on SE-04/SE-06 ("ধাপ ৪/৬") |
| 02.8 Chip | `Chip` (521:127) | Type Assist/Filter/Input/Suggestion × Selected × State Default/Pressed/Disabled. Documented as 32 px visual, 44 px touch, 16 px radius |
| 02.9 Badge | `Badge` (529:204) | Size S/M (24/28) × Style Subtle/Filled/Outline × Type Neutral/Primary/Success/Warning/Error/Info. Props: Label, Show Icon, Show Dot, Icon. Documented order labels: **Preparing, Packed, Shipped, Delivered, Cancelled**. Stock labels: Low Stock, Out of Stock |
| 02.10 App Bar | `App Bar` Root/Back (536:106), `App Bar/Action` Default/Pressed, 48×48 | |
| 02.11 Bottom Navigation | `Bottom Navigation` (539:64), single symbol, 80 px. `Bottom Navigation/Item` Default/Selected | Item labels bound to **`English/xs-medium` / `English/xs-bold` (Inter)** |
| 02.12 Tabs | `Tabs` (542:51), `Tabs/Item` Default/Selected | 48 px |
| 02.13 OTP | `Input/OTP Digit`, `Input/OTP Group` | |
| 02.14 Alerts & Feedback | `Feedback/Alert` (630:68) | Type **Error / Info / Success only**. No Warning |
| 02.15 Selection Cards | `Selection/Choice Card` | Selected False/True |
| 01.7 Iconography | `Icon Wrapper` (Size xs–3xl × 8 colors), 119 `Icon/*` | |
| 03.1 Onboarding WF | `WF/Status Bar`, `WF/Home Indicator`, `WF/Annotation`, `WF/Image Placeholder`, `WF/Journey Step`, `WF/Keyboard / Numeric`, `WF/Alert`, `WF/OTP Digit`, `WF/Choice Card` | `WF/OTP Digit` and `WF/Choice Card` were already promoted to library components. That precedent fits the plan below |
| 03.3 Seller Dashboard WF | `WF/Summary Tile` (675:33), `WF/Empty State` (675:37), `WF/Recent Order Row` (675:42), `WF/Link Row` (675:54) | Each is used on 3+ screens (SD-01 to SD-E02) |
| 03.2 Onboarding Hi-Fi | Local, non-component frames: "Product card" ×2 (END-01, 643:354), "Upload / …" ×3 (SE-04, 644:665), "Placeholder / Summary" (END-02), local "Status bar" and "Home indicator" frames | These are the hi-fi gaps |

**Foundations [Observed]:**
- **Spacing:** variables `space-4, 8, 12, 16, 20, 24, 32, 40, 48, 64, 80, 120`.
- **Radius:** no radius variables found. Radii are hard-coded per component (Badge 6/8, Chip 16, Checkbox 4).
- **Elevation:** `Elevation/1` for cards and chips, `Elevation/2` for sticky headers and bottom nav, `Elevation/3` for sheets and dialogs.
- **Typography:** `Bangla/*` styles now resolve to **Noto Sans Bengali**. However, the typography page note (508:280) still says "All 33 styles use Inter", which is out of date. Several components bind `English/*` (Inter) styles, so Bangla labels in those components render with a fallback font.
- **Icons:** the library has `minus`, `image-plus` and `bell-plus` but **no plain `plus`, `play`, `lock`, `wifi-off`, `alert-triangle`, `package` or `truck`**.

---

## 3. Audit table

Status key: **Exists**, **Needs Variant**, **New Component**, **Not Yet Needed**, **Cannot Verify**.

| Priority | Recommendation (brief) | Status | Evidence in Figma | Action |
|---|---|---|---|---|
| P0 | Summary Tile | **New Component** | `WF/Summary Tile` 675:33, used ×3 on SD-01, 01-S01, 02, 03, 04, 05, E01 and E02. SD-E02 shows an "unavailable" (—) state | Promote to `Card/Summary Tile` |
| P0 | Empty State | **Needs Variant** | `Search/Feedback State` (No Results/Loading/Error) has the right anatomy. `WF/Empty State` 675:37 is a compact dashed version used on SD-01/03/04/05 | Extend the existing set. Don't create a second empty-state component |
| P0 | Recent Order Row | **New Component** | `WF/Recent Order Row` 675:42 on SD-02 and SD-E01. Badge order statuses already exist | Promote to `List/Order Row`. Status comes from a nested Badge, not from variants |
| P0 | Link Row | **New Component** | `WF/Link Row` 675:54 on SD-01/02/03/04/05 | Promote to `List/Link Row` |
| P0 | Product Card | **Needs Variant** | `Search/Result Item` 494:142 (no variants). The hi-fi END-01 product cards are local frames with different anatomy (wide image, no seller) | Extend into `Card/Product`. Replace the hi-fi local frames |
| P0 | Search Field/Bar | **Exists** | `Search/Field` covers default, focused, filled+clear, results and disabled | None |
| P0 | Filter Chip | **Exists (duplicated)** | `Chip Type=Filter` (selected, unselected, pressed, disabled) **and** `Search/Filter Chip` (no disabled) | Keep `Chip`. Swap instances, then deprecate `Search/Filter Chip` |
| P0 | Image/File Upload Field | **New Component** | Hi-fi SE-04 uses three local "Upload / …" rows. The WF uses image placeholders. No uploading or failed state anywhere | Build `Input/Upload` |
| P0 | Order Status Tracker | **Not Yet Needed** (Badge covers lists) | Badge has Preparing/Packed/Shipped/Delivered/Cancelled. No order-detail wireframe exists. The vocabulary differs from the brief's "Placed" | Decide the status vocabulary now. Build the tracker with order detail (P1) |
| P0 | Confirmation Dialog | **New Component** (no screen uses one yet) | None found | Build a minimal version once the first destructive flow is drawn (remove product, cancel order, log out). Moved to P1 |
| P0 | Snackbar/Toast | **New Component** | None. Dashboard success messages use inline alerts | Build `Feedback/Snackbar` (needed for "added to cart" and "saved") |
| P1 | Quantity Stepper | **New Component** | None. **`Icon/plus` is missing** | Add `Icon/plus`, then build with the cart flow |
| P1 | Tabs | **Exists** | `Tabs` + `Tabs/Item` Default/Selected | No disabled variant needed in beta |
| P1 | Bottom Sheet | **New Component** | None | Build with mobile filters/sort |
| P1 | Loading Skeleton | **New Component** | SD-L01 draws raw rectangles named "Skeleton". Search uses a spinner state | Build a small `Feedback/Skeleton` primitive. Add `State=Loading` only to Product Card, Summary Tile and Order Row |
| P1 | Step/Progress Indicator | **Exists** | `Progress/Step Indicator`, `Progress/Bar` | No new component. The error state is handled by an Alert on the screen |
| P1 | Verification Status | **Exists as a pattern** | Badge "Verification level" + `Feedback/Alert` + Button on SD-01/03/04/05 and SE-05/06 | Document it as a pattern. Needs `Feedback/Alert Type=Warning` for "pending" and "payout locked" |
| P1 | Tutorial Card | **Not Yet Needed / Cannot Verify** | Only END-03 "Learn / Handoff" and SD-H09 placeholder | Wait for skill-sharing wireframes |
| P1 | Workshop Card | **Not Yet Needed / Cannot Verify** | Same | Wait |
| n/a | Warning alert | **Needs Variant** | `Feedback/Alert` has Error/Info/Success only. Badge already has Warning tokens | Add `Type=Warning` |

Some items are **Cannot Verify** without a deeper read. I did not open every component's property panel, so these need a check before building:
- the exact boolean and text properties on `Feedback/Alert` and `Search/Result Item`
- whether `Bottom Navigation` exposes a 5th-item boolean (SD notes say "Item 5 enabled")

---

## 4. Duplicates and mismatched conventions [Observed → Recommendation]

1. **`Search/Filter Chip` vs `Chip Type=Filter`.** These are two filter chips with different sizes (46 vs 44 px) and colors. Keep `Chip` and retire the Search one.
2. **`WF/Alert`, `WF/OTP Digit` and `WF/Choice Card`** mirror library components. It is fine to keep them wireframe-only, since they give the neutral wireframe look. Don't let them reach hi-fi.
3. **Hi-fi uses local frames** where components are needed: Product card, Upload rows, Status bar and Home indicator. Replace them with instances after the components exist. For the device chrome, either promote `WF/Status Bar` as `Device/Status Bar` or keep it local; I recommend promoting it, since it's cheap and every screen uses it.
4. **Bangla in components.** `Bottom Navigation/Item` binds `English/xs-*` (Inter) and `Button` binds `English/base-regular`. Bangla labels therefore render in a fallback font. The typography note 508:280 is also out of date.
5. **Naming is mixed:** slash-namespaced (`Search/Field`, `Feedback/Alert`, `Selection/*`, `Progress/*`) next to bare names (`Button`, `Badge`, `Chip`, `Tabs`, `App Bar`, `Input Field`). **Recommendation:** namespace all *new* components (`Card/`, `List/`, `Feedback/`, `Overlay/`, `Input/`) and leave existing names alone until a v1.1 migration.
6. **Heading scale is inverted** (H1 = 20 px, H6 = 64 px). It's already documented. Leave it, and don't add new headings on top of it.
7. **No radius variables.** Every new component would add another hard-coded radius. Add variables first.
8. **No `Button/Label` text style** (also documented). Do this together with the Bangla binding fix.

**Should stay wireframe-only:** `WF/Annotation`, `WF/Journey Step`, `WF/Image Placeholder`, `WF/Keyboard / Numeric`, `WF/Alert`, `WF/OTP Digit`, `WF/Choice Card`, and the SD-H handoff stand-ins.

**Should become master components:** the four SD `WF/` patterns (as reworked above), the hi-fi Product card and Upload row, and optionally the Status bar and Home indicator.

---

## 5. Component specifications (P0)

General rules: use Auto Layout everywhere. Components go **Fill** horizontally inside the 358 px content column (390 frame, 16 px gutters) and **Hug** vertically. Text uses `Bangla/*` styles by default; English screens swap to the size-matched `English/*` style, which avoids layout shift. Status is always shown with text plus an icon or badge, never color alone.

### 5.1 `Card/Summary Tile` (composite)
- **Purpose:** seller dashboard metrics (পণ্য, নতুন অর্ডার, আয়). Screens SD-01 to SD-E02.
- **Anatomy:** optional icon (Icon Wrapper sm 20), label, value, optional note.
- **Properties:**
  - `State` = Default | Loading | Unavailable
  - text: `Label`, `Value`, `Note`
  - boolean: `Show icon`, `Show note`
  - instance swap: `Icon`
  - That gives **3 variants**.
- **Tokens:**
  - padding `space-12`, gap `space-4`
  - fill `Color/Neutral/50`, border `Semantic/Border/Default` (as in the WF)
  - label `Bangla/এক্সট্রা স্মল-রেগুলার` 12/18
  - value `Bangla/লার্জ-বোল্ড` 20/32
  - radius 12 (proposed, as variable `radius-md`)
- **Layout:** three tiles in a horizontal Auto Layout row with gap `space-8`, each tile Fill (≈114 px at 390).
- **States:**
  - Loading replaces the value and note with Skeleton lines.
  - Unavailable shows "—" plus a note ("তথ্য নেই"), as on SD-E02.
- **Bangla/English:** label max 2 lines. Values use Bangla numerals and the "৳" prefix. Truncate large amounts with a unit ("৳১.২ লাখ") instead of wrapping.
- **Accessibility:** non-interactive by default. If the team makes tiles tappable (see decision D1), add a `Pressed` state and a 48 px minimum height. Screen readers should read "label, value".
- **Depends on:** Icon Wrapper, Skeleton (P1; until then use the WF rectangles).

### 5.2 Feedback State: extend `Search/Feedback State` (composite)
- **Purpose:** empty, no-results, loading and error states for products, orders and search.
- **Change:** rename the set to `Feedback/State`. Renaming in place keeps every existing instance linked. Then add a `Size` property:
  - `Size=Full` (existing): Type = No Results | Loading | Error, **+ Empty** (new)
  - `Size=Compact` (from `WF/Empty State`): Type = Empty | Error
  - That's **6 variants**, not 8. Loading at compact size uses Skeleton, not this component.
- **Anatomy:** icon (Icon Wrapper xl 32, inside the existing circle), title, description, optional Button (`Show action`, Medium Outline).
- **Compact:** 358 wide, dashed border as in the WF, padding `space-16`, centered.
- **Copy rule:** every empty state says what will appear and offers one next step ("প্রথম পণ্য যোগ করুন").

### 5.3 `List/Order Row` (composite)
- **Purpose:** recent orders on the seller dashboard. Reusable later for buyer and seller order lists.
- **Anatomy:**
  - optional thumbnail (48, image fill)
  - title: product × qty (`Bangla/স্মল-বোল্ড`)
  - meta: order ID · district · payment (`Bangla/এক্সট্রা স্মল-রেগুলার`)
  - trailing nested **Badge Small**
  - `Icon/chevron-right`
- **Properties:**
  - `State` = Default | Pressed | Loading
  - boolean: `Show thumbnail`, `Show badge`
  - The order status comes from the **nested Badge instance**, so there are no status variants.
  - That gives **3 variants**.
- **Size:** min height 64, padding `space-12`, gap `space-12`. Text column Fill with title on 1 line and ellipsis, meta up to 2 lines.
- **Accessibility:** the whole row is one tap target (≥48). Its accessible name includes the status text.

### 5.4 `List/Link Row` (primitive-level composite)
- **Purpose:** navigation entries (দক্ষতা শেয়ার করুন, profile menu, settings).
- **Properties:**
  - `State` = Default | Pressed | Disabled
  - boolean: `Show leading icon`, `Show subtitle`
  - instance swap: `Icon`
  - text: `Label`, `Subtitle`
  - That gives **3 variants**.
- **Size:** min height 48 (56 with subtitle, proposed), padding-x `space-16`, gap `space-12`. Icon lg 24, chevron lg 24. Label `Bangla/বেস-বোল্ড` 16/24 wraps; the chevron stays vertically centered.
- **Disabled:** dimmed, plus the reason in the subtitle (not color alone).

### 5.5 `Card/Product`: extend `Search/Result Item` (composite)
- **Purpose:** buyer grids (home, category, search) and seller inventory.
- **Change:** move `Search/Result Item` into a new `Card/Product` set as the Buyer/Available variant, which keeps its instances. Then add variants.
- **Properties:**
  - `Context` = Buyer | Seller
  - `Availability` = Available | Out of stock
  - `State` = Default | Loading
  - Use only the meaningful combinations: Buyer × {Available, Out of stock}, Seller × {Available, Out of stock}, and one Loading per context. That's **6 variants**.
  - boolean: `Show seller` (Buyer), `Show badge` (nested Badge such as Handmade / Made to Order)
  - text: `Title`, `Price`, `Seller`, `Stock` (Seller context)
- **Anatomy:** image (aspect ratio is decision D9; I propose 1:1, matching Search), badge overlay (optional), title (2 lines max), seller (1 line, Buyer), price (`Bangla/বেস-বোল্ড`, `Semantic/Text/Brand`), and stock count with a status Badge (Seller).
- **Out of stock:** image at reduced opacity plus Badge "Out of Stock". Price stays visible.
- **Layout:** two-column grid with `space-8` gap; the card is Fill (≈171 px at 390). Radius `radius-md` (proposed), `Elevation/1` or border per the hi-fi direction.
- **Test with:** long Bangla titles ("হাতে বোনা নকশিকাঁথা কুশন কভার — লাল ও সোনালি"), ৳ amounts up to "৳১২,৫০০", and a 360 px width.

### 5.6 `Input/Upload` (composite)
- **Purpose:** NID front/back and selfie (SE-04), product photos (Add product, when it's designed).
- **Anatomy:**
  - leading icon (`camera-01`)
  - label and helper/status text
  - thumbnail (48, when uploaded)
  - trailing action: chevron (Empty), progress % (Uploading), `edit-05` replace + `trash-01` remove (Uploaded), `refresh-ccw-05` retry (Error)
- **Properties:** `State` = Empty | Uploading | Uploaded | Error | Disabled (**5 variants**), plus text `Label` and `Helper`.
- **Visuals:**
  - Empty: dashed border (as hi-fi)
  - Uploading: `Progress/Bar` Small
  - Error: `Semantic` error border plus a message ("ছবি ঝাপসা — আবার তুলুন")
  - Size: min height 64, padding `space-16`
- **Later (not now):** a `Layout=Tile` square variant for a multi-photo product grid, once the Add-product wireframe exists.
- **Accessibility:** each action icon is ≥44×44 (48 recommended). Upload progress must be announced.

### 5.7 `Feedback/Snackbar` (composite), plus `Feedback/Alert` Warning
- **Snackbar:**
  - `Type` = Success | Error | Info (**3 variants**). Boolean `Show action`, text `Message` and `Action`.
  - Dark surface (proposed `Color/Neutral/900`), `Elevation/3`, radius `radius-md` (proposed).
  - Fill width with 16 px margins, sitting 8 px above the bottom nav. Message up to 2 lines.
  - Behavior (proposed): auto-dismiss after 4 s. Errors that have an action stay until the user acts or 8 s pass.
  - Accessibility: announce as a polite live region. Never put the only path to recovery in a snackbar.
- **Alert:** add `Type=Warning` using the existing Warning badge colors and `Icon Wrapper Color=Warning`, plus an icon. Use it for "যাচাই চলছে" (pending), "payout locked" and "offline".

---

## 6. Naming and variant structure

```text
Existing (unchanged)              New / extended
Button                            Card/Summary Tile        State
Input Field                       Card/Product             Context × Availability × State (6)
Search/Field                      List/Order Row           State
Chip  (absorbs Search/Filter Chip) List/Link Row           State
Badge                             Feedback/State  (was Search/Feedback State)  Size × Type (6)
Feedback/Alert  (+ Type=Warning)  Feedback/Snackbar        Type
App Bar, Bottom Navigation, Tabs  Input/Upload             State
Progress/*, Selection/*, Input/OTP *
P1: Feedback/Skeleton (Shape=Line|Block|Circle), Overlay/Dialog (Type=Default|Destructive),
    Overlay/Bottom Sheet, Input/Quantity Stepper (State=Default|Min|Max|Disabled), Order/Status Tracker
```

**Worked example: `Card/Summary Tile`**

```text
Card/Summary Tile                        ← component set
├─ State=Default
├─ State=Loading
└─ State=Unavailable

Component properties
  Label        (text)            "পণ্য"
  Value        (text)            "১২"
  Show note    (boolean)         off
  Note         (text)            "এই মাসে"
  Show icon    (boolean)         off
  Icon         (instance swap)   Icon/tag-01   ← preferred: Icon/*

Layer structure (Auto Layout, vertical, Hug height, Fill width)
  Tile  [padding space-12, gap space-4, radius radius-md*, fill Neutral/50, stroke Border/Default]
  ├─ Icon Wrapper sm      (bound to Show icon / Icon)
  ├─ Label                Bangla/এক্সট্রা স্মল-রেগুলার, Text/Secondary
  ├─ Value                Bangla/লার্জ-বোল্ড, Text/Primary   (Loading → Skeleton line)
  └─ Note                 Bangla/এক্সট্রা স্মল-রেগুলার      (bound to Show note)
* proposed variable
```

Why only 3 variants: content goes into text properties, and the icon and note are booleans. Making "Earnings" or "Orders" into variants would multiply the set without adding any behavior.

---

## 7. Decisions needing team approval

| # | Decision | Facts | My recommendation |
|---|---|---|---|
| D1 | **Seller bottom nav: 4 or 5 tabs** | END-02 (03.1/03.2) has Dashboard · Products · Orders · Profile. The SD page uses 5 (adds Earnings). The buyer hi-fi uses Home · Learn · Orders · Account | Keep **4 tabs** for consistency with existing screens and with the buyer app, which matters for low digital literacy. Reach Earnings from a **tappable earnings tile** and from Profile, which means the tile needs a `Pressed` state. Don't edit existing screens until you decide |
| D2 | **Bangla glyphs in components** | `Bottom Navigation/Item`, `Button` (and likely Chip/Badge) use Inter styles | Rebind component text to `Bangla/*` (Noto Sans Bengali) by default and update note 508:280. Add a Bangla nav-label style at 12/18 Medium (proposed; check conjunct clipping, and go to 13/20 if it clips) |
| D3 | **Touch targets** | Button Small is 40 px tall. Chip uses 32 visual + 44 hit. Checkbox has a 48 hit area | Designs are at 1× (390 frame), so 1 Figma px maps to 1 pt on iOS and 1 dp on Android. Set the rule as **≥48 hit area** (meets 44 pt and 48 dp). Use Button Medium (48) for in-card actions in hi-fi. Keep Small only where an invisible 48 hit area is specified |
| D4 | **Order status vocabulary** | Badge: Preparing/Packed/Shipped/Delivered/Cancelled. Brief: Placed/Packed/Shipped/Delivered | Choose one set: **Placed → Packed → Shipped → Delivered**, with Cancelled only if cancellation is in beta scope. Then update the Badge examples |
| D5 | **Earnings tile meaning** | SD shows "this month" | Label it exactly, e.g. "এই মাসের বিক্রি (৳)". Show payout availability separately on the Earnings screen |
| D6 | **One-time success alerts** | First product / NID approved | Show it once on the next dashboard visit, with a dismiss (×). Clear it after dismissal or on the second visit. Use a Snackbar for transient confirmations instead |
| D7 | **Shared session-expired / offline** | SD-E03 duplicates SY-03 | Use SY-03 for both. Offline uses `Feedback/Alert Type=Warning` (or Error) + Retry, kept as one shared pattern |
| D8 | **Skill-sharing entry** | Link row on SD-01/02 | Move it to Profile for beta. The dashboard stays focused on selling |
| D9 | **Product image ratio** | Search Result Item is square. Hi-fi END-01 cards are ~173×100 | Use **1:1**, which suits handmade products and phone photos |
| D10 | **Radius variables** | None exist | Add `radius-sm 8 / radius-md 12 / radius-lg 16 / radius-full` (proposed values; confirm against current Badge, Chip and Button) |
| D11 | **Missing icons** | No `plus`, `play`, `lock`, `wifi-off`, `alert-triangle`, `package` | Add `plus`, `lock`, `wifi-off` and `alert-triangle` now. Add `play` with the tutorials work |

---

## 8. Step-by-step Figma build checklist (after approval)

Estimates assume one design-system lead. Every step is additive or a rename-in-place, so existing instances stay linked.

**Phase 1: Consolidate primitives (~1 day)**
1. Create radius variables (D10) and the missing icons (D11) on 01.7.
2. Rebind Button, Bottom Navigation/Item, Chip and Badge labels to Bangla styles (D2). Update the typography note 508:280.
3. Add `Feedback/Alert Type=Warning`.
4. Swap `Search/Filter Chip` instances to `Chip Type=Filter`, then move the old set to 99 Archive.

**Phase 2: Promote the seller-dashboard patterns (~1 day)**
5. Build `Card/Summary Tile`, `List/Order Row` and `List/Link Row` on new pages 02.16 Cards and 02.17 Lists (or on existing pages, following your order).
6. Extend `Search/Feedback State` into `Feedback/State` (+Empty, +Compact).
7. Swap the instances on 03.3 so the `WF/` masters are no longer used, then archive them.

**Phase 3: Shared marketplace patterns (~1.5 days)**
8. Build `Card/Product` from `Search/Result Item` and replace the hi-fi END-01 local cards.
9. Build `Input/Upload` and replace the SE-04 hi-fi local rows.
10. Build `Feedback/Snackbar`.

**Phase 4: Feedback, loading, overlays (P1, when flows are drawn)**
11. `Feedback/Skeleton`, `Overlay/Dialog`, `Overlay/Bottom Sheet`, `Input/Quantity Stepper`, `Order/Status Tracker`.

**Phase 5: Skill-sharing (when wireframes exist)**
12. `Card/Tutorial` (Free/Paid, Loading) and `Card/Workshop` (Available/Full/Booked, Loading).

**Phase 6: Stress test (half a day, after each phase)**
13. Use a playground frame per component to test:
    - long Bangla product names
    - ৳ amounts in Bangla numerals
    - English swap
    - 360 px and 390 px widths
    - text at 130% size (simulated by stepping up one style)
    - 48 px hit areas
    - status readable without color

**Out of scope for this plan:** admin, disputes, live tracking, mentoring scheduling, web layouts.
