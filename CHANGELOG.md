# Changelog

## v2.0.0

### New skills (33 added)

alarmkit, app-clips, app-store-review, apple-on-device-ai, authentication,
background-processing, callkit-voip, cloudkit-sync, contacts-framework,
core-motion, core-nfc, coreml, device-integrity, energykit,
eventkit-calendar, healthkit, homekit-matter, ios-localization,
mapkit-location, metrickit-diagnostics, musickit-audio, natural-language,
passkit-wallet, pencilkit-drawing, permissionkit, photos-camera-media,
realitykit-ar, shareplay-activities, speech-recognition, swift-language,
swiftui-gestures, swiftui-layout-components, swiftui-navigation

### Skill refactors

- **swiftui-patterns** split into `swiftui-patterns`, `swiftui-navigation`, and `swiftui-layout-components` — each is now self-contained with no cross-references.
- **ios-security** split into `ios-security`, `authentication`, and `device-integrity` — each owns a clear domain boundary. `ios-security` covers Keychain/CryptoKit/data protection. `authentication` covers Sign in with Apple, passkeys, and biometric sign-in. `device-integrity` covers DeviceCheck and App Attest.
- All skills now self-contained with 4 or fewer reference files. No skill references another skill's files.

### Bundle restructure

v1.x had 6 themed bundles + 1 all-skills bundle. v2.0 has 8 themed bundles + 1 all-skills bundle.

Changes:
- `ios-framework-skills` (17 skills) split into `ios-app-framework-skills` (10) and `ios-data-framework-skills` (7).
- New `ios-ai-ml-skills` bundle created with `apple-on-device-ai`, `coreml`, `natural-language`, `speech-recognition`, `vision-framework`.
- AI/ML skills removed from `ios-engineering-skills` and `ios-platform-skills`.
- `ios-platform-skills` trimmed to 5 specialized platform integrations (HomeKit, SharePlay, CallKit, PermissionKit, EnergyKit).
- `ios-engineering-skills` refocused on 10 core engineering skills (networking, security, auth, accessibility, localization, debugging, diagnostics, background processing, device integrity, App Store review).

### Metadata improvements

- All 56 skill descriptions aligned with Agent Skills best practices.
- Bundle descriptions shortened and focused on user intent.
- Missing keywords added for major frameworks (Sendable, async-await, Foundation Models, BLE, Matter, etc.).
- All iOS 26 API claims verified against Apple documentation.

### Beta framework caveats

Three skills reference iOS 26 beta-only frameworks that may change before GM:
- `permissionkit` — PermissionKit (AskCenter, PermissionQuestion, CommunicationLimits)
- `energykit` — EnergyKit (ElectricityGuidance, EnergyVenue)
- `pencilkit-drawing` — references PaperKit integration

### Migration notes

- **Bundle name change**: `ios-framework-skills` no longer exists. Reinstall as `ios-app-framework-skills` and/or `ios-data-framework-skills`.
- **Skill file paths changed**: `swiftui-patterns/references/` files were reorganized during the split into three skills. Tools or scripts referencing old paths will need updating.
- **No breaking API changes**: All skills remain independently installable via `npx skills add` or `/plugin install`.

## v1.1.0

- Remove apple-docs MCP server references from skills.
- Fix npx skills CLI install commands in README.
- Bump plugin versions to 1.1.0.

## v1.0.0

- Initial release with 23 iOS and Swift development skills.
- 6 themed Claude Code plugin bundles + 1 all-skills bundle.
