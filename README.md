# Bezgram

**An open-source Android Telegram client with a choice of interface styles.**

Bezgram takes inspiration from the desktop freshGram idea and brings it to Android: a feature-rich Telegram client where the user can choose how the app looks instead of being locked into one visual style.

> **Project status: bootstrap / planning.** The repository is being prepared. There is no installable APK yet.

## Vision

Bezgram is planned around three pillars:

- **Choice of interface:** switch between a Material Design 3 experience and a Telegram Classic-style experience.
- **Useful client features:** add carefully selected quality-of-life and privacy improvements, with every feature checked for security and Telegram API compatibility.
- **Transparency:** public source code, documented dependencies and builds, minimal permissions, and no hidden telemetry.

## Privacy-first commitments

Bezgram is intended to have no analytics, advertising SDKs, usage telemetry, hidden tracking, or third-party crash uploads by default. Extra local data storage and any external service must be documented, and optional diagnostics must require explicit consent.

We will publish Bezgram-written source code, build scripts, dependency and license information, and the known network endpoints. We will not reuse upstream API credentials, Firebase projects, signing keys, or service tokens.

**Important Telegram limitation:** ordinary Telegram Cloud Chats are not end-to-end encrypted; Telegram Secret Chats are end-to-end encrypted and device-specific. Bezgram cannot change this distinction or audit Telegram's private server implementation. See [Privacy and Security](docs/PRIVACY_AND_SECURITY.md).

Bezgram will follow Telegram's current API terms. In particular, we will not implement a Ghost Mode that falsifies read receipts, online status, or typing status, and we will not suppress official Sponsored Messages in channels. See the [Telegram API Terms](https://core.telegram.org/api/terms).

## Interface styles

The style switch should change the actual UI components — navigation, settings, dialogs, menus, buttons, and other surfaces — not merely recolor the same screen.

### Material Design 3

- Material 3 components and motion
- Android dynamic colors (Monet) where supported
- Carefully designed light and dark schemes
- Consistent spacing, shapes, typography, and icons

### Telegram Classic

- A familiar Telegram-style layout
- Classic navigation and component behavior
- Its own light and dark appearance

The selected style should persist across restarts. Theme colors and other personalisation options should remain independently configurable.

## Planned feature areas

- Privacy controls that respect Telegram's protocol and API requirements
- User-controlled local message history, with encryption and clear deletion controls
- User-defined chat and message filters that do not interfere with mandatory Telegram features
- Expanded media, download, and playback controls
- More gestures and interface customisation
- Optional integrations with explicit disclosure and consent

Feature availability will depend on what can be maintained safely and reliably on Android. No feature is considered implemented until it exists in the code and has been tested.

## Technical direction

The first idea was to use AyuGram4A directly, but the source audit showed that its `rewrite` branch is from 2023, uses an older Android toolchain, and includes a separate proprietary submodule. exteraGram itself is archived. Those projects remain useful references for feature ideas, but they are not an ideal unreviewed foundation for a new Android client.

The **current preferred base candidate** is [Cherrygram's `main_Reproducible_Builds` branch](https://github.com/arsLan4k1390/Cherrygram/tree/main_Reproducible_Builds). It follows the current Telegram Android source line and explicitly credits exteraGram and Telegram Monet. Bezgram can then bring over selected compatible features after security, license, and API-policy review.

This is still a candidate, not an imported codebase. Cherrygram's build references Firebase and Google Play services; those dependencies and their data flows must be audited before choosing what to keep, replace, or remove. See [the upstream audit](docs/UPSTREAM_AUDIT.md).

## Relevant documentation

- [Privacy and Security requirements](docs/PRIVACY_AND_SECURITY.md)
- [Upstream source audit](docs/UPSTREAM_AUDIT.md)
- [Two-style interface architecture](docs/INTERFACE_ARCHITECTURE.md)
- [Development roadmap](ROADMAP.md)

## Relevant upstream projects

- [Cherrygram](https://github.com/arsLan4k1390/Cherrygram)
- [AyuGram4A](https://github.com/AyuGram/AyuGram4A)
- [exteraGram](https://github.com/exteraSquad/exteraGram)
- [Telegram for Android](https://github.com/DrKLO/Telegram)

Bezgram is an independent, unofficial third-party client. It is not affiliated with or endorsed by Telegram.

## Development status

- [x] Public repository created
- [x] Product direction documented
- [x] First upstream comparison completed
- [x] Privacy and security requirements documented
- [ ] Select and pin the final upstream source revision
- [ ] Audit build requirements, licenses, submodules, and third-party services
- [ ] Import and build the Android project
- [ ] Establish Bezgram application ID, branding, and build variants
- [ ] Implement the interface-style architecture
- [ ] Add Material Design 3 interface
- [ ] Add Telegram Classic interface
- [ ] Implement and test privacy requirements
- [ ] Set up public CI, release checks, and build transparency

## Building

Build instructions will be added after the Android source tree and required configuration have been established. Do not use another project's API keys, signing keys, Firebase configuration, or other private credentials in a Bezgram build.

## Contributing

Contributions, bug reports, and design discussions are welcome. Please read the upstream licenses and notices before reusing code or assets. Changes derived from GPL-licensed code must preserve the applicable copyright and license notices.

## License

The final license and attribution files will be committed alongside the selected source base. The preferred baseline is GPL-2.0-or-later; third-party components and assets may have separate licenses, which must be documented individually.
