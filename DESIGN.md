# Aerogram desktop design contract

**A launcher for finding conversations; a window for reading one.** Most of the
glass belongs *around* the work. Keep the speaker, message and insertion point on
a quiet, continuous surface. This is a specification for native Qt Widgets, not
a skin, a screenshot recreation or a change of messaging protocol.

## Where the decisions come from

The filenames below identify the private `msn09/` reference set; the keys are
used throughout this contract. Reproduce relationships, not screenshot pixels,
sample content, commercial artwork, advertisements or legacy commands.

| Key / Aerogram surface | Exact reference filename(s) | Observation → decision |
|---|---|---|
| L · Launcher | `photo_1_2026-09-13_10-29-25.jpg` | Small portrait and name above a separate search strip; the white list takes the remaining height. Use that hierarchy for account, search and Telegram dialogs, not for ads or a contacts-only list. |
| W · Conversation window | `屏幕截图_20260913_103710.png`, `屏幕截图_20260913_103728.png` | A shallow command strip caps a broad sloping blue header. Maximizing adds reading space, not another conversation. |
| A · Avatar rail | `屏幕截图_20260913_103710.png`, `屏幕截图_20260913_103728.png` | Top portrait is adjacent to the peer name; bottom portrait stays beside the input when maximized. Keep those two anchors and the fine reflective rims. |
| T · Transcript | `屏幕截图_20260913_103710.png`, `屏幕截图_20260913_103728.png` | Each speaker label sits above an indented line on the same white ground. Separate speakers by type and space, not opposing filled shapes. |
| C · Composer | `屏幕截图_20260913_103710.png`, `屏幕截图_20260913_103728.png` | A bordered writing surface above a short row of tools. Both start at the transcript edge and remain together on resize. |
| S · Settings, profile and pickers | `屏幕截图_20260913_102843.png`, `photo_2_2026-09-13_10-29-25.jpg`, `屏幕截图_20260913_110148.png` | Category list beside a form; portrait opens profile actions; thumbnail selection is marked separately from the footer action. Retain Telegram sections, profile actions and preview/apply semantics. |
| M · Menus | `屏幕截图_20260913_103301.png`, `photo_3_2026-09-13_10-29-25.jpg` | Compact rectangular levels with aligned labels and a trailing submenu arrow. Keep the shape and navigation, not the manually selectable presence states. |
| I · Intro, QR and local passcode | `photo_1_2026-09-13_10-29-25.jpg`, `屏幕截图_20260913_102843.png` | Borrow the light identity band and the form's inset border/default action. These are **material references**, not depictions of Telegram authentication. |
| N · Notification and tray | `image_2026-09-13_10-45-33.png`, `image_2026-09-13_10-46-47.png`, `屏幕截图_20260913_103728.png` | The compact notices separate app title/close, thumbnail, summary and action. The maximized view has a distinct taskbar presence. Translate to a self-painted toast and original tray mark; no imitation sign-in broadcasts. |
| H · Theme tint | `屏幕截图_20260913_110917.png`, `屏幕截图_20260913_110148.png` | A warm toolbar replaces cyan without moving the controls; backgrounds are selectable. Geometry must survive a change of hue or wallpaper. |

## Window and identity rules

Measure the following at 100% in logical px. Values are `.style` metrics, not
painter literals. Scale the layout once; choose the compact variants by the
**available scaled content space**, not by a physical monitor resolution.

* **Launcher (L/M):** default **360×720**, minimum **320×480**. One list at
  *every* width; never allocate an empty history column. Stack account card
  (48 portrait, name, status, account switcher), full-width search and compact
  actions, then scrolling folders/dialogs. Preserve unread counts, pin, draft,
  archive, chat type and real empty/offline/search results. Reserve the badge
  width before eliding preview/name; put secondary actions into overflow before
  compressing the search field. Enter opens the selected result; Escape backs
  out of search. Opening a thread leaves the launcher and its scroll position intact.
* **Conversation (W/A/C):** default **800×600**, minimum **560×420**. Title
  controls above command strip and identity header; below, a leading rail and
  a single flexing history/composer column. At content width ≥720 reserve 128
  for the rail (96 portraits); at 640–719 reserve 80 (56 portraits). Below
  640, or if two frames plus their targets cannot fit vertically, remove the
  rail from layout. Always leave a named, keyboard-focusable portrait toggle
  in the header. In compact mode it opens an owned two-identity popover with
  the same profile actions, Escape returns focus; at wider sizes it toggles
  the in-flow rail. Never float a persistent rail over the text/IME rect.
  Keep header and input visible; the history receives the remaining height.
