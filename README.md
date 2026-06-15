# chart-v1
TRADING4SIGHT is a Trading Chart Platform

---
# Changelog

## 2026-06-15

### Volume Cluster
- Updated the default Volume Cluster marker colors to `#00E5FF` for bull markers and `#FFB300` for bear markers through the existing customizable color settings.

## 2026-06-14

### Volume Cluster
- Fixed Volume Cluster marker drift on X-axis zoom and scroll by including the chart's raw visible-range signature in the overlay render cache key, so cached figures cannot be reused across different horizontal scales.

## 2026-06-13

### Volume Cluster
- Added an optional `History Range` setting under `Volume source` so the overlay can limit how much 5-second source history is processed, with timestamp cutoff filtering that leaves the existing lookback, marker, basis-ratio, cluster-zone, and rendering logic unchanged.
- Fixed Volume Cluster marker alignment on price-scale zoom by including the current price-scale fingerprint in the overlay render cache key, so markers are recalculated instead of reusing stale Y positions.

## 2026-06-12

### Volume Cluster
- Reduced Volume Cluster hover work by skipping tooltip hit-map creation when value display is off, avoiding repeated same-pixel lookups, and stopping the tooltip overlay scan as soon as a marker hit is found.
- Hardened Volume Cluster settings normalization so malformed stored values cannot produce `NaN` thresholds/windows or non-boolean toggles, and aligned tooltip border color with the configured marker colors.
- Reworked the Volume Cluster settings panel to follow the TPO-style Inputs/Style tabs with Defaults, Cancel, and Ok actions, keeping edits local until Ok so typing settings no longer repeatedly recalculates the overlay.
- Replaced Volume Cluster marker color text inputs with the shared TPO-style color swatches, palette popover, opacity control, and custom color editor while preserving draft-until-Ok settings behavior.
- Added a chart context-menu fallback for Volume Cluster so right-clicking the chart while the overlay is applied exposes the `Volume Cluster Settings` action even though the rendered cluster figures ignore pointer events.
- Fixed the Volume Cluster Style tab so marker color palettes have enough vertical room and no longer get clipped by the modal body/footer.
- Restored scrolling inside the Volume Cluster Style tab so the fixed-height modal body can reach lower fields instead of expanding past the panel when a palette is open.
- Removed the extra fixed minimum height from the Volume Cluster Style tab body so the panel no longer leaves a blank gap under its fields.
- Updated Volume Cluster default settings so `Basis Ratio filter` now starts enabled and `Cluster zones` starts disabled.
- Updated the default Volume Cluster marker colors to `#1DE9B6` for bull and `#FF8A80` for bear through the shared theme tokens.
- Expanded the Volume Cluster overlay summary to include the active volume source, threshold mode, and minimum basis ratio when the Basis Ratio filter is enabled.
- Shortened the Volume Cluster summary labels to `Vol src`, `Mode`, and `Min BR` for a tighter status line.
- Guarded Volume Cluster's async 5s source loaders with per-overlay request versions so stale symbol or settings responses can no longer overwrite newer overlay state.
- Cleared the Volume Cluster request-version cache when an overlay is removed and when the chart is destroyed so stale cleanup data does not linger across sessions.
- Cached the active Volume Cluster tooltip hit in the chart manager so mouse movement inside the same hit radius reuses the current tooltip instead of rescanning all overlays every frame.
- Tightened the Volume Cluster tooltip reuse path by dropping the rounded lookup-key short-circuit and reducing the reuse radius so crosshair movement releases from marker hover more promptly.
- Removed the duplicate TPO summary update from the chart-stage `pointermove` handler so cursor movement no longer pays for two crosshair-side updates on every mouse move.
- Raised the Volume Cluster default `Minimum Basis Ratio (%)` to `10` and cached full overlay renders so the summary and figures reuse the same output until the data, visible range, or settings actually change.
- Made the Volume Cluster summary text settings-driven by removing bar/event/zone counts and showing only the configured source and toggles.
- Included chart height in the Volume Cluster render cache key so resize-driven pixel coordinates cannot be reused incorrectly.

## 2026-06-11

### Volume Cluster
- Added the Volume Cluster overlay tool with 5-second OHLCV source loading, manual/auto/hybrid threshold detection, Basis Ratio filtering, high/ultra event markers, and internal cluster-generated bull/bear zones.
- Added the Volume Cluster settings panel and wired the toolbar/context settings flow for threshold, lot size, Basis Ratio, cluster, display-value, and marker-color controls.
- Restyled the Volume Cluster settings panel to use the shared Trading4sight modal input/select/toggle controls and aligned paired toggles/color rows with the existing Volume Profile settings panels.
- Replaced always-on Volume Cluster value labels with an `Off` / `Tooltip` value display mode, keeping the chart clean while showing event details near the crosshair.
- Optimized Volume Cluster redraws by caching cluster results per source/settings key, skipping percentile sorting in manual threshold mode, and letting tooltip movement reuse the cached cluster data.
- Optimized Volume Cluster rendering so cached events and zones are filtered to the visible timestamp range before pixel conversion, with tooltip hit-testing limited to the same visible subset.
- Moved Volume Cluster tooltip rendering out of the overlay canvas into a throttled DOM tooltip driven by cached visible event snapshots, preventing crosshair movement from rebuilding overlay figures.
- Added a `Volume source` setting to Volume Cluster so alternate local symbols can supply matched 5-second volume while event placement, direction, zones, and prices stay anchored to the active chart candles.
- Hardened Volume Cluster result and alternate-volume merge caching with compact data signatures so redraws reuse computed clusters even when chart data is exposed through fresh array wrappers.

## 2026-06-09

### Volume Profile
- Added `Volume source` settings to Session Volume Profile and wired selected local symbols through chart, settings, and source-bar resolution, including `Chart TF` calculations with a non-chart volume source.
- Corrected `Volume source` logic for Fixed Range Volume Profile, Session Volume Profile, and Fixed Range TPO's embedded Volume Profile so alternate symbols provide only timestamp-aligned volume while OHLC/price distribution stays on the chart/source symbol candles.

### TPO
- Added `Volume source` back to the session Time price opportunity Volume Profile settings while keeping `Calculation method` hidden and fixed to `Chart TF` for session TPO.

## 2026-06-08

### Volume Profile
- Replaced the `Ultimate LTF 1S-5S` calculation-method option with explicit lower-timeframe choices below the active chart timeframe for Fixed Range and Session Volume Profile, and renamed Session Volume Profile's `TV Session Table` option to `Free 5k bars`.

### TPO
- Wired `Calculation method` and `Volume source` on the `Volume profile` tab for session `Time price opportunity` and `Fixed range TPVO`, keeping the alternate source limited to embedded Volume Profile rows while TPO letters continue to use their existing source bars.
- Removed `Calculation method` and `Volume source` from the session `Time price opportunity` Volume Profile tab while keeping them available for `Fixed range TPVO`.

### Chart navigation
- Fixed `Go to date` for deep historical jumps by loading a chart data window around the requested timestamp before focusing the candle, preventing year-back selections from snapping to the nearest currently paged-in bar.

### UI
- Renamed the left toolbar `Volume footprint` tool label to `Volume Cluster`.

## 2026-06-04

### KlineCharts beta3 compatibility
- Switched the Brush tool back to KlineCharts' built-in continuous `brush` overlay by no longer registering Trading4sight's custom brush template or custom pointer-capture stroke path.
- Kept completed Brush strokes from being treated as active overlay drafts when switching back to Cursor, so turning off the Brush tool no longer deletes existing brush drawings.
- Kept the Brush drawing tool active after completing a stroke so beta3 continuous brush drawing can place repeated strokes until the user chooses Cursor or cancels the tool.
- Restored Ctrl-held strong magnet drawing after the KlineCharts v10.0.0-beta3 hotkey/event changes by syncing the temporary strong-magnet mode from capture-phase keyboard and pointer modifier events before overlay points are converted.
- Updated the custom brush overlay template to opt into KlineCharts v10.0.0-beta3 continuous drawing mode while preserving the existing Trading4sight brush settings and stroke rendering.

### UI
- Restored Brush background fill by re-registering Trading4sight's continuous brush overlay renderer while keeping beta3 freehand drawing behavior.
- Neutralized the Brush line-end setting buttons so their endpoint icons no longer inherit the active stroke color/focus styling.
- Restored Brush drawing by adding the missing built-in brush style mapper and making continuous brush completion re-arm safely after each stroke.
- Rebuilt the Brush settings panel around the TradingView-style Line and Background rows, using the shared color swatcher palette, opacity controls, and thickness/style selectors.
- Reworked the Brush settings modal into a compact TradingView-style panel with a title edit affordance, minimal line/background controls, Template, Cancel, and Ok actions.
- Wired the Brush line and background buttons to the shared color swatcher popover with palette colors, custom color input, opacity, and line thickness controls.
- Fixed the Brush color swatcher stacking so the palette popover is no longer clipped by the compact modal body or hidden behind the footer.

### CSV loading
- Optimized local CSV folder imports by parsing symbol, fileCode, and exchange directly from filename suffixes (SYMBOL-FILECODE-EXCHANGE.csv) from right-to-left, fixing import failures when directories are not named exactly after the symbol.
- Streamlined folder import memory footprint by parsing and writing CSV files to IndexedDB sequentially instead of buffering all parsed arrays in memory, preventing browser out-of-memory crashes on large folders.
- Removed unused helper functions `saveBarsRecords` and `mergeImportedSymbols` to comply with strict TypeScript unused variable/local compilation rules.

## 2026-06-03

### KlineCharts beta2 compatibility
- Updated the chart's indicator creation calls for `klinecharts@10.0.0-beta2` so new indicators now use the beta2 `createIndicator(value, { isStack, pane })` options shape instead of the removed beta1 positional arguments.
- Removed stale `paneId` fields from indicator override payloads and added the required default `indicator.texts` style block, restoring TypeScript/build compatibility with the beta2 indicator API and style typings.

### CSV loading
- Added folder-import progress reporting for CSV uploads, including current read/parse/save phases, processed file counts, skipped file counts, and a determinate progress bar in the Symbol Search modal.
- Preserved existing imported-symbol metadata when importing additional CSV folders after the IndexedDB metadata-store migration, preventing older localStorage-backed imports from being dropped from the symbol list.
- Added a lightweight IndexedDB metadata store for imported CSV symbols so symbol/timeframe discovery no longer needs to read every stored bar array after folder imports or metadata cache misses.
- Verified the KlineCharts beta2 `getBars` paging direction semantics and normalized the loader's `more.forward/backward` flags to keep older-left and newer-right pagination explicit.
- Optimized the offline CSV chart loader to use the beta2 `setDataLoader.getBars(...)` pagination contract, so chart initialization now feeds a recent window into KlineCharts and loads older/newer slices on demand instead of pushing the full cached dataset at once.
- Tightened CSV parsing to avoid unnecessary sorting work when files are already in ascending timestamp order, while still deduplicating safely and preserving full cached bar arrays for replay, `Go To Date`, TPO, FRVP, and other full-data consumers.
- Moved CSV parsing for both runtime chart loads and imported CSV folders into a shared `Web Worker` path with a synchronous fallback, reducing first-load UI blocking while keeping the existing full-data cache and imported-symbol persistence behavior intact.

## 2026-05-25

### Volume Profile
- Updated the shared `Ultimate LTF 1S-5S` source fallback sequence so `secs_tf` calculations now try `1S` before `5S`, `10S`, `15S`, `30S`, and `45S`, bringing the fixed-range and session volume-profile source resolver back in line with the product naming when `1S` data is available.

## 2026-05-23

### TPO
- Corrected the session `TPO` summary row descriptor so `YSTC Auto` now renders with its full active mode label beside the hovered session label and row-size value, matching the intended changelog behavior instead of abbreviating the mode to just `YSTC`.
- Fixed the session `TPO` hover-summary resolver again so the active `Row` session now follows the live cursor column as well as the crosshair timestamp, preventing the summary from staying pinned to the previous session while moving across the chart.
- Reworked `TPO` single-print detection to follow TradingView's sequence-start behavior more closely: single prints now resolve from the first single-print row created when a newer letter extends above or below the previous letter's range, fixing misplaced bearish placement while preserving valid top/bottom sequence starts like `H -> I` and `K -> L`.
- Cleaned up `Fixed range TPVO` row-size handling so fixed-range `YSTC Auto` now behaves like range-based `Auto`, the fixed-range modal no longer exposes the session-only `YSTC Auto` option, and the `Ticks Per Row` / `Row Size Price` preview now syncs from the selected fixed-range drawing instead of reusing session-oriented copy and preview behavior.
- Updated the `Fixed range TPVO` row-size selector again so the fixed-range modal now offers only `YSTC Auto` and `Manual`, while keeping the fixed-range-aware selected-range row-size preview and calculation path behind `YSTC Auto`.
- Added `YSTC Auto 2x` to both session `TPO` and `Fixed range TPVO`, wiring it to use the same `YSTC Auto` calculation path and then double the resulting ticks-per-row value in the builder, preview, modal copy, and summary label text.

