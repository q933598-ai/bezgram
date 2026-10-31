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

The current preferred technical base is [the official Telegram Android source](https://github.com/DrKLO/Telegram). We will pin an exact commit, review its submodules and dependencies, then build a clean Bezgram debug version before redesigning the interface.

[Cherrygram](https://github.com/arsLan4k1390/Cherrygram) remains an important source of UI ideas and selected features, but it will not be copied wholesale: its current reproducible-build branch includes analytics, crash reporting, remote configuration, and extra external services that need a separate review.

[AyuGram4A](https://github.com/AyuGram/AyuGram4A) and [exteraGram](https://github.com/exteraSquad/exteraGram) remain references for individual ideas and compatible code, subject to maintenance, source, license, and Telegram API review.

See [the upstream audit](docs/UPSTREAM_AUDIT.md).

## Relevant documentation

- [Privacy and Security requirements](docs/PRIVACY_AND_SECURITY.md)
- [Upstream source audit](docs/UPSTREAM_AUDIT.md)
- [Two-style interface architecture](docs/INTERFACE_ARCHITECTURE.md)
- [Development roadmap](ROADMAP.md)
- [Contribution and branch workflow](CONTRIBUTING.md)

## Relevant upstream projects

- [Telegram for Android](https://github.com/DrKLO/Telegram)
- [Cherrygram](https://github.com/arsLan4k1390/Cherrygram)
- [AyuGram4A](https://github.com/AyuGram/AyuGram4A)
- [exteraGram](https://github.com/exteraSquad/exteraGram)

Bezgram is an independent, unofficial third-party client. It is not affiliated with or endorsed by Telegram.

## Development status

- [x] Public repository created
- [x] Product direction documented
- [x] First upstream comparison completed
- [x] Privacy and security requirements documented
- [x] Initial project license file added (GPL-3.0)
- [x] `test` branch established as the development branch
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

Contributions, bug reports, and design discussions are welcome. Work on the `test` branch first; keep `main` for changes that have been reviewed and tested. Please read upstream licenses and notices before reusing code or assets.

## License

Bezgram's own project code is licensed under **GNU GPL v3.0** (see [LICENSE](LICENSE)).

Telegram's official Android application is published as **GNU GPL version 2 or later**. The "or later" permission allows eligible Telegram code to be used under GPLv3. Individual upstream files, submodules, libraries, and assets still need to be checked separately; a component marked GPL-2.0-only or carrying another incompatible license must not be silently included in a GPL-3.0-only combined work.

Preserve all applicable upstream copyright notices and license texts. See [GNU's GPL compatibility FAQ](https://www.gnu.org/licenses/gpl-faq.en.html) and the [upstream audit](docs/UPSTREAM_AUDIT.md).