* **Owner is part of the address:** list, deep link, notification, reply jump
  and search open/activate the existing separate window for the same account,
  peer and thread/topic. A different account or topic is a different target.
  A launcher account switch does not rebind an open composer, draft, profile
  layer, media viewer or notification. Close a child independently of its owner.
* For private peers show actual peer above and sending account below. Use the
  existing userpic or initials. For groups/channels show the **chat identity**,
  with member/subscriber data only if available. In a forum, show the topic
  icon/title with the parent forum identity. Anonymous admins, channel posts,
  “send as”, hidden forward origins and Saved Messages keep their Telegram
  attribution. The lower account portrait is not a guarantee of authorship.
  Only show call, send, join, comment or profile actions the peer permits.
* Preserve native frame preference, drag, resize edges, system menu, double
  click, minimize, restore and close. Keep native caption controls out of
  client hit regions. The inactive state changes light, never geometry.

## Palette and paint

The table defines **semantic roles**, not new theme-file keys. The hex columns
are target sRGB swatches for Aerogram's built-in light/dark palettes. The seed
column names upstream roles to draw from on user themes. The six surface roles
are opaque stops; interpolate in sRGB, with no extrapolation. “All” contrast
below is the worst pair against any of those six stops (light / dark, rounded).
Normal text ≥4.5:1, large text and actionable boundaries ≥3:1. Highlights and
shadows are decoration, never the only boundary, label or keyboard cue.

| Role | Light | Dark | Seed / use | Reference |
|---|---|---|---|---|
| `paper` | #FFFFFF | #141E2B | `windowBg`; transcript, input, menu and form field | T/C/S/M |
| `mist` | #F4F9FD | #1B2B39 | `windowBg` + `lightButtonBgOver`; outer content, rail, toast base | L/A/N |
| `ice` | #D8ECF6 | #243948 | `windowBgOver`; toolbar foot, shaded panel, disabled face | C/S |
| `sky` | #B5E8F7 | #285069 | `windowBgActive` tinted toward paper; header middle, hover face | W/L/H |
| `lake` | #92CBDF | #305068 | `activeButtonBgOver` tinted toward paper; deepest glass/pressed stop | W/C/H |
| `fog` | #CAD7DF | #344855 | `windowBgOver` desaturated toward secondary ink; inactive chrome | W/S |
| `ink` | #122633 | #F2F8FC | `windowFg`; names, labels, writing, glyphs. All **8.76 / 7.93** | L/T/S |
| `subInk` | #334F60 | #D5E4EE | `windowSubTextFg`; time, last-seen, placeholder, disabled labels. All **4.87 / 6.53** | A/T/S |
| `action` | #004A6A | #B6E8FF | `windowActiveTextFg`; links, focus, story ring and actionable metadata. All **5.41 / 6.46** | L/A/M |
| `edge` | #36596C | #A1C5D8 | `inputBorderFg`; control/keyline only, not small text. All **4.22 / 4.65** | C/S/N |
| `online` | #0C573D | #9DE9BC | `boxTextFgGood`; live presence rim + labeled dot. All **4.84 / 5.99** | A |
| `error` | #842234 | #FFCED3 | `attentionButtonFg`; failure icon plus inline error. All **5.24 / 6.07** | I/S |
| `selection`, `onSelection` | #185A78, #FFFFFF | #A3D7EC, #102733 | `windowBgActive`, `windowFgActive`; selected text/list/nav pair only. **7.57 / 9.92** | S/M/T |
| `qrMark`, `qrField` | #000000, #FFFFFF | #000000, #FFFFFF | Isolated QR display island; both themes **21:1**, never tinted | I |

**Material recipes.** Each recipe inherits both palettes and the contrast of
its named ink/edge against its stops. For a specular line use `onSelection` in
light mode and `ink` in dark mode at 36% over the face; for a shadow use the
palette's `windowShadowFg` at 14%, offset down 2, fading to zero over 4. Clip
both outside text and state glyphs. Snap outer keylines and inner highlights
to adjacent **one-device-pixel** rows/columns; do not stack two outer borders
at seams. Corner radii: 4 for controls, 8 for portraits and toasts, square
for transcript and menu rows. Shadows have no hit area.