### custom crosshair bottom cursor label
- Added a custom crosshair datetime formatter so the bottom cursor label now shows TradingView-style strings like `Fri 23 May '26  21:52` or `Fri 23 May '26  21:52:10` based on the active period instead of the default KlineCharts numeric date format.

## 2026-05-21

### TPO
- Changed session `TPO` `YSTC Auto` row sizing so each generated profile now calculates its own `ticks per row` from that profile's own high-low range, instead of reusing one shared value taken from only the current session preview.
- Updated the session `TPO` row-size UI copy so the `YSTC Auto` preview clearly refers to the current session preview value while the rendered session profiles each use their own session-derived row size.
- Fixed the session `TPO` summary row-size display so hovering the chart now resolves the `Row` value from the session profile under the current crosshair timestamp instead of always showing the last rendered profile's row size.
- Tightened the session `TPO` hover-summary resolver again so the overlay now stores the exact hovered profile key alongside the crosshair timestamp, avoiding cases where timestamp-only matching could leave the summary pinned to the wrong session row size.
- Made the session `TPO` summary row explicit by showing the active row-size mode plus the hovered session label next to the row-size value, so repeated values like `10.00` can still be verified against the session currently driving the summary.

## 2026-05-15

### UI
- Tightened the TradingView-style session `TPO` layout by capping the letter region against the right-side `VP` band and widening the usable TPO area so narrow sessions keep letters inside their intended footprint without feeling overly compressed.
- Kept auto session `TPO` and `Session Volume Profile` on the hidden-anchor `totalStep: 1` model while preserving normal chart pan behavior, avoiding the unstable point-free interaction experiments from earlier testing.
- Disabled figure-level event capture for auto session `TPO` and `Session Volume Profile` so these chart-generated profiles no longer steal clicks from manual drawing tools or block normal drag-to-pan interaction.
- Hardened session `TPO` source-timeframe resolution so async source swaps now fall back cleanly to chart-timeframe rendering instead of briefly showing a profile and then leaving it blank.
- Restored session `TPO` right-click profile actions by keeping the context-menu session resolver aligned with the same chart-bar fallback used by visible TPO rendering, and by retaining click-position fallbacks when KlineCharts omits a usable figure key or timestamp.
- Preserved the current chart viewport while adding, removing, refreshing, hiding, or reconfiguring indicators so indicator pane changes no longer jump the chart back to a different location.
- Added a derived `Row Size Price` input to the session `TPO` settings beneath `Tick size`, keeping it synced with `ticks per row × tick size` in auto mode and letting manual mode update `Ticks Per Row` through a direct price-based row-size entry.
- Updated the session `TPO` summary line below OHLCV to append the derived row-size price after volume and tint the full line with the bearish red token for stronger visibility.
- Clarified session `TPO` row-size behavior by exposing a new `YSTC Auto` option beside `Auto` and `Manual`; `Auto` continues using the latest 300 bars near the rightmost visible bar, while `YSTC Auto` now calculates row size from the current session bars up to that same rightmost visible bar.

## 2026-05-14

### TPO overlay fixes
- Fixed shared TPO POC selection for both session `TPO` and fixed-range `TPVO` so tied maximum-letter rows now resolve to the row closest to the profile midpoint instead of always picking the highest price row.
- Fixed edge-clipped auto session `TPO` and `Session Volume Profile` rendering so first and last visible sessions now keep their full logical width like partially visible candles, instead of being squeezed to the chart border.
- Fixed session `TPO` and `Session Volume Profile` chart panning so their transparent profile-area rectangles no longer capture drag gestures that should scroll the chart left and right.
- Locked auto session `TPO` and `Session Volume Profile` overlays at creation so KlineCharts no longer treats them like draggable drawings, restoring normal left/right chart panning with the regular cursor behavior over those overlays.
- Reworked auto session `TPO` and `Session Volume Profile` overlays into point-free overlays with `totalStep: 0`, removing the hidden anchor-point drawing behavior that kept showing a hand cursor and blocking drag-to-pan.
- Added explicit `Tick size` support to session `TPO` and fixed-range `TPVO`, wiring the modal setting into row-size and auto ticks-per-row calculations so both overlays can use a real configured price increment instead of relying only on inferred OHLC spacing.

## 2026-05-13

### UI
- Added TradingView-style session TPO context-menu actions below the main overlay settings entry: `Split profile at this letter`, `Merge with previous profile`, and `Reset all merges and splits`, shown only when right-clicking the session `TPO` overlay.
- Wired session TPO profile structure state into the shared TPO builder so manual right-click split and merge actions persist on the overlay through redraws while leaving fixed-range TPO behavior unchanged.
- Fixed session TPO `Reset all merges and splits` so structure-clearing actions now invalidate cached TPO profile builds before the overlay redraws, preventing stale merged or split layouts from surviving the reset path.
- Fixed point-free session profile overlays so `TPO` and `Session volume profile` are now created with an immediate anchor point and are treated as complete when `totalStep` requires no user anchors, preventing hover-only disappearance near the time axis and keeping them in drawing history/state flows.
- Hardened session TPO `Reset all merges and splits` again by recreating the affected `tpoProfile` overlay instances with cleared `profileStructure` instead of only overriding their extend data, ensuring stale merged/split overlay state cannot survive inside the existing overlay instance.
- Implemented a TradingView-style Initial Balance Range marker for TPO by replacing the old faint dashed cue with a solid left-edge IBR vertical line plus top and bottom caps based on the configured opening block range.
- Increased the default TPO single-print highlight opacity from `15%` to `60%` in the shared TPO settings so both session `TPO` and `Fixed range TPVO` start with a stronger single-print emphasis.
- Corrected TPO single-print detection so consecutive single-print runs that touch the profile high or low are no longer highlighted, matching the intended non-extreme single-print behavior more closely.
- Added session TPO double-click expansion so double-clicking a session TPO profile now resolves the target session from the clicked block, session hitbox, or chart timestamp and toggles that profile between normal composite rendering and expanded block rendering without changing the global `Expand blocks` setting.
- Fixed session TPO double-click expansion again by resolving the clicked profile from overlay `x/y` chart coordinates when KlineCharts does not include a direct timestamp on the double-click overlay event, improving session detection across the profile area.
- Reworked session TPO double-click expansion to use a chart-stage `dblclick` fallback with explicit hit-testing against the rendered TPO session bounds, so expanding a session no longer depends on KlineCharts overlay figure double-click delivery.
- Reworked session TPO double-click expansion again to reuse the same timestamp-column session resolution as the working `Split profile at this letter` and `Merge with previous profile` actions, so double-click now targets the session under the current candle column instead of relying on rendered-profile hit boxes.
- Reworked session TPO expansion again to detect the second left-click from the existing chart-stage `pointerdown` path instead of relying on native `dblclick` delivery, while still resolving the target session from the current candle column like the working TPO context-menu actions.
- Added a session TPO right-click action that shows `Expand profile` or `Collapse profile` for the currently targeted session and reuses the same session-resolution context as the existing `Split profile at this letter` and `Merge with previous profile` menu actions.
- Fixed session TPO expand/collapse rendering so the overlay now passes `expandedProfileKey` through to the shared TPO renderer; previously the menu action could toggle the session state without any visible layout change because the renderer only received normalized base settings.
- Fixed single-session TPO expansion layout so expanding one session no longer disables the normal composite/volume-profile layout for every visible session; the expanded rendering is now isolated to the chosen profile instead of affecting the whole session overlay.
- Fixed single-session TPO expansion so the selected expanded profile now hides its volume profile too, matching the existing `Expand blocks` behavior instead of leaving VP visible during expanded-session rendering.
- Updated session TPO expansion state to support multiple expanded sessions at once, so `Expand profile` / `Collapse profile` now toggles the targeted session independently instead of behaving like a single-slot expanded-session mode.

## 2026-05-12

### UI
- Added a TradingView-style chart right-click context menu with app-native actions for `Reset chart view`, `Copy price`, `Paste`, mock `Add alert` and order drafts, `Remove drawing`, `Remove indicators`, and context-aware `Settings`, wiring chart-background and drawing right-clicks into the same white-theme menu flow.
- Fixed drawing right-click menus so the selected overlay now keeps its context through the shared chart-stage menu path, allowing overlay-specific actions like `Rectangle Settings` to appear directly under the main `Settings...` entry.
- Fixed chart context-menu edge positioning so taller menus now shift upward and stay visible near the bottom of the viewport instead of clipping off lower actions.
- Updated the fixed-range TPO settings dialog title to show `Fixed range TPVO` while keeping the session-profile dialog title as `TPO`.
- Updated the chart right-click menu label for the fixed-range TPO overlay to show `Fixed range TPVO Settings`, matching the overlay's settings dialog title.
- Updated the chart right-click menu label for the fixed-range volume profile overlay to show `Fixed range Volume Profile Settings`, making the fixed-range tool name explicit in the context menu.
- Fixed Symbol Search in dev mode so the results list scrolls inside the modal again, and built-in CSV symbol discovery now refreshes when the modal opens so newly added `data/` folders appear without restarting the app.
- Reworked the shared TPO renderer toward the TradingView reference by removing the session/value-area background tint, constraining the non-expanded TPO letters to an early-session fixed candle footprint, shifting the volume profile into a later-session candle region, and extending the main TPO level lines across the combined profile width.
- Refined the TradingView-style TPO layout to use session-width percentages instead of fixed candle counts, including a right-side volume-profile band that starts at about `69.23%` of the session width and grows from the left edge of that band so the profile scales more consistently across different timeframes and zoom levels.
- Tightened the TradingView-style TPO visuals further by limiting POC and single-print row fills to the TPO footprint instead of washing across the VP band, reducing their opacity, and shifting the default outside-value-area VP rendering toward a lighter cyan treatment when the legacy gray volume color is still active.
- Moved right-side TPO volume-value labels to the left side of the VP band so the printed volume values no longer stack outside the profile on the far right.
- Adjusted chart session-break markers in both single-chart and multi-chart layouts so the dashed day-separator line now sits between the previous session’s last candle and the next session’s first candle instead of cutting through the first candle body.
- Tightened the TradingView-style TPO/VP session layout again so the right-side volume-profile band is anchored directly to the session end at full zoom, with the TPO footprint constrained ahead of it instead of leaving a visible gap before the session boundary.
- Corrected the right-side TradingView-style VP rendering so individual volume bars are anchored to the session-end edge again while keeping the volume-value labels on the left side of the VP band.
- Narrowed the TradingView-style right-side VP band so it now ends inside a tighter session-width zone instead of stretching all the way to the session boundary, matching the smaller highlighted band from the TradingView reference more closely.
- Flipped the TradingView-style right-side VP bar growth direction within that narrowed band so the histogram now expands from the opposite edge without changing the band’s start/end placement.
- Moved the top-bar chart layout control into the far-right utility cluster and positioned it immediately to the left of the account badge, keeping it grouped beside the final `Settings` control.
- Compacted the top-bar chart layout popup with a narrower card, denser preset rows, smaller layout icons, and shorter `Sync in layout` labels to better match the TradingView-style menu proportions.
- Changed the default `Sync in layout` state so `Crosshair`, `Time`, `Date range`, and `Drawings` start enabled, while `Symbol` and `Interval` start off.
- Wired multi-chart drawing actions to the selected active pane instead of always targeting the default chart, and scoped drawing cancel/clear plus undo/redo to that same active chart selection.
- Strengthened the multi-chart active-pane highlight again by adding explicit pane separators plus an internal active-frame overlay for multi-chart layouts, while leaving single-chart mode clean and unhighlighted.

## 2026-05-11

