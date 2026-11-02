# Encrypted Form Draft Recovery - Project Scope

Build a browser extension that recovers deliberately enabled form drafts after refreshes, crashes, or session expiry. Capture visibility, encrypted local versions, and explicit restoration are essential to earning user trust.

## Users and problem

Students, applicants, support agents, and writers can lose substantial work when a web form fails to save. Start with Chrome and Edge desktop users completing long textareas and common editable fields on sites they explicitly allow.

## MVP workflow

1. Create and unlock a local encrypted vault.
2. Enable capture for a chosen site and see a persistent capture indicator.
3. Type into supported fields while the extension keeps versioned checkpoints.
4. Return after an interruption, inspect available versions, and preview a restore.
5. Restore selected fields, copy text manually if matching is uncertain, or delete drafts.

## Included in the MVP

- An explicit site allowlist, optional per-site permissions, pause controls, and a visible recording indicator.
- Debounced version capture for textareas and explicitly supported non-sensitive text inputs.
- A tested basic contenteditable adapter; mark unsupported rich editors clearly rather than implying universal support.
- Stable field fingerprints combining origin, route, labels, field attributes, and structure with confidence scoring.
- Timestamped version history, text differences, field-level restore previews, and a safe copy-to-clipboard fallback.
- WebCrypto-based encrypted IndexedDB storage, automatic locking, configurable retention, and automatic expiry.
- Per-site and full-vault deletion plus an encrypted portable export/import path.

## Deferred features

TinyMCE, Quill, CKEditor, closed shadow roots, cross-origin iframes, complex SPA-specific controls, Firefox support, templates, and user-owned cloud sync need later adapters and explicit compatibility testing. Credential capture, background capture on unapproved sites, automatic submission, and silent restoration are outside scope.

## Proposed architecture and data

Use TypeScript, Manifest V3, WebCrypto, IndexedDB, and a small extension UI. Separate site consent, field eligibility, capture adapters, encryption/key lifecycle, version storage, field matching, and restore execution. Content scripts communicate only with the vault components needed for the approved site.

Persist SitePolicy, FieldFingerprint, DraftVersion, VaultMetadata, and RetentionPolicy entities. Keep sensitive draft payloads encrypted at rest and minimize unencrypted matching metadata. Use authenticated encryption with unique nonces and a versioned key-derivation design. The key lifecycle must support deliberate capture while unlocked and stop capture when locked without writing plaintext buffers to disk.

## Privacy, permissions, and cost

Exclude password, payment, one-time-code, hidden, and user-blocked fields before reading or persisting their values. Use type, autocomplete, labels, and site rules conservatively; disable capture for ambiguous sensitive fields. Sensitive-field detection cannot guarantee classification of every custom form, so the user controls exactly which sites are enabled. Keep private browsing disabled by default and never log field contents.

Draft capture and recovery must work locally without an account, analytics, paid inference, or developer-hosted server. An encrypted vault protects stored drafts; it does not make a compromised browser or hostile page safe.

## Acceptance criteria

- Approved supported fields recover their saved versions after refresh, tab close, browser restart, and session expiry.
- Disallowed sites and the sensitive-field test matrix produce no persisted draft payloads.
- Locking the vault or revoking site access stops capture, and the indicator reflects the change.
- Database inspection and diagnostic logs reveal no plaintext draft bodies.
- Wrong passwords and tampered ciphertext fail without altering valid drafts.
- Low-confidence field matching requires user selection; restore never submits a form or silently overwrites text.
- Retention expiry and explicit deletion remove the relevant stored versions and update the UI.
- Export/import restores version history in a clean profile; failed imports preserve existing drafts.

## Delivery and validation

| Phase | Deliverable | Exit condition |
|---|---|---|
| 1 | Consent, field eligibility, and key lifecycle | Capture is visible and the exclusion matrix passes. |
| 2 | Version history and recovery | Standard supported forms recover after interruption. |
| 3 | Basic editor support, export, and pilot | Users understand access and complete a successful recovery. |

Budget 2-4 solo-developer weeks for the basic version as a planning estimate; rich-editor coverage and security review can extend it. Measure permission acceptance, installation-to-first-recovery, successful restores, and repeated use. Publish the capture engine for inspection and plan an independent security review before broad adoption. Later paid options may include longer history, templates, and user-owned encrypted sync.

## Scope basis

This scope is derived from the supplied report, "Recent Verified User Pain Points Suitable for Zero-Backend SaaS, Mobile Apps, and Browser Extensions" (research window ending November 3, 2026). Market demand, estimates, and monetization are hypotheses to validate; the report's internal citation markers are not independently verified references. This repository starts with this scope document only; implementation has not started.