| Recipe | Paint, in order | Reference |
|---|---|---|
| `titleGlass` | `edge` perimeter, vertical `sky → lake`, inner top highlight, `ink` caption. Inactive `mist → fog`, no highlight. OS controls keep platform bounds and semantics. | W/S |
| `identityBand` | Active diagonal from physical upper left `lake → sky → mist` (0/45/100%); one broad diagonal facet confined to the trailing decorative space. Inactive `fog → mist`, no facet. Names sit on unadorned stops; no required desktop blur. | L/W/H |
| `commandFace` | Default vertical stops `paper, mist, ice, mist` at 0/48/50/100%; hover `paper, sky, ice, sky`; pressed `lake, sky, ice, ice` with inset top edge, no shadow. `edge` outline, `ink` label. Default action has `action` perimeter. Keyboard focus adds 2 logical px of `action` with a 1 px paper gap, even on hover/press. Disabled is flat `ice` with `subInk`, no glint or activation. Reserve focus space in every state. | C/S |
| `portraitRim` | Narrow mist bevel, `edge` keyline against the rail, specular top only; no gloss over the image. Online: `online` solid band/dot; offline, hidden or last-seen: `edge` and actual localized status, no inference. Group/channel: neutral frame and type icon; read-only: permission label/lock, not “offline”. A real story uses a separate `action` ring and story target; video uses the actual video renderer/play glyph. Preserve premium badges and reduced-motion behavior. | A/L |
| `writingWell` | `paper` field and transcript, `mist` surrounds, `edge` control border; no shadow under plain messages. Use `action` focus only for the focused control. Media/quote panels use `mist` inset with a leading `edge` rule. | T/C |
| `toolStrip` / `menuPanel` | `mist → ice` compact strip, hairline separator, icons in `ink`; menus opaque `paper` with `edge` perimeter, hover `ice`, selection pair for active keyboard row. Align icon/check, label, shortcut and submenu columns; open inward at screen edge. | C/M |
| `sectionShell` / `authShell` | `identityBand` above `mist` section surround, `paper` scrollable form, `commandFace` actions. Auth decoration is confined to brand header; secret fields and errors use a flat opaque ground. | S/I |
| `noticePanel` | Shallow `sky → mist` title, `mist` body, `edge` perimeter; compact framed portrait, two-line summary, close/options on separate targets. No shadows on text. | N |

## Geometry and interaction

* **Rhythm (L/W/S):** use 4 logical px as the spacing unit: 4 between glyph
  and label, 8 within a group, 12 panel inset, 16 between sections. Compact
  tools have 16 glyphs in ≥32×32 non-overlapping targets; grow rows to font
  height plus 8. Focus outlines do not change size, hit area or tab order.
* **Transcript (T/A):** start *both* directions at the same leading inset
  (12), reserve 16 for a marker, then place the sender and timestamp on the
  first baseline. Timestamp trails the header, wraps intact onto the next
  line when space is short. Body starts 4 below, at the sender's leading
  edge. Use actual message font metrics, 8 after each block. Incoming: open
  dot and author; outgoing: solid dot, actual sender and delivery/read/error
  glyph. Expose direction and state to accessibility independently of color.
  Date change: localized `subInk` label flanked by hairlines, 8 above/below.
  Keep unread divider distinct by explicit label; preserve search/date anchors.
  Wide windows leave trailing whitespace: cap plain reading width at 72
  average body glyph advances, not upstream's asymmetric bubble margin.
* **One geometry, including interaction:** subtract rail, insets and scrollbar
  once to obtain the viewport. For each element, measure the sender, time,
  marker, forward/reply, shaped text, media, reactions and inline buttons in
  element-local logical coordinates; their ordered rectangles plus padding
  define row height. Translate element origin by the list's actual scroll
  offset, and map RTL exactly once. Use that layout for painting, selection
  highlight/copy offsets, text-state, keyboard navigation, accessibility,
  context menus and link/media hits. Map pointer back by the inverse transform;
  a gap isn't a link. No shadow or oversize bubble hot zone. Reflow on edit,
  delete, font, resize or media load, retaining the visible message anchor.
* Keep actual click handlers: author → profile, time → existing message action,
  reply/quote → referenced message/thread, forward → *available* origin.
  Wrap long quotes with source and leading rule; don't invent hidden names.
  Lay captions, album, document progress, stickers/emoji, poll, buttons and
  reactions out at their element's measured width/height. Keep media aspect,
  interactive internal cards, selection, downloads, failure/retry and service
  rows. Never paint a second outer bubble or flatten a poll into text.
* **Composer (C):** flat multiline field above compact attachment/emoji/tools
  and a persistent send/record action. Grow writing area up to one third of
  the available column height, then scroll within it. Reply/edit/cancel strip
  belongs to the field; collapse secondary commands to a named overflow
  before hiding the primary action. Keep native IME caret/candidate anchor,
  paste, drop, formatting, drafts, bot-menu hold, Tab, Escape and configured
  Enter/Ctrl+Enter. Enforce permissions in the command, not just the paint.
