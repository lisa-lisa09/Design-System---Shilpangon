# শিল্পাঙ্গন (Shilpangon) logo: woven thread

Refined woven-thread logo system for the Shilpangon mobile marketplace. Primary brand color is **#B8342B** (`Color/Brand/Primary/500`).

- **Editable masters:** Figma file [Design System v1](https://www.figma.com/design/vgzcpBGmqSlC2d9qIL6JZK/Design-System-v1?node-id=4-161), **Logo** page (sections 01–10, components in section 06).
- **This folder:** exported handoff files generated from the same geometry as the Figma masters.

## What changed from the previous version

| | Before | Now |
|---|---|---|
| Color | #C9452C / #E35336 (orange-red) | #B8342B only, 100% opacity |
| Crossings | white strokes painted over the thread | true cut-outs (transparent) |
| Seams | hairlines where the over-strands joined | removed (over-strands overlap and merge) |
| Thread | 13 / 200 units, 5-unit gaps | 14 / 200 units, 6-unit gaps |
| Small sizes | "bold" stroke variant | documented Small optical version (thread 20, gaps 9) for 24–31 px |
| Wordmark | live text only | outlined vectors + live-text master in Figma |
| Ground | cream #FBF5EC baked into previews | none; place on white or an approved light surface |

## Asset manifest

| File | Size | Format / color | Transparency | Use |
|---|---|---|---|---|
| `svg/logo-horizontal-{primary,reverse,mono}.svg` | viewBox 421.84 × 120 | SVG, sRGB hex | transparent | In-app headers, onboarding |
| `svg/logo-stacked-{primary,reverse,mono}.svg` | viewBox 180 × 201.31 | SVG | transparent | Splash, square spaces |
| `svg/symbol-{primary,reverse,mono}.svg` | viewBox 203.33 square | SVG | transparent | Symbol at 32 px and up |
| `svg/symbol-small-{primary,reverse,mono}.svg` | viewBox 213.33 square | SVG | transparent | Symbol at 24–31 px |
| `svg/wordmark-{primary,reverse,mono}.svg` | viewBox 340.56 × 116 | SVG, outlined | transparent | Standalone name |
| `png/logo-horizontal-{primary,reverse}_160w@{1,2,3}x.png` | 160/320/480 × 46/92/138 | PNG-32 RGBA, sRGB | transparent | In-app raster fallback |
| `png/logo-stacked-primary_120w@{1,2,3}x.png` | 120/240/360 wide | PNG-32 RGBA | transparent | In-app raster fallback |
| `png/logo-stacked-reverse_160w@{1,2,3}x.png` | 160/320/480 wide | PNG-32 RGBA | transparent | Splash on #B8342B |
| `png/symbol-{primary,reverse}_48@{1,2,3}x.png` | 48/96/144 square | PNG-32 RGBA | transparent | In-app symbol |
| `png/symbol-small-primary_24@{1,2,3}x.png` | 24/48/72 square | PNG-32 RGBA | transparent | App bar / compact |
| `app-icon/app-icon-play-512.png` | 512 × 512 px | PNG-32 RGBA, sRGB ICC, 17 KB | opaque | Google Play listing icon |
| `app-icon/app-icon-full-color.svg` | 108 × 108 (dp) | SVG | opaque | Launcher master / review |
| `app-icon/app-icon-foreground.svg` | 108 × 108 (dp) | SVG | transparent | Adaptive foreground layer |
| `app-icon/app-icon-background.svg` | 108 × 108 (dp) | SVG | opaque | Adaptive background layer |
| `app-icon/app-icon-monochrome.svg` | 108 × 108 (dp) | SVG | transparent | Themed-icon monochrome layer |
| `android/drawable/ic_launcher_{foreground,background,monochrome}.xml` | 108 dp viewport | VectorDrawable (**draft**) | — | Android resources |
| `android/mipmap-anydpi-v26/ic_launcher.xml` | — | adaptive-icon XML (**draft**) | — | Android resources |
| `source/*.json` | — | path data | — | Geometry used to generate everything above |

Colors: primary `#B8342B` (Color/Brand/Primary/500); reverse `#FFFFFF` (Semantic/Icon/Inverse); mono `#1B1B1B` (Semantic/Text/Primary). White and mono are functional exceptions, not brand-ramp colors. The app-icon monochrome layer is filled black because Android ignores its color and tints it.

## Brand rules (recommendations, not platform requirements)

- **Clear space:** 1 L on all sides. L = one loop (crossing point to loop tip) = ⅓ of symbol height. Example: horizontal at 160 px wide → L ≈ 15 px.
- **Minimum sizes** (from actual-size tests at 1× and 3×): Regular symbol 32 px or larger. Small optical symbol 24–31 px. Do not use the symbol below 24 px (at 16 and 20 px the crossings fill in). Horizontal lockup at least 120 px wide (at 96 px the ল্প conjunct crowds). Stacked lockup at least 80 px wide.
- Don't stretch, recolor (no orange), place on low-contrast or busy backgrounds, crowd, crop, or use below minimum size.

## Where to place the files

- **Figma:** components are already on the Logo page. To re-import any SVG, drag it onto the canvas and bind its fill to `Color/Brand/Primary/500` (or the matching neutral style).
- **Android app:** copy `android/drawable/*.xml` to `app/src/main/res/drawable/` and `android/mipmap-anydpi-v26/ic_launcher.xml` to `app/src/main/res/mipmap-anydpi-v26/`. In-app PNGs go in `drawable-mdpi`/`-xhdpi`/`-xxhdpi` (1×/2×/3×), or use the SVGs through Android Studio's Vector Asset import.
- **Google Play Console:** upload `app-icon/app-icon-play-512.png` as the app icon.

## Still manual / not verified

- The Android XML files are **drafts**: they were generated from the master geometry and checked visually, but they have **not** been built or tested in Android Studio. The SVG layers are design handoff assets, not a finished adaptive-icon resource.
- Legacy launcher PNG mipmaps (pre-API 26) are not included. Generate them with Android Studio's Image Asset Studio.
- iOS is not delivered. It needs its own icon source and Xcode asset setup per Apple's current guidance.
- **Font:** Baloo Da 2 SemiBold by Ek Type, licensed under the SIL Open Font License 1.1 (per the `google/fonts` repository, `ofl/balooda2/OFL.txt`). The wordmark is set in this font, not custom lettering. The trademark position has not been reviewed.

## References

- Android adaptive icons: https://developer.android.com/develop/ui/compose/system/icon_design_adaptive
- Google Play icon specifications: https://developer.android.com/distribute/google-play/resources/icon-design-specifications
- Apple app icons (future): https://developer.apple.com/design/human-interface-guidelines/app-icons

Color rationale (English and Bangla): see [`color-rationale.md`](color-rationale.md).