### Multi-chart
- Fixed click-based multi-chart time-sync markers so the timestamp callout auto-hides on both the clicked chart and synced receiver charts, and updated primary multi-layout detection to treat every TradingView-style preset as multi-pane mode.
- Reworked the layout picker from simple pane counts into TradingView-style preset groups from `1` through `8`, including horizontal, vertical, grid, and split layouts such as `2h`, `3s`, `4s-l`, `6c`, and `8v`.
- Added a `Sync drawings` multi-chart toggle that mirrors regular drawing-tool overlays from the primary chart into visible secondary panes while keeping profile/TPO overlays local.
- Made `4` chart `Sync time` use the same smooth animated timestamp focus as the `2` chart layout instead of jumping instantly.
- Fixed multi-chart sync option toggles so clicking `Sync crosshair`, `Sync time`, or `Sync date range` no longer reloads passive panes and resets their visible chart layout.
- Updated `Sync time` so the clicked source pane is centered on the selected timestamp too, matching TradingView's same-point-of-time behavior across the whole layout.
- Split TradingView-style multi-chart synchronization semantics so `Sync time` now controls click-to-same-time behavior only, while the continuous visible-window scroll behavior is controlled by a new `Sync date range` toggle.
- Fixed the switch back from `2`/`4` pane layouts to the single-chart layout so the primary chart now reapplies its viewport fit after the grid finishes expanding, preventing the collapsed chart from keeping the narrower multi-pane candle spacing until a manual reset.
- Fixed primary-pane multi-chart `Sync time` behavior so normal candle clicks now broadcast the selected timestamp across the visible layout even when replay bar selection is not armed, matching the existing secondary-pane time-sync behavior.
- Updated click-based multi-chart `Sync time` to show the existing timestamp callout bubble on the synced charts, making candle-click time alignment visible without changing the continuous viewport-follow sync path.
- Tightened `4` chart click-based `Sync time` so receiver panes now jump immediately to the selected timestamp instead of relying on simultaneous animated sync moves, while leaving the working `2` chart layout behavior unchanged.

## 2026-05-09

### Multi-chart
- Hardened passive multi-chart pane teardown by replacing anonymous event-bus listeners with named handlers and explicitly cleaning up `Go to date` timers plus pane-local overlay DOM on destroy, reducing the chance of stale listeners or leftover UI after layout changes.
- Fixed multi-chart `Go to date` targeting so the dialog now follows the active pane's loaded bars and routes the focus jump to that selected chart, while restoring the focus marker only for deliberate `Go to date` jumps instead of ordinary sync events.
- Corrected the synced-latest viewport path again so panes near the live edge now fall back to `scrollToRealTime(0)` instead of pinning the last candle flush against the price scale, preserving the intended `9` bars of right-side space in multi-chart layouts.
- Added a focused multi-chart viewport stabilization pass by centralizing the shared anchor-timestamp, nearest-bar, right-gap, and follow-scroll calculations into one helper, and stopped sync-focused primary panes from showing the `Go to date` marker during ordinary multi-chart time syncing.
- Corrected continuous multi-chart viewport sync so it now follows the visible anchor candle instead of forcing the hovered pane's right-edge timestamp onto sibling charts, preserving the intended `9` empty bars on the right across synced layouts.
- Fixed the primary chart bottom-controls `Reset` path in multi-chart mode so it now re-anchors with the intended right-side breathing room instead of snapping the latest candle flush against the price scale.
- Upgraded `Sync time` from click-only time focusing into continuous horizontal viewport syncing, so manually scrolling one pane through history now keeps the other visible charts following that same time window in real time.
- Fixed the first switch into multi-chart layouts so the primary chart now reapplies the same default viewport fit as secondary panes, removing the initial price-scale/crosshair mismatch that previously required a manual bottom-controls `Reset`.
- Stopped multi-chart sync toggles from force-resetting every visible pane back to the default viewport, so switching `Sync symbol`, `Sync interval`, `Sync crosshair`, or `Sync time` no longer kicks manually scrolled historical charts out of position.
- Aligned multi-layout default and reset viewport behavior so visible panes in `2` and `4` chart layouts snap back to the latest candle while preserving the existing `9`-slot right-side breathing room, with pane spacing now derived from the actual single-layout workspace width so multi-chart candles keep the normal default thickness instead of looking compressed.
- Advanced Phase 2 multi-chart sync by wiring crosshair and candle-time broadcasts through the workspace, and expanded the Layout sync menu with `Sync crosshair` and `Sync time` toggles so visible panes can mirror hover position and time jumps across the grid.
- Fixed multi-chart pane targeting so passive charts can render their own session-break lines and chart-stage overlays instead of everything attaching to the first chart stage, and improved passive crosshair timestamp resolution so hover sync follows more reliably across visible panes.
- Extended passive multi-chart panes with their own session-break rendering path, upgraded crosshair sync to carry both timestamp and price so the price-scale label can follow across panes, and adjusted time-sync centering so selected candles land closer to the intended center when multi-layout right-gap spacing is active.
- Refined multi-chart cursor sync again so crosshair broadcasts now prefer the actual hovered `x/y` price conversion over candle close values, preventing the receiving pane's price label from sticking to the candle when the source cursor is between bar levels.
- Matched the tradingview-style cursor sync more closely by carrying pane id plus normalized `y` context with each crosshair broadcast, allowing receiving panes to reconstruct the hovered price line from cursor position instead of snapping back toward candle-derived values.

## 2026-05-08

### Chart navigation
- Adjusted the default and reset chart viewport to keep a TradingView-style right margin, targeting `273` visible candles with `9` empty slots on the right and avoiding the previous edge-sticking behavior on the latest candle.
- Moved the bottom chart navigation controls into the KLineCharts candle-pane main DOM so they anchor like the tradingview reference and no longer sit on top of the time scale.
- Updated the reset chart action to also restore the candle pane y-axis to auto-scale, so reset now re-centers both the horizontal viewport and price range.

### Multi-chart
- Started Phase 1 multi-chart support with a layout selector for `1`, `2`, and `4` chart views, plus first-pass `Sync symbol` and `Sync interval` toggles that propagate the primary chart selection into visible secondary charts.
- Added a shared multi-chart workspace that keeps the existing full-featured primary chart in place while mounting lightweight secondary KLineCharts instances around it, using per-chart loader IDs so viewport presets and load events no longer clash across panes.
- Extended the Phase 1 workspace with active-pane selection, so clicking a visible pane now marks it active and routes top-bar symbol and timeframe changes to that chart; when sync toggles are enabled, the active pane can also drive symbol and interval propagation across the grid instead of only the first pane doing so.
- Finished the missing Phase 1 pane chrome for secondary charts so non-primary panes now render the same watermark/logo and bottom navigation controls as the main pane instead of appearing as bare squeezed canvases.
- Corrected passive-pane chart chrome anchoring so multi-chart bottom controls now mount into each pane's candle area instead of hovering on the time scale.

## 2026-05-07

### Replay
- Fixed replay candle picking so normal candle clicks no longer trigger replay selection unless the replay toolbar is open and explicitly armed for `Select bar`.
- Tuned the replay toolbar and speed dropdown sizing to better match TradingView proportions, including denser controls, tighter spacing, and a more compact speed menu.
- Fixed replay toolbar interactivity during active playback by preventing full toolbar re-renders on every replay tick, so play/pause and speed changes remain clickable while bars are advancing.
- Fixed the chart resize flow when the replay toolbar opens or closes so the time axis no longer sits underneath the replay bar.

### Chart navigation
- Refined bottom-nav reset behavior so chart reset now restores the default viewport and anchors explicitly to the latest candle timestamp, closer to the tradingview reference behavior than the previous `scrollToRealTime()` reset.

## 2026-05-05

### Build and data loading
- Added a static `dist/data/symbols-manifest.json` build output so static hosts can use a prebuilt built-in symbol list while the existing `dist/server.mjs` runtime still supports manual CSV folder drops into `dist/data/`.
- Added browser-side custom CSV folder import backed by IndexedDB, merging imported symbols into the offline symbol search flow without changing the built-in `data/` folder workflow.
- Switched built-in data and SVG sprite URLs to respect Vite `BASE_URL`, and added `npm run build:static` for relative-path static builds that are easier to deploy to GitHub Pages-style hosting.
- Changed `npm run build:static` to ship an empty `dist/data` manifest so GitHub Pages deployments rely on browser-side CSV imports only, while the normal build still keeps the local `dist/data` runtime flow for `server.mjs`.
- Added a true empty-data startup path for browser-import-only static builds, so the app now opens into an import prompt instead of hanging on `Loading chart...` before the first CSV folder is selected.
- Made the GitHub Pages/browser-import build the default `npm run build` path, and moved the older local runtime packaging flow to `npm run build:runtime` for cases where `dist/server.mjs` is still needed.

### Replay
- Fixed replay speed behavior so the existing `1x, 2x, 5x, 10x, 20x, 50x, 100x, 200x` dropdown now matches its labels more closely at runtime, including the high-speed options that previously collapsed under the timer floor instead of advancing at distinct rates.
- Replaced the temporary bars-per-second replay speed list with the tradingview-style scale shown in the reference capture: `10x`, `7x`, `5x`, `3x`, `1x`, `0.5x`, `0.3x`, `0.2x`, and `0.1x`, including the matching “updates per second” / “1 update every N seconds” semantics.
- Rebuilt replay controls into a TradingView-style bottom toolbar above the chart footer, using local sprite icons for select bar, play, forward, speed, interval, jump-to-realtime, and exit instead of the previous floating replay panel.

## 2026-05-04

### Replay
- Added tradingview-style replay candle picking on chart clicks: clicking a candle now fills the replay panel from that exact candle, opens the replay panel if needed, and shows a helper note so replay starts forward from the selected bar instead of requiring manual datetime entry.

## 2026-05-03

### Replay
- Added a first chart replay system modeled on the tradingview flow, including a replay-aware data-loader path, a chart-side `ReplayController`, top-bar `Replay` activation, and a floating replay control panel with start/jump, play/pause, step, step back, restart, skip, exit, speed selection, hotkeys, and live replay status metrics.
- Wired replay into the local CSV runtime so historical playback now uses the same KlineCharts v10 `setDataLoader({ getBars, subscribeBar, unsubscribeBar })` lifecycle as the live offline chart, replaying forward through `subscribeBar` updates while reverse/restart/exit operations rebuild the chart slice through `resetData()`.
- Preserved replay across timeframe changes for the same symbol by re-aligning the replay cursor to the active timeframe's cached bars, while symbol changes intentionally exit replay and return the chart to normal offline mode.
- Upgraded replay from a simple index-based slice into a session-state engine with `visibleBars` and a forward replay buffer, enabling more tradingview-like behavior such as mid-candle start reconstruction, cleaner same-symbol timeframe switches, and accurate restart/skip handling without losing replay context.
- Added lower-timeframe partial-candle reconstruction for replay starts and same-symbol timeframe changes, so when the chosen replay time lands inside a larger candle the chart now begins from an aggregated in-progress candle instead of always jumping to the full higher-timeframe bar.
- Made `Step back` boundary-aware so rewinding across the original replay start restores a reconstructed partial candle when the session began mid-bar, instead of bluntly removing that candle and making the replay context feel discontinuous.

### Performance
- Reduced custom indicator CPU cost by rewriting `Volume * YSTC` moving-average and lookback calculations into single-pass series builders, switching alternate-symbol volume alignment from map lookups to a linear timestamp merge, and limiting its custom draw pass to the current visible range while suppressing dense marker text at very narrow bar widths.
- Accelerated shared volume-profile and TPO profile math by replacing full `bars x bins/rows` scans with overlap-range iteration, so FRVP, Session Volume Profile, TPO, and Fixed Range TPO no longer walk every bin for every bar when only a small price slice overlaps.
- Added lightweight profile-result caching for Fixed Range Volume Profile, Session Volume Profile, and shared TPO builders so repeated scroll and repaint cycles can reuse previously computed profile structures when the source bars, selection range, and settings have not changed.
- Trimmed smaller redraw and interaction costs by removing repeated spread-based high/low scans in fixed-range anchor normalization, reusing precomputed TPO total volume in overlay summaries, and reducing Swing High/Low allocation and hit-test overhead.

## 2026-05-02

### Performance
- Reduced chart-pan overhead by batching visible-range follow-up work into one animation-frame pass and by limiting session TPO auto-row preview recalculation to moments when the Session TPO settings dialog is actually open in `Auto` mode, instead of recomputing that preview on every scroll tick.
- Improved first-time timeframe switching by making TF changes hand off to the chart loader immediately, sharing in-flight CSV loads through a promise cache, and warming a few likely nearby/favorite timeframes in the background so later indicator and overlay calculations can reuse the same cached bar data without duplicate fetch/parse work.
- Hardened offline chart loading against stale symbol/timeframe responses by discarding out-of-date `getBars` completions after rapid switches, and improved symbol precision setup by deriving price/volume precision from already cached bars instead of always forcing `2` / `0`.

