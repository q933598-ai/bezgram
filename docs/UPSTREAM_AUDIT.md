# Upstream audit

**Checked:** 31 October 2026  
**Purpose:** choose a maintainable, open-source Android base for Bezgram. This is a source-selection note, not a legal opinion.

## Summary

| Project | Latest checked branch head | Status | What it means for Bezgram |
|---|---|---|---|
| [Telegram Android](https://github.com/DrKLO/Telegram) | `master` — `3de37c8`, 25 Oct 2026 | Active | Current upstream protocol and Android implementation baseline |
| [Cherrygram](https://github.com/arsLan4k1390/Cherrygram) | `main_Reproducible_Builds` — `f7d7a9a`, 29 Oct 2026 | Active | Preferred candidate: current Telegram base plus selected client/UI changes; credits exteraGram and Telegram Monet |
| [Nagram](https://github.com/NextAlone/Nagram) | `main` — `87863c8`, 20 Oct 2026 | Active | Alternative feature-rich client; based on NekoX and GPL-3.0 |
| [AyuGram4A](https://github.com/AyuGram/AyuGram4A) | `rewrite` — `7013145`, 28 Jul 2023 | Not archived, but branch is old | Keep as a source of feature ideas and selectively portable code; not a safe default base |
| [exteraGram](https://github.com/exteraSquad/exteraGram) | `main` — `6f78031`, 2 Jun 2023 | Archived | UI/features reference only unless a newer maintained successor is found |

## Why Cherrygram is the preferred candidate

- It is an unofficial fork of the original Telegram Android app.
- Its current branch follows Telegram Android version 13.1.1 and was updated on 29 October 2026.
- Its README explicitly credits exteraGram, OwlGram, Telegraher, and Telegram Monet.
- Its reproducible-build branch documents Android Studio 2025.1.4, Android SDK 36, and NDK 27.2.12479018.
- Its checked-in license is GNU GPL version 2. This is consistent with the planned GPL-2.0-or-later direction, subject to checking all imported files and submodules individually.

This does **not** mean Cherrygram can be copied blindly. Its build references Firebase Messaging / Datatransport / App Indexing, Google Play services, Firebase Analytics / Crashlytics, and a Generative AI SDK. These need a dependency and network-behavior audit. Bezgram should not silently reuse Cherrygram's Firebase project, signing material, API keys, bot tokens, or release infrastructure.

## Why not use AyuGram4A as the whole base immediately?

The checked `rewrite` branch has its latest commit dated 28 July 2023. Its build files declare Android Gradle Plugin 7.4.2, Gradle 7.5.1, compile / target SDK 33, Java 11, and NDK 21.4.7075529. The repository also defines an `AyuGram4A_proprietary` Git submodule. Its README says independent forks must implement `AyuMessageUtils` and `AyuHistoryHook`, replace Google Services configuration, and supply their own API / signing configuration.

These are solvable problems, but starting there would mean first reviving an older build and auditing a separate submodule before Bezgram-specific work can begin.

## Why keep Nagram as an alternative?

Nagram is actively updated and documents built-in Material Design themes, many Nekogram/NekoX-derived customisations, and an optional build without Google Services. However, it is based on NekoX, uses GPL-3.0, and brings a large set of extra proxy and client features that Bezgram would need to review. It remains a useful comparison point, not the preferred baseline at this stage.

## Proposed approach

1. Start from the pinned Cherrygram reproducible-build revision if its full source and submodules pass the dependency / license review.
2. Keep a clear record of the exact upstream commit and all submodule SHAs.
3. Remove or replace upstream telemetry, analytics, and service configuration unless Bezgram explicitly needs a feature and the data flow is documented.
4. Port only specific AyuGram features that are compatible with the selected base; preserve source notices and document their origin.
5. Build a clean debug APK before starting the broad UI redesign.
6. Implement Material 3 and Telegram Classic as separate presentation layers over shared chat / account logic. Do not fork the message, network, or storage behavior for each style.

## References

- [Telegram Android master](https://github.com/DrKLO/Telegram/tree/master)
- [Cherrygram reproducible-build branch](https://github.com/arsLan4k1390/Cherrygram/tree/main_Reproducible_Builds)
- [Nagram](https://github.com/NextAlone/Nagram)
- [AyuGram4A](https://github.com/AyuGram/AyuGram4A/tree/rewrite)
- [exteraGram](https://github.com/exteraSquad/exteraGram)
