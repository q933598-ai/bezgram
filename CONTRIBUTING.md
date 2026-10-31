# Contributing to Bezgram

Thank you for taking an interest in Bezgram.

Bezgram is an Android Telegram client project. Privacy, transparent behavior, and license compliance are important project requirements.

## Branch workflow

- `main` is the stable branch. It should contain only changes that have been reviewed and tested.
- `test` is the long-lived development and integration branch. Ongoing implementation, experiments, dependency changes, and feature work should go here first.
- Do not push experimental work directly to `main`.
- Promote a tested set of changes from `test` to `main` through a pull request.
- A pull request should describe what changed, how it was tested, and any known limitations.

Until a first reproducible debug APK exists, `test` is the active working branch.

## Before contributing code

1. Read the project [README](README.md), [roadmap](ROADMAP.md), and privacy requirements in [docs/PRIVACY_AND_SECURITY.md](docs/PRIVACY_AND_SECURITY.md).
2. Preserve upstream copyright notices and license files when modifying or importing code.
3. Do not copy API IDs, signing keys, Firebase files, tokens, or other credentials from upstream projects.
4. Do not add analytics, advertising, crash-upload services, or undocumented network requests.
5. Document new permissions, dependencies, endpoints, and external services.
6. Do not describe a feature as Telegram-policy-compliant unless its behavior has been checked against the current official API Terms.

## Testing

For each change, record the build or test command that was run and whether it passed. If a change cannot yet be tested, state that clearly in the pull request.

## Security and privacy reports

Do not publish exploitable vulnerabilities, private account data, access tokens, signing material, or other secrets in public issues. A private security-reporting contact will be added before public releases.