### Indicators
- Added a new `Volume source` input section to the custom `Volume * YSTC` settings so the study can now use either the main chart symbol or another locally available symbol's volume on the current chart timeframe, with async CSV caching, timestamp alignment onto the active chart, and source status shown in the indicator status line.
- Fixed the custom `Volume * YSTC` pane scaling by exposing its `volume` and MA result fields through KLineCharts indicator figures, so the sub-pane axis now uses real volume values instead of falling back to the library's default `0-10` range while preserving the existing custom bar rendering and marker logic.
- Increased the default height used for newly created indicator sub-panes from the library baseline of `100` to `150`, so studies like `VOL`, `Volume * YSTC`, and `CVD` open with 50% more vertical space by default while leaving the main candle pane behavior unchanged.
- Refined the custom `Volume * YSTC` `Vol > Pre Vol` marker rendering so the asterisk now appears smaller and sits just above each volume bar, matching the TradingView reference more closely instead of floating higher in the pane.
- Expanded the `Volume * YSTC` indicator Inputs configuration toward the supplied TradingView layout by adding grouped settings sections, inline help hints, and the missing `Volume Ratio`, `Look back`, `Timeframe`, and `Wait for timeframe closes` fields while preserving the existing volume study behavior and calc-param persistence.
- Reworked the `Volume * YSTC` indicator settings modal into a TradingView-style `Inputs / Style / Visibility` flow for this study, wiring the Style tab's six volume palette colors, `Vol > Pre Vol` marker styling, `Volume MA` visibility/color, live `Volume Spike` marker styling, and status-line / price-scale display toggles into the custom indicator render path.
- Added a new custom `Volume * YSTC` sub-pane study based on the supplied TradingView script, including selectable MA types, optional MA overlay, `Vol > Pre Vol` markers, ultra-high/threshold coloring logic, and local chart-volume sourcing without the Pine `request.security(...)` remap.

### Overlays
- Fixed the Date and Price Range drawing label so its bar count, elapsed time, and volume are recalculated from the moved endpoint timestamp instead of reusing the endpoint's stale data index after handle drags.
- Changed Fixed Range Volume Profile settings from one shared live bucket into a dual-mode flow: toolbar right-click now edits defaults for future FRVP drawings, while right-clicking a placed FRVP edits only that selected overlay so multiple profiles on different dates can keep independent settings.
- Added a persisted `Volume source` setting to Fixed Range Volume Profile so the overlay can now use either the main chart symbol or another local symbol's volume on the current chart timeframe while preserving the existing FRVP lower-timeframe fallback logic and falling back cleanly to the chart symbol when the chosen alternate symbol lacks that timeframe.
- Updated the Fixed Range Volume Profile on-chart summary to include the resolved source symbol name alongside the source timeframe and source bar count, so alternate-symbol volume sources are visible directly on the overlay.
- Corrected Fixed Range Volume Profile alternate-symbol sourcing so the overlay now keeps the active chart symbol's price bars and only substitutes the selected source symbol's volume, preventing POC/VAH/VAL and profile rows from being built from the wrong instrument.

## 2026-05-01

### Indicators
- Added a new custom `Cumulative Volume Delta` (`CVD`) sub-pane indicator with TradingView-style `Anchor period`, `Use custom timeframe`, and `Timeframe` inputs, a zero reference line, live tooltip/legend values, and automatic lower-timeframe CSV sourcing when a finer local file is available.
- Wired `CVD` into the shared indicator catalog, registration flow, and indicator-refresh path so asynchronous lower-timeframe cache fills can trigger a clean recalculation without re-adding the study manually.
- Refined `CVD` calculation behavior to match the supplied guide more closely by using the 1h-4h auto-source exception (`1m`) and carrying forward the previous intrabar direction when flat intrabars also match the previous close.

## 2026-04-30

### Drawing tools
- Added a project-local custom `editableText` figure for the `Text` drawing tool and switched text labels to render through it, improving hit-testing and empty-label behavior while keeping the existing modal-based editing flow intact.
- Aligned the `Text` settings panel more closely with the shared TradingView-style controls by moving its color pickers onto the shared swatch palette flow, adding real border opacity support so Border matches Background behavior, and restoring proper modal scrolling for the lower text settings rows.
- Expanded the custom `Text` drawing tool into a TradingView-style settings-backed annotation: new labels open an editor after placement, toolbar right-click edits defaults, placed labels support right-click editing, and the Text tab now controls content, color, size, bold/italic, background color/opacity, border color/width, and text wrapping while leaving Visibility as a placeholder.
- Aligned the custom `Brush` overlay more closely with Coinray's KLineCharts free-path pattern by using a two-step overlay lifecycle, hiding default anchor handles for freehand strokes, and explicitly rendering the stroke as an unsmoothed polyline while keeping the existing style settings and placeholder Visibility tab.

## 2026-04-29

### Drawing tools
- Reworked the custom `Brush` tool to draw through chart-stage pointer capture instead of KLineCharts' click-step overlay lifecycle, so strokes now render live while dragging and finish on pointer release much closer to TradingView's freehand behavior.
- Added a first custom `Brush` drawing tool with TradingView-style persisted `Style` and `Visibility` settings, including freehand stroke capture, optional background fill, toolbar right-click defaults, and right-click editing for placed brush drawings.
- Replaced the built-in Fib `5. Rotation Range` preset with the supplied TradingView-style defaults, enabling the compact rotation ladder around parity (`-0.1`, `0`, `0.1`, `0.5`, `0.9`, `1`, `1.1`) with the matching black midpoint and red/green offset colors.
- Replaced the built-in Fib `4. Trend Pullback` preset with the supplied TradingView-style defaults, enabling only the `0`, `0.382`, `0.5`, `0.618`, and `1` levels and matching the custom black `0.5` / green `0.382` color treatment.
- Replaced the built-in Fib `3. Std. Projections` preset with the supplied TradingView-style level set and enabled states, including the alternating negative projection ladder (`-2.25`, `-4.25`, `-6.25`, `-8.25`) and the custom `-1` tint.
- Enabled the deeper `-3.5`, `-4`, `-4.5`, and `-5` levels in the built-in Fib `1. Risk : Reward` preset so newly applied defaults include those extended risk:reward targets by default.
- Updated the default Fib retracement preset so new tools and resets now start with trend-line thickness `1`, level-line thickness `1`, `Extend right` enabled, and `Background` disabled.
- Replaced the Fib settings footer `Reset` button with a TradingView-style `Defaults` dropdown that offers `Save as...`, `Apply defaults`, and a removable locally persisted preset list in the footer menu.
- Applied the supplied TradingView-style `1. Risk : Reward` Fib preset levels to the built-in defaults menu entry, including its enabled states, reordered positive/negative level values, and matching base line colors.
- Applied the supplied TradingView-style `2. OTE` Fib preset levels to the built-in defaults menu entry, including its enabled states, custom `0.62` / `0.705` / `0.79` levels, and matching base line colors.
- Reverted the experimental Fib retracement level-label alignment split after it pushed labels away from the existing TradingView-style edge anchoring; the overlay is back to using the prior label placement behavior.
- Updated Fib retracement level labels to render with an opaque white in-chart label background again, so `Center + Middle` and `Right + Middle` placements match the TradingView-style look by masking the level line under the text instead of showing the line through the label.
- Refined Fib retracement `middle` label rendering again so the level line now leaves a small gap for the label box itself, bringing `Center + Middle` and `Right + Middle` much closer to TradingView by avoiding the line-under-text look rather than only painting over it.
- Swapped the Fib label fill from white to the active level tint when background bands are enabled, so the in-line label treatment now matches the TradingView-style colored overlay look more closely instead of reading as a separate white chip.
- Changed the Fib in-line label fill to transparent, keeping the line-gap treatment but removing the visible label chip so the text sits cleanly over the chart without a white or tinted box.
- Corrected the Fib `Labels` horizontal placement modes against the TradingView non-extended reference: `Left` now sits outside the left span edge, `Center` stays centered on the span, and `Right` sits outside the right span edge while preserving the transparent-label rendering.
- Matched the Fib extended-edge label behavior more closely to TradingView by keeping `Left` and `Right` labels truly outside the line endpoints even when `Extend left` or `Extend right` is enabled, instead of clamping those labels back inside the pane.
- Corrected the Fib `Right` label visibility at the chart edge so right-aligned labels stay readable when the line reaches the pane boundary, instead of rendering off-canvas.
- Corrected the matching `Left` edge case as well, so extended Fib labels now stay outside when possible and switch to readable edge-hugging placement only when the pane boundary would otherwise clip them.

## 2026-04-28

### Drawing tools
- Fixed the custom `fibonacciLine` overlay so the Fib retracement tool now follows KLineCharts' expected two-point draw lifecycle (`totalStep: 3`) instead of stalling with the repo's previous custom override; the overlay also now renders left-anchored price-plus-percent labels using the active pane precision while keeping project theme colors.
- Added a first Fib retracement settings flow modeled on the Rectangle / Horizontal Ray editors: right-clicking the toolbar icon opens default Fib `Style` settings, right-clicking a placed Fib opens `Style` and `Coordinates` tabs for that overlay, and new or edited Fibs now persist shared line color / opacity / thickness / style defaults through overlay `extendData`.
- Expanded the Fib retracement `Style` tab toward the TradingView property layout you provided: it now includes Trend line styling, shared Levels line width/style, Extend controls, a per-level enabled/value/color list, `Use one color`, Background opacity, Prices / Levels / Text toggles, label alignment, and font size; the overlay rendering now follows those settings for level visibility, colors, backgrounds, label content, and line spans.
- Completed the Fib settings defaults and remaining style controls against the TradingView inspect reference: the default trend line, level widths/styles, 24 level values, enabled states, and per-level colors now match the supplied markup, and the panel now also includes `Reverse`, the `Levels -> Values` row, separate `Labels` and `Text` alignment controls, and the disabled `Fib levels based on log scale` row while the overlay honors reverse direction and the split label/text alignment settings.
- Aligned the Fib retracement color swatches with the app's default shared swatch/palette behavior, restoring the active swatch state and replacing the fib-only compact palette layout with the standard shared palette grid.
- Restored the Fib `Levels` list to an inner scroll region and moved its color palette to a floating anchored popup, so the lower style controls stay visible while level swatches still open cleanly inside the modal.
- Switched the Fib `Style` tab back to one primary modal scroll flow by removing the inner levels-list scrollbar, and adjusted right-aligned Fib labels so they stay visible against the chart's right edge instead of rendering off-canvas.
- Gave the Fib settings modal its own scrollable body instead of inheriting the Rectangle body overflow behavior, fixing the clipped `Style` tab so lower rows like `Labels`, `Text`, and `Font size` can scroll into view normally.

## 2026-04-27

### Overlays
- Restored the missing checkbox ticks in the Fixed Range Volume Profile, Session Volume Profile, and Rectangle settings panels by wiring their toggle rows back to the shared TradingView-style checkbox markup, so paired options like `Values`, `Extend right`, and rectangle setting toggles are visible again.
- Reshaped the Rectangle settings modal toward the TradingView dialog pattern: it now has a compact editable-title header, `Style`, `Text`, `Coordinates`, and `Visibility` tabs, a single extend selector, tighter appearance controls for border / middle line / background, and grouped coordinate editing for selected rectangles.
- Added a TradingView-style line-property popover to Rectangle settings for `Border` and `Middle line`, so those controls now open one compact panel for color, opacity, thickness presets, and line style instead of splitting them across separate inline inputs.
- Changed Rectangle `Extend` to a TradingView-style dropdown with independent `Extend left` and `Extend right` toggles, while keeping the same underlying overlay behavior and summary label.
- Reworked Rectangle `Text` settings toward the TradingView layout: the tab now uses a compact text toolbar for color, size, bold, and italic, a cleaner textarea body, and a single alignment row with vertical and horizontal dropdowns; bold and italic now also flow into the rendered rectangle label styling.
- Updated Rectangle `Coordinates` to use TradingView-style numeric inputs with native up/down steppers for price and bar editing, and removed the extra delta summary so the tab now matches the reference layout more closely.
- Changed Rectangle default/reset styling to your requested purple preset: border `#9c27b0` at thickness `2`, middle line `#9c27b0`, background `#9c27b0` at `20%` opacity, and text `#9c27b0` at font size `14`.
- Added a full Horizontal Ray settings flow modeled on the Rectangle editor: right-clicking a ray now opens `Style`, `Text`, `Coordinates`, and `Visibility` tabs, with a TradingView-style line-property popover, a real `Price` toggle, text styling/alignment controls, and a single `#1 (price, bar)` coordinates row backed by live overlay updates.

