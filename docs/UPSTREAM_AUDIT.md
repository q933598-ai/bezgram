# Upstream audit

**Checked:** 31 October 2026  
**Purpose:** choose a maintainable, open-source Android base for Bezgram. This is a source-selection note, not a legal opinion.

## Summary

| Project | Latest checked branch head | Status | Current assessment |
|---|---|---|---|
| [Telegram Android](https://github.com/DrKLO/Telegram) | `master` — `3de37c8`, 25 Oct 2026 | Active | Best candidate for a privacy-first technical baseline, pending a clean build and dependency audit |
| [Cherrygram](https://github.com/arsLan4k1390/Cherrygram) | `main_Reproducible_Builds` — `f7d7a9a`, 29 Oct 2026 | Active | Strong feature and UI reference; do not use unchanged as the privacy baseline |
| [Nagram](https://github.com/NextAlone/Nagram) | `main` — `87863c8`, 20 Oct 2026 | Active | Alternative feature-rich client; based on NekoX and GPL-3.0 |
| [AyuGram4A](https://github.com/AyuGram/AyuGram4A) | `rewrite` — `7013145`, 28 Jul 2023 | Not archived, but branch is old | Keep as a source of feature ideas and selectively portable code; not a safe default base |
| [exteraGram](https://github.com/exteraSquad/exteraGram) | `main` — `6f78031`, 2 Jun 2023 | Archived | UI/features reference only unless a newer maintained successor is found |

## Telegram Android as the proposed base

The official Telegram Android repository is actively maintained and its checked revision is current with version 13.1.1. Its build guide documents Android SDK 36 and NDK 27.2.12479018.

### License compatibility

Telegram's official applications page identifies Telegram for Android as **GNU GPL v. 2 or later**. Several checked Java source headers also say "GNU GPL v. 2 or later". This is important: GPL-2.0-or-later permits choosing GPLv3 for a combined work, unlike GPL-2.0-only. Bezgram's own `LICENSE` currently selects GPL-3.0.

This does not automatically relicense every dependency or submodule in Telegram's source tree. Before importing the project, each submodule, library, binary, font, image, and other asset needs an inventory. A component marked GPL-2.0-only or otherwise incompatible must not be included in a GPL-3.0-only combined work without additional permission; replace it, exclude it where possible, or obtain the required permission. Preserve original copyright and license notices.

References: [Telegram Apps — Android source license](https://telegram.org/apps/download?setln=en), [GNU GPL FAQ — GPLv2 and GPLv3 compatibility](https://www.gnu.org/licenses/gpl-faq.en.html).

### Privacy and build audit

It is **not automatically privacy-clean or Google-free**:

- The main Gradle module includes Firebase Messaging, Firebase Datatransport, Firebase App Indexing, and a range of Google Play services dependencies. Each dependency must be mapped to a feature before keeping it.
- The checked debug and release manifests explicitly set `firebase_analytics_collection_deactivated=true` and `google_analytics_adid_collection_enabled=false`.
- The checked source search found Firebase Crashlytics usage in the HockeyApp application flavor. Bezgram should not include that flavor or crash-upload path without an explicit, documented decision.
- The official build also expects its own build configuration and push-notification setup. Bezgram must use its own identifiers and credentials and should document whether push notifications require Google services.

This makes official Telegram a promising starting point: it avoids inheriting Cherrygram's additional analytics and remote-configuration behavior, but still needs a proper dependency and network audit.

## Cherrygram: useful donor, not clean as-is

The checked `main_Reproducible_Builds` branch follows Telegram Android version 13.1.1 and was updated on 29 October 2026. Its README credits exteraGram, OwlGram, Telegraher, and Telegram Monet. The checked-in license is GNU GPL version 2. Its README documents Android Studio 2025.1.4, Android SDK 36, and NDK 27.2.12479018.

Further inspection found several privacy concerns that are more substantial than a simple list of optional dependencies:

- `TMessagesProj/build.gradle` includes Firebase Analytics, Firebase Crashlytics, Firebase Messaging / Datatransport, Google Play services, and the Google Generative AI SDK.
- `FirebaseAnalyticsHelper` enables Firebase Analytics collection during initialization. The helper also supports event tracking.
- `FirebaseCrashlyticsHelper` sends crash reports and attaches account-related identifiers and public-name information to crash reports.
- `CherrygramFirebaseConfig` initializes Firebase and enables Crashlytics collection.
- `RemoteConfigHelper` periodically downloads configuration from Cherrygram-controlled GitLab, GitHub, and jsDelivr endpoints. The configuration includes feature flags and advertising / partner-related options.
- The README asks builders to create Firebase applications and add a `google-services.json` file. Bezgram must never reuse Cherrygram's project configuration, keys, signing material, tokens, or release infrastructure.

These components may be removable, but doing so requires tracing every call site and removing or replacing dependent features. Cherrygram remains valuable as a source of UI ideas and individual features after review; it should not be copied wholesale into a privacy-first release.

## Why not use AyuGram4A as the whole base immediately?

The checked `rewrite` branch has its latest commit dated 28 July 2023. Its build files declare Android Gradle Plugin 7.4.2, Gradle 7.5.1, compile / target SDK 33, Java 11, and NDK 21.4.7075529. The repository also defines an `AyuGram4A_proprietary` Git submodule. Its README says independent forks must implement `AyuMessageUtils` and `AyuHistoryHook`, replace Google Services configuration, and supply their own API / signing configuration.

These are solvable problems, but starting there would mean first reviving an older build and auditing a separate submodule before Bezgram-specific work can begin.

## Why keep Nagram as an alternative?

Nagram is actively updated and documents built-in Material Design themes, many Nekogram/NekoX-derived customisations, and an optional build without Google Services. However, it is based on NekoX, uses GPL-3.0, and brings a large set of extra proxy and client features that Bezgram would need to review. It remains a useful comparison point, not the preferred baseline at this stage.

## Proposed approach

1. On Bezgram's `test` branch, pin the checked official Telegram Android commit and establish a clean, reproducible debug build before importing a large client fork.
2. Inventory the official base's Gradle dependencies, submodules, permissions, manifests, and external network destinations. Decide which features need Google services and document the trade-offs.
3. Keep Cherrygram as a feature and UI donor. Port selected non-network features only after reviewing their source, dependencies, data flow, and licenses.
4. Keep AyuGram4A as a reference for selected features; do not treat its Ghost Mode implementation as policy-compliant.
5. Preserve upstream copyright notices and license files. Record the exact source commit and every submodule SHA.
6. Give Bezgram its own package identity, API credentials, signing configuration, and build configuration. Never reuse credentials from upstream projects.
7. Build and test the clean Bezgram debug APK before starting the broad UI redesign.
8. Implement Material 3 and Telegram Classic as separate presentation layers over shared chat / account logic. Do not fork message, network, or storage behavior for each style.

**Base selection is still provisional.** The official Telegram source is the current preferred technical baseline for a privacy-first start, subject to a successful clean build and dependency audit. Cherrygram is currently the stronger feature/UI reference.

## References

- [Telegram Android master](https://github.com/DrKLO/Telegram/tree/master)
- [Telegram Android checked commit](https://github.com/DrKLO/Telegram/commit/3de37c8cfb039452be7398909461da6288babf15)
- [Telegram Apps page](https://telegram.org/apps/download?setln=en)
- [GNU GPL compatibility FAQ](https://www.gnu.org/licenses/gpl-faq.en.html)
- [Cherrygram reproducible-build branch](https://github.com/arsLan4k1390/Cherrygram/tree/main_Reproducible_Builds)
- [Cherrygram checked commit](https://github.com/arsLan4k1390/Cherrygram/commit/f7d7a9a77af2c556b66f5251dbb09e1a663b676f)
- [Nagram](https://github.com/NextAlone/Nagram)
- [AyuGram4A](https://github.com/AyuGram/AyuGram4A/tree/rewrite)
- [exteraGram](https://github.com/exteraSquad/exteraGram)
