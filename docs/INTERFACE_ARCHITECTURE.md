# Bezgram interface architecture

## Goal

Bezgram has two selectable interface styles:

1. **Material 3** — a modern Android-native presentation with Material components, dynamic color support, and Material motion.
2. **Telegram Classic** — a familiar Telegram-style presentation with its own controls, spacing, and navigation.

This is a presentation choice, not two separate applications.

## Core rule: one app, two presentation layers

Both styles must use the same:

- Telegram account and session state
- Chats, messages, folders, and drafts
- Network and MTProto implementation
- Media, downloads, and notifications
- Local databases and user preferences
- Feature logic and privacy controls

Only the presentation layer changes. Switching styles must never migrate, duplicate, clear, or re-create account data.

## Style setting

The appearance settings should expose a clear selector:

- Material 3
- Telegram Classic

The selected value is a persistent Bezgram preference. It must not be stored inside a theme color setting, because UI style and theme palette are independent choices.

The first implementation should define an explicit enum or equivalent stable identifiers, rather than storing translated display names. Example identifiers:

- `material3`
- `telegram_classic`

Unknown values from a future or older build must safely fall back to the default style.

## Theme preferences

### Material 3

- Light / dark / follow system
- Dynamic Android colors where available
- Bezgram color palettes as a fallback
- Material typography, shape, spacing, and motion tokens

### Telegram Classic

- Light / dark / follow system
- Telegram-like palette and controls
- Independent accent and background colors
- Classic iconography and spacing

Both styles should support a consistent accessibility text scale. A user changing the UI style should not lose their palette, font size, or other unrelated preferences.

## Component strategy

Introduce a small set of Bezgram-owned presentation boundaries for high-value shared surfaces:

- Settings rows and groups
- Switches, checkboxes, radio choices, and sliders
- Dialogs and bottom sheets
- Navigation surfaces
- Menus and contextual actions
- Empty states and informational cards

Each surface should have a Material 3 and Telegram Classic implementation backed by the same setting or action.

Do not build a second copy of chat, media, or account logic. Do not mechanically wrap every upstream view before understanding how it is used. Migrate screen-by-screen, starting with Appearance and Settings, then navigation and chat surfaces.

## Applying a style change

Preferred behavior:

1. Save the selected style.
2. Recreate or rebind the current presentation safely.
3. Return the user to the same screen and scroll position where practical.
4. Preserve drafts, open chats, media playback, and active account state.

If a particular upstream screen cannot be safely refreshed in place, show a clear restart-required message rather than risking state loss.

## Acceptance criteria

The first UI-style milestone is complete only when:

- The selector is available in Settings → Appearance.
- The choice persists after force-stop and relaunch.
- At least the Appearance and main Settings screens visibly change component style.
- Both styles work in light and dark modes.
- Changing style does not clear chats, accounts, drafts, folders, or other app data.
- No screen displays mixed styles accidentally.
- Accessibility font scaling remains usable in both modes.