### Drawing tools
- Changed magnet-mode switching to immediately update the active overlay draft or currently selected drawing, so tools like Trendline now snap with `Weak magnet` or `Strong magnet` even when the magnet toggle is turned on after the tool is already active.
- Added a temporary `Ctrl` strong-magnet shortcut for drawing tools: holding `Ctrl` now forces active drafts and selected overlays into `Strong magnet`, and releasing it returns the overlay to the previously chosen persistent magnet mode.
- Reworked the custom rectangle tool toward TradingView-style behavior: it now supports editable `Style`, `Text`, and `Coordinates` settings, including border/background styling, optional middle line, extend-left/right, in-box text placement, and right-click editing on placed rectangles.

## 2026-04-26

### Layouts
- Removed the multi-chart layout workspace again and restored the runtime to a single-chart shell, dropping the top-bar layout picker, mirror panes, and pane-sync event flow after the layout path proved unstable.
- Fixed multi-chart pane resizing so layout-mode charts now respond to real workspace size changes, not just preset switches and browser resizes; opening the right panel, expanding the account manager, and similar shell shifts now trigger fresh pane sizing across visible charts.
- Reintroduced multi-chart layouts using a tradingview-style preset system rebuilt natively for this repo, adding `chart1` through `chart16`, active-pane selection, the expanded layout picker structure, and stored sync toggles for `Crosshair`, `Time`, `Symbol`, `Interval`, and `Drawing`.
- Removed the multi-chart workspace completely and returned the app shell to a single-chart layout, including the mirrored chart panes, preset state, pane-sync event wiring, and the top-bar chart layout picker.

## 2026-04-25

### Indicators
- Restyled the custom indicator pane legend to follow the TradingView-style two-state pattern more closely: the resting state now shows the indicator name and live values inline, while hover, focus, and active-menu states switch to the bordered action chip with visibility, settings, remove, and overflow controls.
- Added sticky indicator-legend selection behavior so clicking a legend row keeps its action chip expanded until the user clicks elsewhere, while preserving working `hide`, `settings`, `remove`, and `more` actions.
- Added the missing `Swing H/L * YSTC` low/mid/high enable toggles from the original Pine version and wired them through indicator settings, while keeping backward compatibility with older three-strength-only saved calc params.
- Changed editable chart-study and profile settings to react live while values change instead of waiting for blur or a final apply step, so indicator inputs and profile numeric fields now preview immediately while `Apply`/`Done`/`Ok` still leaves the resulting settings in place.

### Layouts
- Expanded the TradingView-style chart layout picker to include a full `3-chart` section with six presets (`3 horizontal`, `3 vertical`, `left focus`, `right focus`, `top focus`, and `bottom focus`), while keeping the `4-chart` section limited to the single `2x2` grid and preserving the existing `Sync in layout` controls.
- Reworked multi-chart pane placement to be driven directly from layout preset metadata instead of hardcoded CSS slot mappings, making the `1`, `2`, `3`, and `4` layout menu sections easier to extend without changing the core chart shell.
- Refined the multi-chart sync engine around an explicit active-pane model with visible-pane filtering and dedicated sync-from-active helpers, so layout symbol and interval sync now follow a cleaner tradingview-style control flow while staying inside the app's existing eventBus architecture.
- Implemented the previously disabled `Time` sync toggle in chart layouts, so clicking a candle or time-axis crosshair target in any visible pane now scrolls the other visible panes to that same timestamp while keeping `Symbol`, `Interval`, `Crosshair`, and `Date range` sync paths active in the same layout menu.
- Added a TradingView-style chart layout picker to the top bar and wired a first real multi-chart workspace using KLineCharts, with working `1`, `2 horizontal`, `2 vertical`, and `2x2` presets instead of a placeholder layout button.
- Added synced mirror-chart panes for those multi-chart layouts so extra panes follow the shared symbol and interval, and can optionally sync crosshair and visible date range against the main chart in layout mode.
- Added an active-pane model for multi-chart layouts so clicking any visible pane makes it the current chart, the layout menu now shows the current preset on its trigger, and symbol / interval sync toggles now control whether other panes follow the active pane or keep their own selection.

## 2026-04-24

### Indicators
- Added TradingView-style live indicator detail values to the app-owned pane headers, so non-selected studies such as `BOLL`, `VOL`, and `MACD` now show crosshair-driven readings beside their legend title while keeping the existing action controls for the selected/menu state.
- Changed the centered indicator settings modal to use true viewport centering on both axes, so it now balances vertically according to its own panel height instead of appearing pinned near the top.
- Restyled the indicator settings modal to use the same centered modal treatment as the Fixed Range Volume Profile settings panel, with a larger top-centered panel, stronger header hierarchy, and scrollable body for longer indicator input sets.
- Removed the always-on `Main` prefix from custom indicator header chips, so main-pane studies now read as the indicator name directly while pane context stays available only inside the settings/details content when needed.
- Changed pane-anchored custom indicator headers from a horizontal wrap layout to a vertical per-pane stack, so multiple studies in the same pane now appear one below another instead of spreading left-to-right.
- Changed main-pane indicator creation to use KLineCharts stacking semantics when the candle pane already has studies, following the library/reference pattern so adding a second overlay study no longer replaces or blocks the first one.
- Moved the custom main-pane indicator headers lower again to clear the built-in candle status line more reliably.
- Removed the single-instance guard from indicator creation so the chart can now place multiple active copies of the same study type instead of silently ignoring the second add request.
- Moved the custom main-pane indicator headers further below the OHLC status line so they no longer sit on top of the primary chart status row.
- Disabled the native KLineCharts indicator tooltip/header row now that the app uses pane-anchored custom study headers, removing the duplicate built-in labels and action icons that were still appearing under indicators such as `Swing H/L * YSTC`, `RSI`, and `MACD`.
- Hardened pane-anchored indicator header placement by resolving legend anchors from pane root bounds with a deferred post-layout sync, fixing cases where sub-pane studies such as `MACD` could render without their custom header while main-pane studies still appeared.
- Reworked the custom indicator action strip from one shared top-left overlay into pane-anchored headers driven by live KLineCharts pane geometry, so main-pane studies now sit below the OHLC row while sub-pane studies like `VOL` and `MACD` render inside their own pane header area instead of stacking globally at the chart top.
- Re-enabled the app-owned in-chart indicator action strip and wired it to live chart indicator state, so indicator `hide`, `settings`, `remove`, and `more` controls now appear only when indicators actually exist instead of showing as stray chart-header icons in the no-indicator state.
- Removed the misplaced candle status-line tooltip feature buttons from the main OHLC header, so the top-left chart status no longer shows indicator-style actions when no indicator is present.
- Added a shared current-indicator catalog for the app's existing studies (`VOL`, `Swing H/L * YSTC`, `MA`, `EMA`, `BOLL`, `MACD`, `RSI`, `CCI`, `DMI`, `OBV`, and `SAR`) so indicator naming, pane placement, favorites, and editable parameter metadata now come from one source instead of being split across the modal and chart manager.
- Switched indicator add-placement to that shared catalog, so current price-pane studies and sub-pane studies now open through one consistent pane model rather than a hardcoded name check in the chart manager.
- Enabled built-in KLineCharts indicator tooltip feature buttons for the active app path and wired `visibility`, `setting`, and `close` clicks through `onIndicatorTooltipFeatureClick`, giving the native indicator header/tooltip row real action handling again.
- Added a native app-owned indicator settings modal for the current indicator set and wired it to `overrideIndicator(...)`, so the existing supported studies can now edit their current `calcParams` from a shared parameter-schema layer without reviving the old custom in-chart legend.
- Updated the custom `Swing H/L * YSTC` indicator tooltip source to preserve the same shared feature buttons as the built-in indicators, keeping its tooltip behavior aligned with the rest of the indicator stack.

## 2026-04-23

### Indicators
- Restored indicators to the built-in KLineCharts presentation path by removing the custom in-chart indicator legend from the active UI and returning indicator visibility, titles, values, and pane-local labels to the library-owned display.
- Stopped suppressing the built-in `VOL` indicator tooltip/status presentation, so volume studies now behave like the other native indicator panes again instead of being partially rerouted through the custom legend experiment.

### Indicator legend
- Reworked the in-chart custom indicator legend so it now lists all active indicators instead of only main-pane overlays, keeping the custom `hide`, `settings`, `remove`, and `more` actions in one consistent place instead of splitting behavior between the legend and KLineCharts' built-in tooltip features.
- Added first-pass real indicator input editing to the custom legend settings popover for numeric `calcParams`, wiring `Apply` through `overrideIndicator(...)` so common indicators such as `MA`, `EMA`, and similar studies can be adjusted directly from the chart legend.
- Synced the main Settings modal back to live chart state through `settings:sync` and removed deprecated v10 indicator-tooltip fields, reducing drift between the custom UI and the underlying KLineCharts configuration.

### Chart branding
- Added the `KLineChart Logo` from `docs/UI/24 icons.md` to the shared `public/icons.svg` sprite and mounted it as a bottom-left in-chart watermark so the chart workspace now carries a TradingView-style brand mark without interfering with chart interaction.
- Updated the chart watermark to behave like a hover-reveal brand mark: the circular KLineChart icon stays visible by default, and the `KLineChart` wordmark now expands from left to right into its full position only while hovering the logo area.
- Added TradingView-style overflow affordances to the top bar and side rails: those shells now use hidden native scrolling with soft edge fades that appear on hover, making cramped layouts read as intentionally scrollable instead of abruptly clipped.
- Wired those overflow affordances to real scroll state and added direct wheel scrolling for the vertical rails, so the top bar now shows left/right markers only when content is actually clipped and the side rails can be scrolled when the window height gets too small.
- Hardened the mobile shell by increasing touch hit sizes under `720px`, disabling browser touch gestures over the chart surface to reduce page-scroll versus chart-drag conflicts, adding horizontal scrolling for the footer range strip, and calling KLineChart `resize()` again on viewport/orientation changes so the canvas tracks phone and tablet layout shifts more reliably.
- Simplified the left side of the top bar by removing the extra favorite-tools and add-symbol icon pills beside the symbol chip, giving the symbol/search area more room in narrower layouts.
- Added a TradingView-style indicator legend strip inside the chart area with per-indicator action buttons for show/hide, settings, remove, and more; the first pass wires live visibility and removal actions against KLineCharts indicator instances and shows lightweight settings/more popovers for unsupported deeper indicator configuration.
- Changed volume tooltip ownership so when the `VOL` indicator exists, the main candle status line drops its duplicate `Volume` legend entry and leaves volume display to the volume indicator row instead; the built-in VOL tooltip is also suppressed to avoid duplicate top-left labels over the OHLC header.
- Scoped the custom indicator action legend to candle-pane indicators only, so pane-based indicators like `VOL`, `MACD`, and `RSI` no longer duplicate themselves again in the chart's top-left overlay while still keeping their native pane-local KLineCharts labels.

## 2026-04-22

### Offline data discovery
- Extended the Vite offline-symbol manifest scan so startup discovery now records each symbol folder's actual CSV timeframe file codes, letting the app bootstrap and render symbol/timeframe choices from the real `data/` contents instead of a hardcoded fallback list.
- Reworked offline symbol and timeframe selection to use the generated manifest end to end: the app now starts on the first discovered CSV-backed symbol with a valid available timeframe, the symbol search keeps the current interval only when that symbol really has it, and the top-bar interval row/dropdown now show only CSV timeframes present for the selected symbol.
- Changed production packaging to leave `dist/data/` as a tester-managed runtime folder instead of copying the repo's source CSVs into the build, reducing distribution size and allowing each tester to paste their own market folders after receiving the app bundle.
- Added a generated `dist/server.mjs` runtime server so a distributed `dist` folder is self-contained: testers can paste CSV folders into `dist/data/`, run `node server.mjs` from inside `dist`, and get a live-scanned symbol manifest without any source files.

