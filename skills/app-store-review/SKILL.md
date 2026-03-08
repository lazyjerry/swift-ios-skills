---
name: app-store-review
description: "App Store review preparation and rejection prevention. Covers App Store review guidelines, app rejection reasons, PrivacyInfo.xcprivacy privacy manifest requirements, required API reason codes, in-app purchase IAP and StoreKit rules, App Store Guidelines compliance, ATT App Tracking Transparency, EU DMA Digital Markets Act, HIG compliance checklist, app submission preparation, review preparation, metadata requirements, entitlements, widgets, and Live Activities review rules. Use when preparing for App Store submission, fixing rejection reasons, auditing privacy manifests, implementing ATT consent flow, configuring StoreKit IAP, or checking HIG compliance."
---

# App Store Review Preparation

Guidance for catching App Store rejection risks before submission. Apple reviewed 7.7 million submissions in 2024 and rejected 1.9 million. Most rejections are preventable with proper preparation.

## Contents

- [Overview](#overview)
- [Top Rejection Reasons and How to Avoid Them](#top-rejection-reasons-and-how-to-avoid-them)
- [PrivacyInfo.xcprivacy -- Privacy Manifest Requirements](#privacyinfoxcprivacy-privacy-manifest-requirements)
- [Data Use, Sharing, and Privacy Policy (Guideline 5.1.2)](#data-use-sharing-and-privacy-policy-guideline-512)
- [In-App Purchase and StoreKit Rules (Guideline 3.1.1)](#in-app-purchase-and-storekit-rules-guideline-311)
- [HIG Compliance Checklist](#hig-compliance-checklist)
- [App Tracking Transparency (ATT)](#app-tracking-transparency-att)
- [EU Digital Markets Act (DMA) Considerations](#eu-digital-markets-act-dma-considerations)
- [Entitlements and Capabilities](#entitlements-and-capabilities)
- [Common Mistakes](#common-mistakes)
- [Pre-Submission Checklist](#pre-submission-checklist)
- [References](#references)

## Overview

Use this SKILL.md for quick guidance on common rejection reasons and key policies. Use the references for detailed checklists and privacy manifest specifics.

## Top Rejection Reasons and How to Avoid Them

### Guideline 2.1 -- App Completeness

The app must be fully functional when reviewed. Apple rejects for:

- Placeholder content, lorem ipsum, or test data visible anywhere
- Broken links or empty screens
- Features behind logins without demo credentials provided in App Review notes
- Features that require hardware Apple does not have access to

**Prevention:**
- Provide demo account credentials in the App Review Information notes field in App Store Connect
- Walk through every screen and verify real content is present
- Test all flows end-to-end, including edge cases like empty states and error conditions

### Guideline 2.3 -- Accurate Metadata

- App name must match what the app actually does
- Screenshots must show the actual app UI, not marketing renders or mockups
- Description must not contain prices (they vary by region)
- No references to other platforms ("Also available on Android")
- Keywords must be relevant -- no competitor names or unrelated terms
- Category must match the app's primary function

### Guideline 4.2 -- Minimum Functionality

Apple rejects apps that are too simple or are just websites in a wrapper:

- WKWebView-only apps are rejected unless they add meaningful native functionality
- Single-feature apps may be rejected if the feature is better suited as part of another app
- Apps that duplicate built-in iOS functionality without significant improvement are rejected

### Guideline 2.5.1 -- Software Requirements

- Must use public APIs only -- private API usage is an instant rejection
- Must be built with the current Xcode GM release or later
- Must support the latest two major iOS versions (guideline, not strict rule)
- Must not download or execute code dynamically (except JavaScript in WKWebView)

## PrivacyInfo.xcprivacy -- Privacy Manifest Requirements

This is the fastest-growing rejection category (Guideline 5.1.1). A privacy manifest is **required** if your app or any of its dependencies uses certain categories of APIs.

**See:** `references/privacy-manifest.md` for the full structure, reason codes, and checklists.

### Summary

- File timestamp, system boot time, disk space, user defaults, and active keyboard APIs all require reason codes.
- Every third-party SDK must ship its own privacy manifest.
- Manifest declarations must match App Store privacy nutrition labels and actual network behavior.

## Data Use, Sharing, and Privacy Policy (Guideline 5.1.2)

- A privacy policy URL must be set in App Store Connect AND accessible within the app
- The privacy policy must accurately describe what data you collect, how you use it, and who you share it with
- App Store privacy nutrition labels must match your actual data collection practices
- Apple cross-references your privacy manifest, nutrition labels, and observed network traffic

## In-App Purchase and StoreKit Rules (Guideline 3.1.1)

IAP rules are strict and heavily enforced.

### What Requires Apple IAP

All digital content and services must use Apple's In-App Purchase system:

- Premium features or content unlocks
- Subscriptions to app functionality
- Virtual currency, coins, gems
- Ad removal
- Digital tips or donations

### What Does NOT Require IAP

- Physical products (e-commerce)
- Ride-sharing, food delivery, real-world services
- One-to-one services (tutoring, consulting booked through the app)
- Enterprise/B2B apps distributed through Apple Business Manager

### Subscription Display Requirements

- Price, duration, and auto-renewal terms must be clearly displayed before purchase
- Free trials must state what happens when they end (price, billing frequency)
- No links, buttons, or language directing users to purchase outside the app
- "Reader" apps (Netflix, Spotify) may link to external sign-up but cannot offer IAP bypass

### StoreKit Implementation Checklist

- Consumables, non-consumables, and subscriptions must be correctly categorized in App Store Connect
- Restore purchases functionality must be present and working
- Transaction verification should use StoreKit 2 `Transaction.currentEntitlements` or server-side validation
- Handle interrupted purchases, deferred transactions, and ask-to-buy gracefully

## HIG Compliance Checklist

See `references/review-checklists.md` for the full HIG checklist (navigation, modals, widgets, system feature support, launch screen, empty states). This section stays intentionally brief to keep SKILL.md concise.

## App Tracking Transparency (ATT)

### When ATT Is Required

If your app tracks users across other companies' apps or websites, you must:

1. Request permission via `ATTrackingManager.requestTrackingAuthorization` before any tracking occurs
2. Respect the user's choice -- do not track if the user denies permission
3. Not gate app functionality behind tracking consent ("Accept tracking or you cannot use this app" is rejected)
4. Provide a clear purpose string in `NSUserTrackingUsageDescription` explaining what tracking is used for

### When ATT Is NOT Required

If you do not track users across apps or websites, do not show the ATT prompt. Apple rejects unnecessary ATT prompts.

### ATT Implementation

```swift
import AppTrackingTransparency

func requestTrackingPermission() async {
    let status = await ATTrackingManager.requestTrackingAuthorization()
    switch status {
    case .authorized:
        // Enable tracking, initialize ad SDKs with tracking
        break
    case .denied, .restricted:
        // Use non-personalized ads, disable cross-app tracking
        break
    case .notDetermined:
        // Should not happen after request, handle gracefully
        break
    @unknown default:
        break
    }
}
```

**Timing:** Request ATT permission after the app has launched and the user has context for why tracking is being requested. Do not show the prompt immediately on first launch.

## EU Digital Markets Act (DMA) Considerations

For apps distributed in the EU:

- Alternative browser engines are permitted on iOS in the EU
- Alternative app marketplaces exist -- apps may be distributed outside the App Store
- External payment links may be allowed under specific conditions, with Apple's commission structure adjusted
- Notarization is required even for sideloaded apps distributed outside the App Store
- Apps using alternative distribution must still meet Apple's notarization requirements for security

## Entitlements and Capabilities

Every entitlement must be justified. Apple reviews these closely:

| Entitlement | Apple Scrutiny |
|---|---|
| Camera | Must explain purpose in `NSCameraUsageDescription` |
| Location (Always) | Must have clear, user-visible reason for background location |
| Push Notifications | Must not be used for marketing without user opt-in |
| HealthKit | Must actually use health data in a meaningful way |
| Background Modes | Each mode (audio, location, VoIP, fetch) must be justified and actively used |
| App Groups | Must explain what shared data is needed |
| Associated Domains | Universal links must actually resolve and function |

### Usage Description Strings

Usage descriptions in Info.plist must be specific about what data is accessed and why:

```
// REJECTED -- too vague
"This app needs your location."

// APPROVED -- specific purpose
"Your location is used to show nearby restaurants on the map."

// REJECTED -- too vague
"This app needs access to your camera."

// APPROVED -- specific purpose
"The camera is used to scan barcodes for price comparison."
```

Apple rejects vague usage descriptions. Always state what the data is used for in user-facing terms.

## Common Mistakes

1. **Missing demo credentials.** Provide App Review login credentials in App Store Connect notes. Most Guideline 2.1 rejections are from reviewers unable to test behind a login.
2. **Privacy manifest mismatch.** Declared data collection in PrivacyInfo.xcprivacy must match App Store privacy nutrition labels and actual network traffic.
3. **Unnecessary ATT prompt.** Do not show the App Tracking Transparency prompt unless you actually track users across apps or websites. Apple rejects unnecessary prompts.
4. **Vague usage descriptions.** "This app needs your location" is rejected. State the specific feature that uses the data.
5. **External payment links for digital content.** Any language or button directing users to purchase digital content outside the app is rejected.
6. **Missing concurrency annotations.** Ensure ATT request and StoreKit calls run on `@MainActor` or appropriate actor context. Mark shared state types as `Sendable` for Swift 6 concurrency safety.

## Pre-Submission Checklist

Quick-check before every submission (full version in `references/review-checklists.md`):

- [ ] No placeholder/test content; all features functional; demo credentials provided
- [ ] App name matches functionality; screenshots are real; no prices in description
- [ ] PrivacyInfo.xcprivacy present with reason codes; nutrition labels match reality
- [ ] Privacy policy URL set and accessible in-app
- [ ] Digital content uses IAP; subscription terms visible; restore purchases works
- [ ] Dark Mode and Dynamic Type supported; standard navigation patterns
- [ ] Built with current Xcode GM; no private APIs; entitlements justified
- [ ] ATT prompt only if cross-app tracking occurs

## References

- Review checklists: `references/review-checklists.md`
- Privacy manifest guide: `references/privacy-manifest.md`
