# HTML and CSS review

This review preserves the existing content, Paper Mono font, sage palette, and automatic dark theme.
The site still uses static HTML, Jekyll, and one stylesheet.
It adds no client JavaScript or runtime dependencies.

## Defensive CSS references

Reference: [Defensive CSS tips by Ahmad Shadeed](https://defensivecss.dev/tips/).

Apply the tips that match the site's existing elements:

- [Flexbox wrapping](https://defensivecss.dev/tip/flexbox-wrapping/): let the header, navigation, and contact links wrap.
- [Minimum content size](https://defensivecss.dev/tip/flexbox-min-content-size/): let text items shrink with `min-inline-size: 0`.
- [Component spacing](https://defensivecss.dev/tip/spacing/): use explicit gaps between links and header items.
- [Using space-between](https://defensivecss.dev/tip/space-between/): retain the two-sided header, but give its items a minimum gap.
- [Fixed sizes](https://defensivecss.dev/tip/fixed-sizes/): use fluid widths and minimum link heights, not fixed text container heights.
- [Image maximum width](https://defensivecss.dev/tip/img-max-width/): constrain every image and preserve its aspect ratio.
- [Scrollbar gutter](https://defensivecss.dev/tip/scrollbar-gutter/): reserve scrollbar space to keep the content position stable.
- [Accidental hover on mobile](https://defensivecss.dev/tip/hover-media/): restrict hover effects to devices that support hover.

The site wraps long text rather than truncating titles or contact labels.
The [long content tip](https://defensivecss.dev/tip/long-content/) permits truncation when the hidden text does not matter.
That trade-off does not fit this site's headings or contact links.

The site has no forms, grids, dialogs, or sticky panels.
Do not add rules for those components until the site needs them.
All color variables have values in `:root`, so duplicate variable fallbacks add no benefit here.

## HTML and accessibility

- Add a keyboard skip link to the homepage, Jekyll layout, and error page.
- Make the main content a focus target for the skip link.
- Mark the blog navigation link as the current page on the blog index.
- Retain list semantics with `role="list"` where CSS removes list markers.
- Restore native text selection in the header and footer.
- Darken muted text to exceed the normal-text contrast requirement in the light theme.
- Use relative units, fluid headings, explicit spacing, and logical CSS properties.
- Keep navigation and contact links at least `2.75rem` high.

## Images and font

Both article images measure 1200 by 640 pixels.
Their Markdown attribute lists now provide those dimensions to reserve space before the images load.
The responsive CSS still reduces their displayed size on narrow screens.
The second image uses native lazy loading.
The browser may still request it early because it lies near the viewport.

Paper Mono remains unchanged at 53,276 bytes, approximately 52 KiB.
The existing WOFF2 variable font supplies all requested weights in one request.
The existing `font-display: swap` rule lets the browser show fallback text while the font loads.

A font preload increased first-paint time in an initial local test.
The final HTML therefore does not preload the font.
Do not add a preload without measuring its effect on the stylesheet and first paint.

## Reset comparison

The review consulted these open-source stylesheets:

- [modern-normalize](https://github.com/sindresorhus/modern-normalize/blob/main/modern-normalize.css): a small reset for browser consistency.
- [sanitize.css](https://github.com/csstools/sanitize.css/blob/main/sanitize.css): broader defaults for content, controls, and accessibility.

A full reset would add rules for elements this site does not use.
The final stylesheet keeps local rules for the existing HTML instead.
It preserves universal border-box sizing and controls text size after mobile orientation changes.
It keeps Safari's prefixed text-size property alongside the standard property.

## Code checks

The checks used the generated homepage, blog index, demo article, and error page.
They did not use screenshots or visual judgments for acceptance.

- Jekyll builds successfully.
- `git diff --check` passes.
- Stylelint's standard configuration reports no errors.
- HTML Validate's recommended configuration reports no errors with the renderer exceptions below.
- 56 layout checks pass across four pages, two themes, and seven viewport widths.
- The viewport widths are 240, 320, 375, 420, 768, 1280, and 1920 CSS pixels.
- No tested element extends outside the viewport.
- 16 axe-core checks report no violations across both themes at 375 and 1280 pixels.
- Four keyboard checks confirm that the first Tab reaches the skip link and Enter focuses the main content.
- Additional checks cover 200% text, short viewports, long headings, long labels, unavailable fonts, and unavailable images.
- The images retain their declared aspect ratio when requests fail.
- The browser reports no script errors.

HTML Validate exceptions:

- Allow explicit list roles because Safari can lose list semantics when CSS removes list markers.
- Allow Kramdown's self-closing image syntax.
- Ignore whitespace that Liquid emits in generated HTML.

Automated checks do not prove complete accessibility or visual quality.
The checks do not include a manual screen-reader test or a production network test.

## Local loading measurements

The test compares the original commit with the changed site through equivalent local static servers.
It uses Chromium at 375 by 812 pixels with a cold cache.
It simulates 150 milliseconds of network latency and 1.6 Mbps download bandwidth.
Each result gives the median of three samples.

| Page | Version | First paint | Largest content paint | Layout shift |
| --- | --- | ---: | ---: | ---: |
| Homepage | Original | 380 ms | 380 ms | 0 |
| Homepage | Changed | 380 ms | 380 ms | 0 |
| Demo article | Original | 416 ms | 452 ms | 0 |
| Demo article | Changed | 452 ms | 468 ms | 0 |

These samples do not establish an article speed improvement.
They also do not predict production performance.
The dimensions protect image layout space even though the ordinary loading samples show zero layout shift for both versions.

The stylesheet grows from 3,887 bytes to 5,433 bytes before compression.
The site adds no font or JavaScript requests.
The font request takes about 420 milliseconds in this simulated network test.
That request duration does not represent a delay before visitors can read fallback text.

The local test scripts and detailed results remain in `/tmp/greyfd-css-review/` for this session.
Jekyll excludes this `_notes` directory from the generated site.