### Time Price Opportunity
- Fixed fixed-range TPO lower-timeframe source slicing so the selected range now includes the full final chart bar instead of truncating underlying source bars at the selected bar's open timestamp; the same selected-range timestamp-window correction is now applied to Fixed Range Volume Profile as well.
- Corrected TPO volume-profile guide lines so `VPOC`, `VVAH`, and `VVAL` are now derived from the actual per-row volume distribution instead of reusing the TPO letter-count POC and value-area rows.
- Tightened TPO row assignment at exact row boundaries so bars no longer double-stamp the next row when their high lands exactly on a row edge, reducing inflated row counts around POC / VAH / VAL and single-print detection.
- Updated the shared TPO settings modal so when Session TPO uses `Rows Size = Auto`, the disabled `Ticks Per Row` field now shows the live computed auto value and switching to `Manual` seeds that same value for direct editing.

## 2026-04-21

### Toolbar
- Added the `Fixed range TPO profile` drawing-toolbar icon from `docs/UI/24 icons.md` to the shared SVG sprite and inserted its button directly above `Time price opportunity` in the left drawing rail.
- Added the `Volume footprint` icon from `docs/UI/24 icons.md` to the shared SVG sprite and inserted its button directly below `Time price opportunity` in the left drawing rail.
- Turned the `Fixed range TPO profile` rail button into a real drawing tool with the same right-click settings access used by the existing TPO overlay.

### Chart interaction
- Disabled the browser's native right-click context menu on the main chart container so chart interactions use the app's own overlay and settings behavior instead of the browser image/canvas menu.
- Extended that native right-click suppression to the app's chart-related modal surfaces and backdrops as well, so settings panels no longer let the browser context menu appear over the chart workspace.
- Simplified the right-click policy further by disabling the browser's native context menu across the entire app root, so the platform consistently reserves right-click for future in-app menus instead of mixing browser and product behavior.
- Kept overlay-tool settings access on the actual overlay instances themselves: Fixed Range Volume Profile, Session Volume Profile, and TPO settings open when right-clicking those overlay visuals on the chart, while generic right-click elsewhere remains reserved for future custom menus.
- Tightened overlay right-click settings access further so those settings now open only when the right-click hits an actual drawn overlay figure, instead of firing from a looser selected-overlay region.
- Narrowed that figure gating for Session Volume Profile and TPO again so their settings now open only from right-clicking the core profile body figures (`session-vp-up/down-*`, `tpo-block-*`, `tpo-volume-*`), not from helper backgrounds, labels, or guide lines.
- Kept custom profile overlays off KLineCharts' default right-click delete path while doing that gating, so Fixed Range Volume Profile, Session Volume Profile, and TPO no longer get removed when right-click misses a settings-eligible figure.
- Added explicit invisible profile hitboxes for Session Volume Profile and TPO so right-clicking anywhere inside the displayed profile footprint now opens that overlay's settings, while clicks outside the visible profile area still do nothing.
- Moved those Session Volume Profile and TPO hitboxes to the top of their figure stacks so they actually win right-clicks across the visible profile footprint instead of being hidden behind other overlay figures.
- Aligned Session Volume Profile and TPO hit targeting more closely with FRVP: Session VP now has a real full-profile background figure like FRVP's box, and TPO settings right-click now also accepts its existing session and value-area background figures instead of relying only on blocks, volume bars, or transparent hitboxes.

### Time Price Opportunity
- Added a dedicated `fixedRangeTpoProfile` overlay that uses the current TPO settings with a two-click selected range, keeps draggable midpoint anchors aligned to the chosen span, and renders its summary card locally so multiple fixed-range TPO drawings can coexist without all stacking text in the chart corner.
- Changed `fixedRangeTpoProfile` to behave like Fixed Range Volume Profile instead of the session-style TPO tool: the selected span now renders as one composite fixed-range TPO profile with a range-local summary card, rather than being split into day/week/month profiles inside the selection.
- Hid the session-only `Period` control whenever the shared TPO settings modal is opened from `Fixed range TPO profile`, and changed fixed-range TPO auto row sizing to derive ticks-per-row from the selected range's own high/low span instead of the rolling 300-bar context used by the session-style TPO overlay.
- Changed the default TPO `Opacity outside VA` setting from `35` to `70` so new profiles keep more visible structure outside the value area by default.
- Expanded the TPO `Expand blocks` control to support both TradingView-style modes: `All` now expands every visible profile, while `Last Profile` expands only the latest visible profile and leaves older profiles consolidated.
- Changed extended TPO POC, VAH, VAL, poor high, poor low, and single-print markers so they now stop at the first later bar that intersects their level or zone instead of always running to the pane edge, bringing the overlay closer to TradingView-style extend behavior.
- Wired the existing TPO VAH / VAL / poor high / poor low line-style settings back into the overlay renderer so persisted `solid` versus `dashed` choices now affect the actual guide lines instead of being ignored.

## 2026-04-18

### Chart Navigation
- Updated the go-to-date focus marker so clicking anywhere on the chart now dismisses the floating marker instead of leaving it stuck onscreen after navigation.

### Time Price Opportunity
- Corrected the TPO auto row-size rounding thresholds so exact boundary values now follow the reference calculation steps (`100 -> 50`, `1000 -> 500`, `10000 -> 5000`, etc.), keeping auto `Ticks per row` aligned more closely with the TradingView-style TPO documentation.
- Changed the default `Single prints` TPO style color to `#e040fb` with `15%` opacity so new profiles start with the requested softer magenta highlight.
- Updated the TPO `Lines And Labels` controls to match the supplied reference more closely by replacing the old `Solid / Dashed` dropdowns with `Don't extend / Extend` controls for POC, poor high, poor low, single prints, VAH, and VAL; `Poor high`, `Poor low`, `VAH`, and `VAL` now render as dashed guide lines, while `POC` and `Single prints` use row-height markers driven by their own setting colors and swatch opacity.
- Changed TPO rendering so `Expand blocks` now hides the volume profile on the chart without turning off the `Show volume profile` setting itself; when expand is turned back off, the volume profile reappears automatically if its setting was still enabled.
- Replaced the temporary browser color input behind the shared swatch `+` button with an inline custom-color editor that includes preview, hex entry, Add action, and a hue/saturation picker, bringing the popup much closer to the TradingView interaction model.
- Wired the shared swatch popover `+` button to a real custom color picker, and fixed profile-overlay color rendering so swatch opacity now survives into Fixed Range and Session Volume Profile histogram colors instead of being overwritten by the row opacity logic.
- Tightened the shared TradingView-style swatch popup again by matching the grouped row structure more closely, adding a dedicated custom-color button row and separate `Opacity` section, and fixing the wiring bug where picking a new swatch did not immediately retint the opacity slider to that selected color.
- Updated the shared platform color swatcher again using the exact TradingView color order supplied from the live popup DOM, and changed the opacity slider to tint from transparent to the currently selected swatch color instead of using a fixed accent.
- Reworked the shared app-wide color swatcher to use a TradingView-style multi-row palette and updated all current swatch consumers (`Settings`, `Fixed Range Volume Profile`, `Session Volume Profile`, and `TPO`) to pull from the same shared palette definition, so the platform now uses one consistent color-popover grid.
- Removed the duplicate `IBR Blocks` input from the TPO `Inputs` tab now that `Initial Balance Range` is managed from the `Style` tab, keeping that setting in one place.
- Reshaped the TPO `Style` tab toward the supplied screenshot by moving `TPO Midpoint`, `Open`, `Close`, and `Initial Balance Range` controls into `Style`, and by giving `Expand blocks` an inline right-side control row that matches the surrounding line-setting layout more closely.
- Removed the separate TPO `Profile mode` control from `Inputs` and wired the existing `Expand blocks` toggle in the `Style` tab to drive the same split/composite behavior, so profile expansion is controlled from one place instead of two competing settings.
- Adjusted the TPO `Inputs` tab to match the supplied TradingView-style screenshot more closely: it now opens by default, uses a combined `Period` row with count plus unit dropdown, switches `Block Size` to compact `m/h` labels, renames the row-size/value-area labels to match the reference, and disables `Ticks Per Row` whenever row sizing is set to `Auto`.
- Turned the `Time price opportunity` toolbar icon into a real `tpoProfile` overlay instead of a placeholder, with managed single-overlay creation, symbol/timeframe source refreshes, right-click settings access, and keyboard `Delete` / `Backspace` removal when selected.
- Added a dedicated TPO settings modal covering the main TradingView guide inputs and style sections: profile period, block size, auto/manual row sizing, value area percent, TPO block/letter display, IBR controls, and the optional volume-profile section with placement, values, colors, and per-line styles.
- Added the first working TPO overlay implementation that groups source bars into day/week/month profiles, assigns TradingView-style letter blocks by time segment, calculates POC / VAH / VAL plus single prints, poor highs/lows, midpoint, open/close, IBR, and renders an aligned optional volume profile beside each TPO profile.
- Corrected the TPO profile visuals so letters now sit inside colored TPO cells instead of rendering as loose chart text, removed the overly heavy full-row POC slab, and added session/value-area background shading to make the profile read much closer to the GoCharting-style reference.
- Added a TPO `Profile mode` setting with `Unsplit` and `Split` options, and changed the default to `Unsplit` so new profiles build as one composite visible-range TPO unless the user explicitly switches back to per-period splitting.
- Refined the TPO settings UI so `Show volume profile` works as the clear VP on/off switch, and the extra volume-profile styling controls only appear when VP is enabled.
- Restyled the TPO settings modal toward the TradingView reference with tabbed sections (`Inputs`, `Style`, `Volume profile`, `Visibility`), stronger section headers, and a compact four-swatch gradient control for the TPO block colors.
- Polished the TPO modal controls further with a `Defaults / Cancel / Ok` footer, a simpler `TPO` title, tighter row spacing, and TradingView-like checkbox styling for toggle rows.
- Added inline `Style`-tab control rows for `POC`, `VAH`, `VAL`, `Poor high`, `Poor low`, and `Single prints`, each with its own toggle, line-style dropdown, and color swatch, and wired those settings through to the TPO overlay renderer.
- Reworked the TPO settings color controls to use the same shared swatch and palette popover system as the main chart Settings and volume-profile modals, so gradient colors and per-line colors now follow the app-wide wick/border picker behavior instead of using native browser color inputs.
- Updated the default TPO gradient palette to the requested four-color mapping: `A-Z` now defaults from `#E91E63` to `#00C853`, and `a-z` now defaults from `#00BCD4` to `#651FFF`.
- Corrected split-profile rendering so `Split` profiles once again span from the session's first candle to its last candle, while `Unsplit` keeps the compact composite layout, and adjusted the TPO gradient interpolation so the active uppercase and lowercase letter ranges reach their intended endpoint colors more faithfully.
- Fixed a TPO rendering bug where poor-high, poor-low, and single-print markers were spanning from each profile across the full chart width; those guides are now clipped to the owning profile so the candle pane no longer fills with chart-wide dashed lines.
- Refined the TPO overlay styling to read closer to the TradingView guide: the POC now uses a profile-local row highlight instead of a full-width line, letters only render when both row height and slot width have enough space, and right-side labels now anchor beyond the full TPO-plus-volume footprint instead of sitting on top of the profile bars.
- Fixed viewport clipping for both TPO and Session Volume Profile so fully offscreen historical profiles are skipped instead of being clamped to the left edge and stacked at `x = 0`, removing the large left-wall artifact when many old profiles exist.
- Added viewport-aware edge handling for TPO and Session Volume Profile labels so profiles near the left or right pane boundary now move their labels to the cleaner side when possible instead of awkwardly clamping them into the chart edge.
- Corrected TPO construction so daily profiles now anchor block letters from the NSE session start (`09:15` IST), which makes `30m` daily sessions progress `A` through `M` as expected, and changed auto row-size handling to compute one shared ticks-per-row value from the rightmost visible context instead of recalculating it independently for each profile.

## 2026-04-17

### Overlay interaction behavior
- Changed volume-profile overlay interactions so right-clicking a selected `fixedRangeVolumeProfile` or `sessionVolumeProfile` on the chart now opens that tool's settings modal instead of letting KLineCharts perform its default right-click deletion.
- Added selected-overlay keyboard deletion so pressing `Delete` or `Backspace` removes the currently selected overlay, while ignoring focused form fields to avoid accidental deletion during modal input.
- Added the `Time price opportunity` left-rail icon below `Session volume profile` using the shared SVG sprite and the `docs/UI/24 icons.md` reference, leaving it as a visual placeholder until the actual TPO feature is implemented.

