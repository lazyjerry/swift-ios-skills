# Changelog

## v2.0.1

- Update `swiftui-animation` to cover `.animation(_:body:)` alongside `.animation(_:value:)`, and tighten wording around bare `.animation(_:)` and scoped transactions to match Apple docs.
- Update `swiftui-performance` to reference both value-bound and modifier-scoped implicit animation patterns.
- Bump Claude marketplace bundle versions to 2.0.1.

## v2.0.0

### New skills (33 added)

**SwiftUI (3)**
- `swiftui-gestures` — Tap, drag, magnify, rotate, long press, simultaneous and sequential gestures
- `swiftui-layout-components` — Grid, LazyVGrid, Layout protocol, ViewThatFits, custom layouts
- `swiftui-navigation` — NavigationStack, NavigationSplitView, programmatic navigation, deep linking

**Core Swift (1)**
- `swift-language` — Swift 6.2 features, macros, result builders, property wrappers

**App Experience Frameworks (3)**
- `alarmkit` — AlarmKit alarms and countdown timers, Live Activity integration, AlarmAttributes, AlarmButton
- `app-clips` — App Clip experiences, invocation, size limits, shared data
- `photos-camera-media` — PhotosPicker, AVCaptureSession, photo library, video recording, media permissions

**Data & Service Frameworks (7)**
- `cloudkit-sync` — CKContainer, CKRecord, subscriptions, sharing, NSPersistentCloudKitContainer
- `contacts-framework` — CNContactStore, fetch requests, key descriptors, CNContactPickerViewController, save requests
- `eventkit-calendar` — EKEventStore, EKEvent, EKReminder, recurrence rules, EventKitUI editors and choosers
- `healthkit` — HKHealthStore, queries, statistics, workout sessions, background delivery
- `musickit-audio` — MusicKit authorization, catalog search, ApplicationMusicPlayer, MPRemoteCommandCenter
- `passkit-wallet` — Apple Pay, PKPaymentRequest, PKPaymentAuthorizationController, Wallet passes
- `weatherkit` — WeatherService, current/hourly/daily forecasts, alerts, attribution requirements

**AI & Machine Learning (3)**
- `coreml` — Core ML model loading, prediction, MLTensor, compute unit configuration, VNCoreMLRequest, MLComputePlan
- `natural-language` — NLTokenizer, NLTagger, sentiment analysis, language identification, embeddings, Translation
- `speech-recognition` — SFSpeechRecognizer, on-device recognition, audio buffer processing

**iOS Engineering (5)**
- `authentication` — Sign in with Apple, ASAuthorizationController, passkeys, biometric auth (LAContext), credential management
- `background-processing` — BGTaskScheduler, background refresh, URLSession background transfers
- `device-integrity` — DeviceCheck (DCDevice per-device bits), App Attest (DCAppAttestService attestation and assertion flows)
- `metrickit-diagnostics` — MXMetricManager, hang diagnostics, crash reports, power metrics
- `ios-localization` — String Catalogs, pluralization, FormatStyle, right-to-left layout

**Hardware & Device Integration (4)**
- `core-motion` — CMMotionManager, CMPedometer, accelerometer, gyroscope, activity recognition, altitude
- `core-nfc` — NFCNDEFReaderSession, NFCTagReaderSession, NDEF reading/writing, background tag reading
- `pencilkit-drawing` — PKCanvasView, PKDrawing, PKToolPicker, Apple Pencil, PaperKit integration
- `realitykit-ar` — RealityView, entities, anchors, ARKit world tracking, raycasting, scene understanding

**Platform Integration (7)**
- `callkit-voip` — CXProvider, CXCallController, PushKit VoIP registration, call directory extensions
- `energykit` — ElectricityGuidance, EnergyVenue, grid forecasts, load event submission, electricity insights
- `homekit-matter` — HMHomeManager, accessories, rooms, actions, triggers, MatterSupport commissioning
- `mapkit-location` — MapKit, CoreLocation, annotations, geocoding, directions, geofencing
- `permissionkit` — AskCenter, PermissionQuestion, child communication safety, CommunicationLimits
- `shareplay-activities` — GroupActivity, GroupSession, GroupSessionMessenger, coordinated media playback
- `apple-on-device-ai` — Foundation Models framework, Core ML, MLX Swift, on-device LLM inference

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