* **Settings and profiles (S/M):** launcher opens long forms in an owned
  separate window. Leading category list (144) beside an expanding scrollable
  settings section; below 640 show one pane with a labeled category/back
  button, preserving focus and scroll. Keep existing privacy, proxy, language,
  theme, notifications, account and Forkgram sections. Mark thumbnail selection
  with a check and outline, not tint alone; preserve Apply/Cancel or immediate
  save as the section defines it. Confirmations and menus are owned by their
  originating window. Escape closes only the topmost layer and returns focus.
* **Intro/lock (I):** center one column of original Telegram steps, with
  scrollable content at small heights. Keep phone/QR switch, code, 2FA,
  recovery, proxy, retry and local unlock as distinct native actions. Use the
  QR renderer's quiet zone (at least four modules), integral device-pixel
  modules, no resampling. **Zero glass glare over QR codes, verification
  inputs or error text**. Never cover the QR with the brand mark. Replace all
  sensitive surfaces/previews while locked; never use profile pixels as the
  lock background. Errors remain readable and associated with their field.
* **Notifications (N):** style only application-painted notices. Respect OS
  notification choice, mute, permission, privacy, timeout and work mode. On
  lock remove sender, avatar and snippet when hidden, including stale queued
  content. Activate the correct account/thread on click; close only dismisses.
  Tray/taskbar use the original Aerogram mark with upstream unread/work-mode
  semantics, not a pasted OS icon or synthetic presence signal.

## Type, direction, scaling and theme changes

Use the client's configured UI family and Qt script fallback. At 100%, base
body/control 14 regular, sender/section label 14 semibold, timestamp/caption
13 regular, identity heading 18 semibold (L/T/S). Respect user font size; use
shaped line metrics, not fixed Latin ascent. Test mixed Han, kana, Arabic,
emoji, long localized actions and composition underlines. Let form labels
wrap and grow; elide list cells at grapheme boundaries, with the full identity
available in the profile/accessibility name. Do not letter-space CJK to imitate
the bitmap reference lettering.

At 100/150/200% size metrics via the existing style scale. A 32-unit target
is 32/48/64 in scaled logical layout; the actual `devicePixelRatio` determines
backing pixels separately, including fractional ratios. Round shared edges
once, not each adjacent width; target layout is not screen-capture scaling.
Let the OS frame and work area constrain windows; scroll/reflow instead of
cropping controls. Test minimum, default and maximized windows while moving
between displays, switching activation and returning to the original scale.

In RTL mirror leading rail/nav, row insets, submenu/back arrows, header
alignment, flow of toolbar groups and reply rule. Preserve Unicode bidi runs,
timestamps, phone numbers, URL text, media, avatars, logo, QR, physical light
direction and platform-owned caption control placement. Input direction follows
the user's text. Mirrored geometry is shared by focus, painting and hits.

`.tdesktop-theme` remains a palette and optional background/tiled image, not a
layout API. Resolve each role from the **current** window/chat palette; retain
user wallpaper choice, position and tiling. Show wallpaper around the reading
column; behind glyphs use a continuous opaque reading plane if contrast cannot
be guaranteed. No per-message backing bubbles. Preserve the existing chat
theme readability pass. Check resolved ink/edge against the worst composited
stop, including selection. If below threshold, reduce gloss/tint toward the
user's solid surface, then move foreground lightness toward black or white
until the threshold is met; do not reset the whole theme to Aerogram blue.

Paint cheap gradients directly. For a cached painted surface the minimum key
is **palette revision + UI scale + real DPR + logical size + state** (active,
hover, pressed, focused, disabled as applicable). Include direction, wallpaper
revision/position, avatar identity/version and story/video frame only when the
cached pixels depend on them. Invalidate on those changes; remeasure text on
font/language/direction changes. Release window-local subscriptions and caches
with their owner. Never share account-specific pixels across windows by size.

## Implementation boundary and brand

Use application-layer QPainter and app `.style` metrics with existing widget,
history element, session/window registry and theme lifetimes. Keep public
`RpWindow` / `SeparateTitleControls` and platform hit-testing seams. **No
`lib_ui → application` dependency, no submodule edits or pin changes.** No
global skin engine, new service, fake contact states or new theme format.

The mark in `Telegram/Resources/art/aerogram/` is a folded glass wing: a swept
outer edge, a pale upper vane and an azure keel. Use the SVG as master and
the transparent 16/32/64/256 PNGs at native pixel sizes. Preserve orientation
in RTL, generous clear corners and original colors in both themes. This
contract and the renders define appearance; they do not attest that any native
window, account flow or platform build has already passed QA.