### Session Volume Profile implementation
- Replaced the Session Volume Profile placeholder overlay with a working auto-rendered per-session profile that groups bars by IST trading day, draws horizontal histogram rows on the candle pane, and highlights POC / VAH / VAL using the existing volume-profile math and styling model.
- Added Session Volume Profile lower-timeframe source resolution based on the TradingView Session VP table, including offline-friendly fallback behavior when the preferred local source file is unavailable, such as falling back from the TradingView `1S` preference to the lowest available second-based CSV.
- Wired Session Volume Profile through the chart manager so it now creates as a managed single overlay, refreshes correctly on symbol and timeframe changes, and reapplies settings to any existing session profile overlay.
- Added a dedicated Session Volume Profile settings modal with persistent defaults and a toolbar right-click shortcut, matching the existing Fixed Range Volume Profile settings workflow.

### Side rail chrome alignment
- Unified the left and right side rails so they now share the same visual width, button hitbox sizing, divider proportions, and vertical spacing, giving the app a more consistent TradingView-style chrome on both edges.
- Tightened the right dock button stack to match the left toolbar rhythm more closely while preserving the existing right-panel interactions and mobile horizontal layout behavior.
- Switched the right rail to use the same core toolbar button and divider chrome classes as the left rail, so hover, active, color, and icon treatment now stay visually identical instead of merely using similar values.
- Increased the collapsed account-manager header and chart-footer shell heights from `30px` to `38px` so the lower chrome aligns with the updated top and side shell proportions.

## 2026-04-16

### Session Volume Profile scaffold
- Added initial Session Volume Profile scaffolding with dedicated overlay and settings files mirroring the Fixed Range Volume Profile structure, ready for the upcoming session-specific calculation and rendering rules.
- Registered the new `sessionVolumeProfile` overlay in the shared overlay registry, but kept it as a non-final placeholder until the actual session logic is provided.
- Added the Session Volume Profile toolbar icon from `docs/UI/24 icons.md` to the shared SVG sprite and placed the new left-rail tool directly below Fixed Range Volume Profile.

### Fixed Range Volume Profile calculation methods
- Changed `Ultimate LTF 1S-5S` (`secs_tf`) so it now attempts the lowest available second-based CSV first, then the lowest available minute-based CSV, and only falls back to the current chart timeframe if no lower source file exists.
- Renamed the Fixed Range Volume Profile `TV Style` calculation method label to `Free 5k bars` in the settings UI while keeping the persisted internal value as `tv_style`.
- Added a new Fixed Range Volume Profile `Essential 10k bars` calculation method that uses the same lower-timeframe resolution sequence as `tv_style` but with a `10000`-bar threshold.
- Added a new Fixed Range Volume Profile `Plus 20k bars` calculation method that uses the same lower-timeframe resolution sequence as `tv_style` but with a `20000`-bar threshold.
- Added a new Fixed Range Volume Profile `Premium 40k bars` calculation method that uses the same lower-timeframe resolution sequence as `tv_style` but with a `40000`-bar threshold.
- Changed the Fixed Range Volume Profile `tv_style` source-timeframe resolver to use a `5000`-bar threshold instead of `20000`, keeping the same timeframe sequence `1`, `3`, `5`, `15`, `30`, `60`, `240`, `D` and the existing `5S` override for second-based charts or ranges up to five minutes.
- Added FRVP source-timeframe loading and caching support so non-`same_tf` calculation modes can pull a lower timeframe dataset for the selected range and gracefully fall back to the chart timeframe when the requested local CSV data is unavailable.
- Updated single-bar FRVP volume classification so a one-bar profile now treats `close > open` as up volume and `close <= open` as down volume, matching the specified TV-style rule.

## 2026-04-15

### Fixed Range Volume Profile settings
- Fixed Fixed Range Volume Profile placement persistence so choosing `Right` now survives settings normalization, storage, and reapplication to existing overlays instead of falling back to `Left`.
- Fixed the Fixed Range Volume Profile settings modal so layout toggles and single-color mode rerender immediately while the dialog is open, keeping the visible controls in sync with the live applied state.
- Updated the Fixed Range Volume Profile color swatch popovers to match the main chart Settings candle palette, including the expanded TradingView-style color grid, opacity slider behavior, and numeric opacity input handling.
- Added Fixed Range Volume Profile row-value labels with a `Values` toggle and text color setting; when `Single color` is enabled the overlay shows total row volume, and when it is disabled the overlay shows down/up row volumes in sequence on the histogram bars.
- Changed the default Fixed Range Volume Profile style so new profiles start with `Single color` enabled, `Values` disabled, the profile color set to the cyan swatch (`#14b8c4`), and both value-area and outside opacities set to `60`.
- Added per-level `Extend right` controls for POC, VAH, and VAL in the Fixed Range Volume Profile settings, allowing each level line to continue to the right edge of the chart independently.
- Refined the Fixed Range Volume Profile settings UI so each `Extend right` checkbox now sits inline with its corresponding `POC`, `VAH`, or `VAL` toggle instead of appearing as a separate stacked row.
- Added a new Fixed Range Volume Profile `Calculation method` dropdown after `Tick size` with `Default (Same TF)`, `TV Style`, and `Secs TF` options; the selection is persisted in settings and reserved for future calculation wiring.

## 2026-04-13

### Swing H/L indicator
- Added a custom price-overlay indicator `Swing H/L * YSTC` for KLineCharts, reproducing the TradingView Pine behavior with three pivot strengths: low-bar dots, mid-bar triangles, and high-bar cross markers above and below the candle extremes.
- Added lightweight custom KLineCharts figure registrations for the Swing H/L glyphs and wired the indicator into chart startup plus the indicator modal so it can be added from the existing `Indicators` picker on the main pane.
- Tightened the Swing H/L marker styling to sit closer to the candle highs and lows, reduced the low-strength dot size, and aligned the high-strength `x` markers directly on top of the mid-strength triangles so the overlay reads much closer to the TradingView reference.
- Matched the Swing H/L mid-strength triangle colors to the TradingView Pine script more closely with dedicated indicator tokens, without changing the app-wide bullish and bearish candle palette.
- Refined the Swing H/L glyph geometry again after screenshot comparison by reducing marker weight, narrowing the triangle silhouettes, shrinking the `x` markers slightly, and pulling all marker tiers closer to the wick extremes for a more TradingView-like overlay density.

### UI consistency pass
- Started a shared UI-system reset across the TradingView-inspired shell by introducing common panel, control, and surface tokens for modal radii, shadows, header spacing, footer actions, and input sizing.
- Tightened the top bar, left toolbar, and right dock to a more consistent chrome rhythm, including more uniform hover states, active states, control radii, and menu surfaces.
- Unified the visual language of Settings, Indicators, Symbol Search, Go To, and Fixed Range Volume Profile settings so those panels now share closer header, close-button, search, field, palette, and footer styling instead of reading like separate mini-apps.
- Rebuilt the right trading panel from the old placeholder form into a TradingView-style side surface with header actions, `Order / DOM` tabs, buy/sell quote strip, compact order-type tabs, grouped risk boxes, stronger primary action styling, and a persistent lower order-info section.
- Upgraded the right dock and account manager chrome to better match the TradingView references, including active rail states, cleaner account header controls, stronger account overview metrics, and a dedicated toolbar row above the lower trading table workspace.
- Turned the right side into a multi-view workspace driven by the top bar and right rail, so `Watchlist`, `Trade`, `Option Chain`, and `Alerts` now render distinct panel layouts instead of reusing a single drawer surface for every action.

### Drawing history controls
- Added working `Undo` and `Redo` controls to the top bar for chart drawings, with button disabled states that reflect the current history stack.
- Implemented overlay drawing history in the chart manager by snapshotting completed overlays and restoring them through KLineCharts overlay APIs after draw, move, remove, and clear actions.
- Scoped undo behavior so an in-progress drawing is cancelled first, matching expected chart-tool behavior before stepping back through completed drawing history.
- Fixed a regression where the new history wrapper was overriding Fixed Range Volume Profile draw and drag lifecycle callbacks; FRVP now runs its own midpoint anchor sync again before the drawing state is recorded for undo/redo.

### Fixed Range Volume Profile anchors
- Restored the earlier Fixed Range Volume Profile edit model: default point handles are enabled again and the overlay re-centers its two anchor points to the selected range midpoint after draw and drag, bringing back the previous blue-point fine-tuning behavior.

## 2026-04-11

### Go to date navigation
- Added a TradingView-style `Go to date` modal on the footer range strip, with date/time inputs, a month calendar, and nearest-bar jumping for the currently loaded symbol and timeframe.
- Wired the new modal into the shared event flow so loaded CSV bars feed the picker state through `chart:data`, and chart navigation now uses KLineCharts `scrollToTimestamp(...)` for actual timeline jumps.
- Refined `Go to date` navigation so the chosen bar now lands near the middle of the viewport and immediately shows its timestamp via a programmatic crosshair focus, matching the TradingView-style behavior more closely.
- Added a dedicated floating timestamp callout above the focused candle after `Go to date`, so the jump now shows a visible TradingView-style date/time tag instead of relying only on the axis crosshair label.
- Repositioned the `Go to` modal to open centered over the chart workspace and removed the unused `Custom range` tab so the dialog now matches the intended single-purpose date-jump flow more closely.
- Tuned the default first-load viewport to reserve roughly 10 empty bars on the right and fit about 284 total visible bar slots when enough data exists, so the initial chart density reads closer to the TradingView reference instead of using the old fixed pixel gap.
- Aligned the main chart chrome closer to the latest TradingView screenshot measurements by tightening the top bar to `38px`, widening the left toolbar to `52px`, setting the right rail to `45px`, and normalizing the bottom range strip and collapsed account dock to `38px` each so the drawable chart area matches the reference proportions more closely.
- Reduced the shared icon sizes and hitboxes across the top bar, left toolbar, right rail, and footer strip to better match `docs/UI/01 TradingView main UI.png`, giving the shell a lighter TradingView-like icon rhythm instead of the slightly oversized previous chrome.
- Rebalanced the left drawing-toolbar icons again after screenshot comparison, increasing the rail glyph size and button hitbox so tools like brush, text, and fixed-range volume profile read closer to TradingView instead of looking undersized.
- Applied the same icon-size correction to the top bar and bottom range strip, increasing their shared icon sizes and hit areas so the main chrome now feels consistent with the corrected left toolbar instead of leaving the `Go to` footer control and top utility icons too small.
- Added a TradingView-style chart-type dropdown to the top bar using the `24 icons.md` reference glyphs and the sequence `Bars (OHLC)`, `Candles`, `Hollow Candles`, and `Area`, wired directly to the existing KLineCharts candle-style setting.
- Fixed the top-bar chart-type dropdown so clicking the Candles control now opens the menu in the viewport instead of clipping it inside the scrollable top bar.
- Reworked the offline-mode timeframe strip to match the TradingView-style favorites row more closely, adding visible `W` and `M` favorites plus a grouped dropdown for `Seconds`, `Minutes`, `Hours`, and `Days`.
- Changed offline timeframe switching so missing CSV intervals no longer trigger fallback aggregation; the chart now stays on the current timeframe when the selected interval file is unavailable.
- Wired the timeframe dropdown stars to the top-bar favorites row, so unstarring an interval removes it from the row immediately and starring it adds it back in the same canonical order, with the preference persisted in `localStorage`.
- Replaced the hardcoded offline symbol list with a dynamic registry built from the actual `data/<folder>/<file>.csv` layout on load, deriving symbol names from folder names and short display names from the portion before the first dash when present.
- Reworked offline symbol discovery again to use a generated `data/symbols-manifest.json` at runtime instead of bundling `data/**/*.csv` via `import.meta.glob(...)`, so builds no longer need to transform every CSV just to populate the offline symbol search list.
- Added a TradingView-style symbol search popup driven by the dynamic offline symbol registry, and in offline mode the top bar now opens that modal and labels the search surface clearly as `Offline` instead of showing exchange-focused metadata.
- Changed the default offline chart bootstrap to open `NIFTYBANK-INDEX` on the daily timeframe instead of the old `SBIN-EQ` 5-minute default, and aligned the top bar and related modal defaults to match.
- Corrected the top-bar mode switch semantics so the toggle now behaves naturally: off means `Offline`, and on means `Online` / broker mode, with the online path left as a not-yet-wired placeholder.
- Implemented `Settings > Events > Session breaks` as a real chart feature for intraday timeframes, drawing TradingView-style vertical break markers at day/session boundaries and refreshing them on data load plus pan/zoom range changes.
- Kept KLineCharts' built-in mouse-wheel zoom behavior and moved the custom chart controls into a centered floating bottom cluster with `-`, `+`, left, right, and reset actions to better match the TradingView-style screenshot.
- Refined the floating bottom chart controls again so they now reveal only near the bottom-center hover zone and render as separate compact buttons instead of a permanently visible grouped tray.
- Normalized Fixed Range Volume Profile anchor points after draw and drag so the start/end handles now stay centered on the selection’s start and end vertical boundaries instead of preserving arbitrary clicked prices that could leave one handle floating away from the profile box.

