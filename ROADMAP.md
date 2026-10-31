# Bezgram roadmap

This roadmap describes the intended order of work. A checkbox means the work is implemented and verified, not merely planned.

## Stage 0 — Repository groundwork

- [x] Create the public repository
- [x] Document the product direction
- [x] Compare active Android client candidates with the AyuGram / exteraGram source line
- [ ] Add contribution, security, and privacy guidance
- [ ] Add license and attribution files for the selected source base

## Stage 1 — Finalise the upstream base

- [x] Check the current Telegram Android source line
- [x] Check the status and age of AyuGram4A and exteraGram
- [x] Review a current fork that carries exteraGram / Telegram Monet lineage
- [ ] Choose and pin the exact upstream commit
- [ ] Review all required Git submodules and their licenses
- [ ] Audit Firebase, Google Play services, analytics, crash reporting, and other external services
- [ ] Identify required API credentials, build configuration, signing, and fork-specific integration points
- [ ] Decide which AyuGram features can be ported safely and maintainably

**Exit condition:** the selected source can be built from a clean checkout, every submodule and third-party component is accounted for, and no upstream maintainer credentials or service configuration are reused.

## Stage 2 — First reproducible Android build

- [ ] Import the selected upstream source while preserving history and notices where practical
- [ ] Set a unique Bezgram application ID and app label
- [ ] Remove upstream service configuration and credentials from the build
- [ ] Implement or replace fork-specific integration points properly
- [ ] Build a debug APK from a clean checkout
- [ ] Install it on a test device and verify sign-in, chats, media, and notifications

**Exit condition:** a clean checkout can produce an installable debug APK using documented, maintainer-provided configuration.

## Stage 3 — Bezgram identity

- [ ] Create the app icon and launcher assets
- [ ] Apply Bezgram name, package identity, and application metadata
- [ ] Add an About screen with upstream credits and license information
- [ ] Establish a consistent icon and typography system

## Stage 4 — Two UI systems

- [ ] Design a UI-style setting: Material 3 / Telegram Classic
- [ ] Store the selected style persistently
- [ ] Define shared semantic colors, dimensions, typography, and component contracts
- [ ] Implement Material 3 navigation, settings, dialogs, menus, and controls
- [ ] Implement Telegram Classic equivalents
- [ ] Ensure style switching does not reset chats, preferences, or account state
- [ ] Test both styles in light and dark modes

**Exit condition:** switching styles changes the component presentation across the app without changing application behavior or user data.

## Stage 5 — Features and polish

- [ ] Audit existing AyuGram / exteraGram features for compatibility
- [ ] Select the first Bezgram feature set
- [ ] Add feature toggles with clear descriptions
- [ ] Review permissions, background work, notifications, and battery use
- [ ] Add accessibility checks and small-screen / large-screen testing
- [ ] Test upgrades and settings migration

## Stage 6 — Releases

- [ ] Add CI for debug builds and automated checks
- [ ] Document reproducible release builds
- [ ] Publish checksums and source links with each release
- [ ] Document signing-key ownership and backup expectations
- [ ] Publish a changelog and security reporting process

---

## Milestone 1

The first meaningful milestone is **a clean, reproducible Bezgram debug APK** built from this repository with Bezgram-owned configuration. UI redesign comes after the foundation builds reliably.
