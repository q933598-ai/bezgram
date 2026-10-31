# Bezgram privacy and security requirements

**Status: design requirements, not an audit certification.** These requirements must be implemented and tested before a public release. A statement in this document is a project commitment, not proof that the current repository or a future APK already satisfies it.

## Privacy promise

Bezgram should collect as little as possible, explain every external connection, and keep optional data sharing off by default.

The project must never describe Telegram itself as end-to-end encrypted by default. Bezgram is a third-party client for the Telegram network; it cannot change Telegram's server-side data practices or turn cloud chats into Secret Chats.

## Non-negotiable defaults

- No advertising SDKs, ad attribution, analytics, usage telemetry, fingerprinting, or hidden tracking in Bezgram.
- No third-party crash-reporting SDK enabled by default. The first release should ship without crash upload. A future diagnostic feature must be opt-in, explain exactly what it sends, redact account and message content, and identify the recipient/endpoint.
- No Bezgram-operated account, message, contact, or usage-data backend for the initial release.
- No automatic upload of local data, message history, contacts, media, logs, or diagnostics to a Bezgram server.
- No reuse of API credentials, Firebase projects, signing keys, bot tokens, keystores, or other secrets belonging to Telegram, Cherrygram, AyuGram, or any other upstream project.
- No hidden HTTP clients, WebViews, SDKs, background jobs, or native modules that contact undeclared third-party services.
- No permission is requested before it is needed by a user-triggered feature. Denial must not break unrelated functionality.
- All features that store extra data locally must have documented retention and deletion behavior.

## Telegram network and encryption limits

Bezgram connects to Telegram using Telegram's API. Users must be told clearly that they are using an unofficial third-party Telegram client.

Telegram distinguishes two relevant chat types:

- **Cloud Chats** (including ordinary private chats, groups, and channels) use client-server/server-client encryption and are stored in Telegram's cloud. They are **not end-to-end encrypted**.
- **Secret Chats** use end-to-end encryption and are device-specific; they are not available in Telegram's cloud.

Bezgram must preserve this distinction in its onboarding or privacy information. It must not claim that all chats are E2EE, that Bezgram controls Telegram's servers, or that changing the interface style changes the encryption model.

Telegram's server implementation is not part of this repository. Open-sourcing Bezgram can make the client and its own build process inspectable, but it cannot make Telegram's servers auditable by Bezgram contributors.

References:
- https://telegram.org/privacy
- https://telegram.org/faq
- https://core.telegram.org/api/terms

## Telegram API policy compatibility

Bezgram must follow the current Telegram API Terms of Service and Security Guidelines.

In particular:

- Do not implement Ghost Mode or otherwise falsify / suppress read receipts, online / last-seen state, or typing status.
- Do not prevent self-destructing content from disappearing or otherwise interfere with core Telegram behavior.
- If the client supports Telegram channels, preserve support for official Telegram Sponsored Messages and do not suppress or interfere with them.
- Obtain and use a Bezgram-owned Telegram `api_id`; never copy one from an upstream application.
- Clearly identify Bezgram as an unofficial third-party client and do not use Telegram's official logo or imply endorsement.

This means some features advertised by other forks may be unsuitable for Bezgram even if their source code is public. Each port must be checked against the API terms before implementation.

## Data-flow policy

Every feature that sends information outside the device must have an entry in a maintained data-flow inventory before it is enabled.

| Area | Required Bezgram behavior |
|---|---|
| Telegram login, messages, media, calls | Connect to Telegram endpoints required by the protocol. Explain that Telegram processes account and cloud-chat data according to its own policy. |
| Analytics / advertising | Not included. |
| Crash reports | Disabled / absent in the initial release. Any future opt-in report must show the recipient and data categories before sending. |
| Update checks | No device, account, chat, or usage identifiers. Document the endpoint and payload. Prefer user-triggered checks. |
| Push notifications | Audit each push provider and payload. Do not reuse upstream Firebase configuration. Document the provider and the data it receives; provide a no-Google-services option where technically feasible. |
| Translation, AI, bots, link previews, external media | Treat as external services. Make the destination clear and send only after a user action. Never use private messages to train or improve AI models. |
| Local message history / deleted-message archive | Store only after the user enables it. Keep it in app-private storage, protect encryption keys with Android Keystore where technically appropriate, provide clear retention / clear-all controls, and do not sync it to a third-party service by default. |
| Contact sync | Follow Telegram's permission and consent flow. No separate Bezgram upload or address-book copy. |
| Logs | Never log message bodies, phone numbers, auth tokens, API hashes, session keys, contact lists, or media contents. |

The table describes the intended design. The actual implementation must be checked with source review and network tests.

## Local data protection

- Keep authentication/session material in the upstream Telegram security model; do not create a parallel plaintext session store.
- Use Android Keystore-backed encryption for any new sensitive local archive or secret Bezgram preference data when feasible.
- Do not store secrets in source files, Gradle scripts, resources, build logs, or CI artifacts.
- Do not include personal data in crash traces, analytics events, bug-report templates, screenshots, or test fixtures.
- Give the user a way to clear any Bezgram-specific archive and local diagnostic data.
- Avoid extra copies of message history. A local history feature must explain what it saves, how much storage it uses, and how it is deleted.

## Open source and inspectability

The following must be public in the repository:

- All Bezgram-written application source code.
- Build scripts, Gradle configuration, CI workflows, patches, and release scripts.
- The exact upstream commit and Git submodule revisions used for each build.
- A third-party dependency inventory with versions and license identifiers (SBOM where practical).
- A list of network endpoints and what triggers each request.
- Permission declarations and the feature that needs each permission.
- Privacy and security design notes, changelog, and known limitations.

Do not add proprietary or closed-source application libraries when an open implementation is reasonably available. If a non-open component is unavoidable, document its purpose, publisher, version, license, data access, and why it is needed. Do not describe third-party components or Android / Google system services as Bezgram source code.

Keep source readable: meaningful names, small focused modules, comments for security-sensitive behavior, and tests for permission, storage, and network boundaries. Obfuscation must not be used to conceal application behavior. Standard release optimization may be considered later, with corresponding source and build instructions published.

## Build and release transparency

- Publish the complete corresponding source for every distributed build and preserve upstream license / copyright notices.
- Pin dependencies and submodules; review changes before updating them.
- Use public CI for checks and build provenance. Do not place signing keys or API secrets in the repository.
- Publish release checksums and source commit IDs.
- Aim for reproducible builds and document any non-deterministic parts. Do not claim bit-for-bit reproducibility until independent rebuilds verify it.
- Produce a dependency list / SBOM for releases where feasible.
- Review APK contents and permissions before release; compare the declared dependency and network inventory against the built artifact.

## Required privacy tests before release

- [ ] Verify no analytics or advertising SDK is packaged.
- [ ] Verify no crash-reporting or telemetry endpoint is contacted during normal use.
- [ ] Capture and review outbound connections on first launch, login, idle, chat, settings, media, and update-check flows.
- [ ] Check that no third-party endpoint receives message content, contacts, phone number, or account identifiers unless explicitly required by a documented user action.
- [ ] Test permission denial and revocation.
- [ ] Test local archive encryption, retention, and deletion.
- [ ] Check that logs and crash traces contain no secrets or message content.
- [ ] Review all bundled native libraries, AARs, JARs, submodules, and Gradle plugins.
- [ ] Verify release APK permissions and embedded services match the published inventory.
- [ ] Re-check Telegram API Terms before every major release.

## Reporting security issues

Until a dedicated private security contact is configured, do not publish exploit details or account/session data in public issues. A private reporting channel and response process must be established before the first public release.
