# Haptics

## Intent

Use haptics sparingly to reinforce user actions (tab selection, refresh, success/error) and respect user preferences.

## Core patterns

- Centralize haptic triggers in a `HapticManager` or similar utility.
- Gate haptics behind user preferences and hardware support.
- Use distinct types for different UX moments (selection vs. notification vs. refresh).

## Example: simple haptic manager

```swift
@MainActor
final class HapticManager {
  static let shared = HapticManager()

  enum HapticType {
    case buttonPress
    case tabSelection
    case dataRefresh(intensity: CGFloat)
    case notification(UINotificationFeedbackGenerator.FeedbackType)
  }

  private let selectionGenerator = UISelectionFeedbackGenerator()
  private let impactGenerator = UIImpactFeedbackGenerator(style: .heavy)
  private let notificationGenerator = UINotificationFeedbackGenerator()

  private init() { selectionGenerator.prepare() }

  func fire(_ type: HapticType, isEnabled: Bool) {
    guard isEnabled else { return }
    switch type {
    case .buttonPress:
      impactGenerator.impactOccurred()
    case .tabSelection:
      selectionGenerator.selectionChanged()
    case let .dataRefresh(intensity):
      impactGenerator.impactOccurred(intensity: intensity)
    case let .notification(style):
      notificationGenerator.notificationOccurred(style)
    }
  }
}
```

## Example: usage

```swift
Button("Save") {
  HapticManager.shared.fire(.notification(.success), isEnabled: preferences.hapticsEnabled)
}

TabView(selection: $selectedTab) { /* tabs */ }
  .onChange(of: selectedTab) { _, _ in
    HapticManager.shared.fire(.tabSelection, isEnabled: preferences.hapticTabSelectionEnabled)
  }
```

## Design choices to keep

- Haptics should be subtle and not fire on every tiny interaction.
- Respect user preferences (toggle to disable).
- Keep haptic triggers close to the user action, not deep in data layers.

## Pitfalls

- Avoid firing multiple haptics in quick succession.
- Do not assume haptics are available; check support.

## Core Haptics (CHHapticEngine)

For advanced haptic patterns beyond the simple feedback generators, use Core Haptics. It provides precise control over haptic intensity, sharpness, and timing with support for audio-haptic synchronization.

> **Docs:** [CHHapticEngine](https://sosumi.ai/documentation/corehaptics/chhapticengine) · [Preparing your app to play haptics](https://sosumi.ai/documentation/corehaptics/preparing-your-app-to-play-haptics)

### Capabilities check

Always verify hardware support before creating an engine:

```swift
import CoreHaptics

let supportsHaptics = CHHapticEngine.capabilitiesForHardware().supportsHaptics
let supportsAudio = CHHapticEngine.capabilitiesForHardware().supportsAudio
```

### Engine setup and lifecycle

```swift
@MainActor
final class CoreHapticManager {
    private var engine: CHHapticEngine?

    func prepareEngine() throws {
        guard CHHapticEngine.capabilitiesForHardware().supportsHaptics else { return }

        engine = try CHHapticEngine()

        // Called when the engine stops due to external cause (audio session interruption, app backgrounding)
        engine?.stoppedHandler = { reason in
            print("Haptic engine stopped: \(reason)")
        }

        // Called after the engine is reset (e.g., after audio session interruption ends)
        engine?.resetHandler = { [weak self] in
            do {
                try self?.engine?.start()
            } catch {
                print("Failed to restart engine: \(error)")
            }
        }

        try engine?.start()
    }

    func stopEngine() {
        engine?.stop()
    }
}
```

Key lifecycle rules:
- Call `engine.start()` before playing any patterns.
- Handle `stoppedHandler` — the system can stop the engine when your app moves to the background or during audio interruptions.
- Handle `resetHandler` — restart the engine when the system resets it.
- Call `engine.stop()` when haptics are no longer needed to save battery.

### CHHapticPattern and CHHapticEvent

Build patterns from individual haptic and audio events:

```swift
func playTransientTap() throws {
    let sharpness = CHHapticEventParameter(parameterID: .hapticSharpness, value: 0.8)
    let intensity = CHHapticEventParameter(parameterID: .hapticIntensity, value: 1.0)

    // Transient: short, single-tap feel
    let event = CHHapticEvent(
        eventType: .hapticTransient,
        parameters: [intensity, sharpness],
        relativeTime: 0
    )

    let pattern = try CHHapticPattern(events: [event], parameters: [])
    let player = try engine?.makePlayer(with: pattern)
    try player?.start(atTime: CHHapticTimeImmediate)
}
```

**Event types:**

| Type | Description |
|------|-------------|
| `.hapticTransient` | Brief, tap-like impulse |
| `.hapticContinuous` | Sustained vibration over a `duration` |
| `.audioContinuous` | Sustained audio tone |
| `.audioCustom` | Play a custom audio resource |

**Common parameters:** `.hapticIntensity` (0–1), `.hapticSharpness` (0–1), `.attackTime`, `.decayTime`, `.releaseTime`.

### Playing patterns with CHHapticPatternPlayer

```swift
func playContinuousBuzz() throws {
    let intensity = CHHapticEventParameter(parameterID: .hapticIntensity, value: 0.6)
    let sharpness = CHHapticEventParameter(parameterID: .hapticSharpness, value: 0.3)

    let event = CHHapticEvent(
        eventType: .hapticContinuous,
        parameters: [intensity, sharpness],
        relativeTime: 0,
        duration: 0.5
    )

    let pattern = try CHHapticPattern(events: [event], parameters: [])
    let player = try engine?.makePlayer(with: pattern)
    try player?.start(atTime: CHHapticTimeImmediate)
}
```

For looping, seeking, and pausing, use `CHHapticAdvancedPatternPlayer` via `engine.makeAdvancedPlayer(with:)`.

### Haptic parameter curves (CHHapticParameterCurve)

Smoothly vary parameters over time within a pattern:

```swift
func playRampingPattern() throws {
    let event = CHHapticEvent(
        eventType: .hapticContinuous,
        parameters: [
            CHHapticEventParameter(parameterID: .hapticIntensity, value: 0.2),
            CHHapticEventParameter(parameterID: .hapticSharpness, value: 0.1)
        ],
        relativeTime: 0,
        duration: 1.0
    )

    // Ramp intensity from 0.2 → 1.0 over 1 second
    let curve = CHHapticParameterCurve(
        parameterID: .hapticIntensityControl,
        controlPoints: [
            .init(relativeTime: 0, value: 0.2),
            .init(relativeTime: 0.5, value: 0.7),
            .init(relativeTime: 1.0, value: 1.0)
        ],
        relativeTime: 0
    )

    let pattern = try CHHapticPattern(events: [event], parameterCurves: [curve])
    let player = try engine?.makePlayer(with: pattern)
    try player?.start(atTime: CHHapticTimeImmediate)
}
```

### Audio-haptic synchronization (AHAP files)

AHAP (Apple Haptic and Audio Pattern) files define haptic patterns in JSON for easy authoring and design iteration. Load them directly:

```swift
func playAHAPFile() throws {
    guard let url = Bundle.main.url(forResource: "success", withExtension: "ahap") else { return }
    try engine?.playPattern(from: url)
}
```

AHAP files support the same events, parameters, and parameter curves as the programmatic API. Use the **Core Haptics** design tools in Xcode to preview patterns.

> **Docs:** [Representing haptic patterns in AHAP files](https://sosumi.ai/documentation/corehaptics/representing-haptic-patterns-in-ahap-files)