## 2026-04-09

### Account manager UI and overlay drawing tools
- Added a custom `fixedRangeVolumeProfile` drawing tool with a TradingView-style two-click range selection, per-price volume binning, split up/down histogram bars, and highlighted POC / VAH / VAL levels rendered directly on the candle pane.
- Added a reusable volume-profile calculation helper so fixed-range overlays can distribute bar volume across price bins and derive the value area from the selected range.
- Added the `Fixed range volume profile` button and matching sprite icon to the left toolbar, wiring it into the shared overlay registry so it can be activated like the other drawing tools.
- Added a dedicated Fixed Range Volume Profile settings modal with persistent defaults for rows, placement, width, and POC / VAH / VAL visibility; right-clicking the toolbar button now opens those settings and reapplies them to existing profiles on the chart.
- Updated Fixed Range Volume Profile row sizing to support TradingView-style `Rows layout` behavior, so the tool can now calculate rows by either target row count or explicit ticks per row instead of using a simple equal-bin count.
- Expanded Fixed Range Volume Profile settings with manual `Tick size`, `Value Area %`, `Label placement`, and direct color controls for up/down volume plus the POC / VAH / VAL markers.
- Changed the Fixed Range Volume Profile defaults to left placement with a black POC line, and added `Single color`, `Value area opacity`, and `Outside opacity` controls so the histogram can be styled either with one shared profile color or split up/down colors.
- Preserved Fixed Range Volume Profile overlays across timeframe changes by remapping their saved anchor timestamps onto the newly loaded bars, so existing profiles keep rendering cleanly after a timeframe switch without duplicating overlays.
- Rebuilt the bottom account manager to follow the TradingView Positions reference more closely, with a stronger header, trading-source strip, account summary metrics, tab rail, and positions-style table shell.
- Increased the account manager height and replaced the old placeholder pills with TradingView-like tab underlines and a centered empty-state workspace so the bottom panel reads as a real trading workstation instead of a collapsed footer.
- Changed the account manager to a collapsed-by-default dock so the shell now behaves more like the `01` vs `07` TradingView references: compact in the main chart view, and expanded into the positions workspace only after clicking `Account Manager`.
- Fixed the top-bar `Option Chain` / `Offline mode` cluster so it now includes the missing divider and shows the offline toggle pill before the text label, matching the reference order more closely.
- Moved the top-bar divider to the far side of `Offline mode`, keeping the settings icon anchored at the end of the row while improving the final top-bar grouping.
- Added the missing final top-bar divider between the account badge and the last utility icons, so the rightmost cluster now ends closer to the TradingView grouping.
- Adjusted the final top-bar alignment so only the settings icon is pushed to the far right edge, while the account badge and layout icon stay in their original flow position.
- Ran another shell polish pass using the updated `docs/UI/24 icons.md` reference, switching the settings modal tabs to sprite-based icons, improving account-manager control glyphs, and tightening shared chrome spacing and hover behavior across the top bar, rails, footer, and indicator list.
- Rebuilt the settings modal structure against `docs/UI/04 TradingView Settings.png`, `05 TradingView Settings.png`, and `06 TradingView Settings.png`, adding the missing `Trading`, `Alerts`, and `Events` tabs plus the denser TradingView-style mix of nested checkboxes, compact selects, sliders, and display swatches.
- Upgraded the symbol-tab candle color controls to use TradingView-style swatch popovers with a reference-based palette and inline opacity footer, replacing the plain browser color-input feel for the key candle color settings.
- Fixed the settings modal overflow behavior so swatch popovers now open inward like the TradingView references instead of widening the modal body and causing a horizontal scrollbar.
- Corrected the candle swatch popover anchoring again so both color chips now open their palettes inward within the modal, avoiding right-edge clipping when the first swatch is active.
- Wired the candle swatch palette opacity slider to the actual chart color state, so moving opacity now live-updates the selected candle color instead of acting like a dead footer control.
- Smoothed the candle-palette opacity control by batching live chart applies to animation frames, restyling the slider to a TradingView-like track/thumb, and replacing the fake percentage badge with a real editable numeric input.
- Hardened the settings overlay click handling so interactions inside the panel and color palette no longer fall through the close path, and removed an extra color-pick rerender that could make the modal feel like it was disappearing.
- Removed the full settings-modal rebuild from candle palette color picks, updating swatch state locally instead so choosing a color no longer tears down the modal mid-interaction.
- Tightened the palette trigger and swatch event handling so color-button clicks, palette swatch clicks, slider drags, and opacity input edits all stop propagation before the modal-close path can see them.
- Reworked the candle swatch trigger so opening and closing the palette now happens locally in place instead of forcing a full modal rerender, reducing the chance of the settings panel disappearing during the initial color-button click.
- Fixed the real palette-open crash by removing a recursive settings-apply loop from the candle color controls: opening a swatch no longer immediately re-applies color state, and the symbol-tab color callbacks now update state without double-triggering the modal render path.
- Marked the non-wired settings rows with a `Temporary` badge in the modal so the TradingView-style placeholder controls are visibly distinguished from the KLineChart-backed settings that already work live.
- Updated the default candle palette to start with green/red bodies and borders (`#4caf50` / `#f23645`) and black wick defaults, matching the requested KLineChart-style baseline more closely.
- Split the candle color state and settings wiring into separate body, border, and wick up/down colors, so the symbol tab now truly controls those three candle parts independently instead of sharing one bullish/bearish pair.
- Fixed the left-toolbar `Horizontal ray` tool registration so the emitted `horizontalRayLine` overlay name is now actually registered with KLineCharts at chart startup; also registered the custom `fibonacciLine` overlay alongside it for consistency.
- Renamed the custom horizontal ray overlay to `horizontalRightRay` and pointed the toolbar at that explicit custom name, avoiding a collision with KLineCharts' built-in `horizontalRayLine` and preserving the intended one-point right-only ray behavior.
- Corrected the custom horizontal ray overlay lifecycle to use `totalStep: 2`, matching KLineCharts' single-click line-tool pattern more closely so the tool completes properly instead of leaving repeated anchor dots behind on later clicks.
- Activated the project's custom `rect` overlay in the shared overlay registry, so the left-toolbar `Rectangle` tool now uses the local rectangle implementation with the dashed midpoint line instead of the stock built-in rectangle.
- Corrected the custom rectangle overlay to use KLineCharts' two-point tool lifecycle (`totalStep: 3`), so it can complete properly across the first click, preview move, and second click instead of stalling after the start point.
- Replaced the built-in `priceChannelLine` left-bar tool with a custom `datePriceRange` overlay that combines TradingView-style price-range and date-range behavior: a two-point shaded range box with price change, percent change, bar count, elapsed time, and summed volume stats.

## 2026-04-08

### UI shell and chart foundation
- Replaced the starter app with a TradingView-style white-theme shell using pure TypeScript DOM components.
- Added top bar, left toolbar, right dock, chart footer, order panel scaffold, and account manager scaffold.
- Wired the chart to KlineCharts v10 with local CSV loading for supported symbols and timeframes.
- Updated the build so local `data/` assets are available in production output as well as dev.

### UI reference alignment
- Refined the outer chart chrome to better match the reference images in `docs/UI`, while keeping the center chart area owned by KlineCharts.
- Reworked the layout so the footer and account manager sit only in the center column between the left and right rails.
- Reduced the right-side rail to `Watchlist`, `Alerts`, and bottom `Help`, using the documented icon set.
- Tuned icon contrast, spacing, and panel structure to more closely match the TradingView reference.

### Drawing tools
- Fixed drawing-tool activation so switching tools no longer deletes previously placed overlays.
- Switched core tools such as trend line and horizontal/vertical line to working KlineCharts built-in overlay names.
- Added magnet mode support using KlineCharts v10 overlay modes: `normal`, `weak_magnet`, and `strong_magnet`.
- Made magnet buttons toggle on/off correctly and show active state in the toolbar.
- Added a custom `arrow` overlay implementation and registered it without overriding the working built-in tools.

### Indicators
- Added a TradingView-style indicators modal inspired by `docs/UI/21 TradingView Indicators.png`.
- Changed the `Indicators` top-bar action to open the modal instead of directly adding one hardcoded indicator.
- Added built-in indicator selection for `VOL`, `MA`, `EMA`, `BOLL`, `MACD`, `RSI`, `CCI`, `DMI`, `OBV`, and `SAR`.
- Prevented duplicate indicator creation and routed price-overlay indicators to the candle pane.

### Chart settings
- Added a TradingView-style chart settings modal based on `docs/UI/04 TradingView Settings.png`, `05 TradingView Settings.png`, and `06 TradingView Settings.png`.
- Wired the top-bar settings button to open a dedicated settings panel with the KlineCharts-applicable tabs: `Symbol`, `Status line`, `Scales and lines`, and `Canvas`.
- Connected the modal controls to live KlineCharts updates through `setStyles(...)`, `setTimezone(...)`, and `setOffsetRightDistance(...)`.
- Added live controls for candle type and colors, candle body/border/wick visibility, status-line content, price and time scale visibility, price marks, and the currently wired grid/crosshair behavior.
- Added a TradingView-style `Canvas > Background` row in the settings surface; the current implementation keeps that area as part of the placeholder UI rather than a live chart-backed color control.
- Changed the default canvas configuration so horizontal and vertical grid lines start turned off.
- Increased the built-in high and low price-mark label contrast so those chart annotations are easier to read on the white background.
- Tuned the high/low marks, symbol title, and OHLCV header values to a slightly softer 80% primary-text tone for better visual balance.
- Refined the settings modal chrome to look closer to the TradingView references, with a flatter left nav, denser inline controls, and a bottom action bar with `Template`, `Cancel`, and `Ok`.
- Reworked the settings panel layout again to more closely match the reference proportions and control styling, including an icon-led sidebar, corrected left-aligned checkbox rows, tighter TradingView-like spacing, and larger square color pickers.
- Tightened the main shell chrome against `docs/UI/01 TradingView main UI.png`, refining the top bar density, left drawing rail width and icon treatment, right rail spacing and divider, and the bottom range strip with a TradingView-style `Go to` control.
- Added an optical spacing pass for the main shell UI, reducing glyph sizes inside consistent hitboxes and rebalancing cluster gaps, separators, rail spacing, and footer alignment so the top bar, drawing rail, right rail, and range strip feel closer to TradingView's spacing rhythm.
- Standardized the shell chrome with shared tokens for icon sizes, control hit areas, font sizes, divider spacing, and icon contrast, so future top/left/right/bottom UI adjustments can stay visually consistent from one place.
- Strengthened the shell chrome standard after screenshot review, increasing shared icon sizes and darkening the common chrome text/icon tones so faint controls in the timeframe strip, left rail, right rail, and footer remain visible on the white background.
- Fixed a break in the chrome icon standard by adding per-icon optical normalization for glyphs with mismatched internal padding and viewboxes, so controls like `Go to`, `Settings`, `Watchlist`, `Alerts`, and `Help` now render at a more consistent apparent size.
- Finalized a coordinated shell polish pass against the `docs/UI` references, tightening the top bar, trade cluster, left rail, right rail, and footer proportions together so the shared chrome standard is based on the actual TradingView rhythm rather than incremental one-off tweaks.
- Rebalanced the shell chrome again against the latest screenshot comparison, widening toolbar hit areas while shrinking icon glyphs, softening dividers, and reducing top and bottom text contrast so the left rail, right rail, top meta text, and footer align more closely with TradingView's lighter spacing and typography.
- Restricted the TradingView-matching pass back to chrome-only surfaces after screenshot review, removing the in-chart HUD mount so only the top, bottom, left, and right bars are affected.
