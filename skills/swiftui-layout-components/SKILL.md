---
name: swiftui-layout-components
description: "Build SwiftUI layouts using stacks, grids, lists, scroll views, forms, and controls. Covers VStack/HStack/ZStack, LazyVGrid/LazyHGrid, List with sections and swipe actions, ScrollView with ScrollViewReader, Form with validation, Toggle/Picker/Slider, .searchable, and overlay patterns. Use when building data-driven layouts, collection views, settings screens, search interfaces, or transient overlay UI."
---

# SwiftUI Layout & Components

Layout and component patterns for SwiftUI apps targeting iOS 26+ with Swift 6.2. Covers stack and grid layouts, list patterns, scroll views, forms, controls, search, and overlays. Patterns are backward-compatible to iOS 17 unless noted.

## Contents

- [Layout Fundamentals](#layout-fundamentals)
- [Grid Layouts](#grid-layouts)
- [List Patterns](#list-patterns)
- [ScrollView](#scrollview)
- [Form and Controls](#form-and-controls)
- [Searchable](#searchable)
- [Overlay and Presentation](#overlay-and-presentation)
- [Common Mistakes](#common-mistakes)
- [Review Checklist](#review-checklist)
- [References](#references)

## Layout Fundamentals

### Standard Stacks

Use `VStack`, `HStack`, and `ZStack` for small, fixed-size content. They render all children immediately.

```swift
VStack(alignment: .leading, spacing: 8) {
    Text(title).font(.headline)
    Text(subtitle).font(.subheadline).foregroundStyle(.secondary)
}
```

### Lazy Stacks

Use `LazyVStack` and `LazyHStack` inside `ScrollView` for large or dynamic collections. They create child views on demand as they scroll into view.

```swift
ScrollView {
    LazyVStack(spacing: 12) {
        ForEach(items) { item in
            ItemRow(item: item)
        }
    }
    .padding(.horizontal)
}
```

**When to use which:**
- **Non-lazy stacks:** Small, fixed content (headers, toolbars, forms with few fields)
- **Lazy stacks:** Large or unknown-size collections, feeds, chat messages

## Grid Layouts

Use `LazyVGrid` for icon pickers, media galleries, and dense visual selections. Use `.adaptive` columns for layouts that scale across device sizes, or `.flexible` columns for a fixed column count.

```swift
// Adaptive grid -- columns adjust to fit
let columns = [GridItem(.adaptive(minimum: 120, maximum: 1024))]

LazyVGrid(columns: columns, spacing: 6) {
    ForEach(items) { item in
        ThumbnailView(item: item)
            .aspectRatio(1, contentMode: .fit)
    }
}
```

```swift
// Fixed 3-column grid
let columns = Array(repeating: GridItem(.flexible(minimum: 100), spacing: 4), count: 3)

LazyVGrid(columns: columns, spacing: 4) {
    ForEach(items) { item in
        ThumbnailView(item: item)
    }
}
```

Use `.aspectRatio` for cell sizing. Never place `GeometryReader` inside lazy containers -- it forces eager measurement and defeats lazy loading. Use `.onGeometryChange` (iOS 18+) if you need to read dimensions.

See `../swiftui-patterns/references/grids.md` for full grid patterns and design choices.

## List Patterns

Use `List` for feed-style content and settings rows where built-in row reuse, selection, and accessibility matter.

```swift
List {
    Section("General") {
        NavigationLink("Display") { DisplaySettingsView() }
        NavigationLink("Haptics") { HapticsSettingsView() }
    }
    Section("Account") {
        Button("Sign Out", role: .destructive) { }
    }
}
.listStyle(.insetGrouped)
```

**Key patterns:**
- `.listStyle(.plain)` for feed layouts, `.insetGrouped` for settings
- `.scrollContentBackground(.hidden)` + custom background for themed surfaces
- `.listRowInsets(...)` and `.listRowSeparator(.hidden)` for spacing and separator control
- Pair with `ScrollViewReader` for scroll-to-top or jump-to-id
- Use `.refreshable { }` for pull-to-refresh feeds
- Use `.contentShape(Rectangle())` on rows that should be tappable end-to-end

**iOS 26:** Apply `.scrollEdgeEffectStyle(.soft, for: .top)` for modern scroll edge effects.

See `../swiftui-patterns/references/list.md` for full list patterns including feed lists with scroll-to-top.

## ScrollView

Use `ScrollView` with lazy stacks when you need custom layout, mixed content, or horizontal scrolling.

```swift
ScrollView(.horizontal, showsIndicators: false) {
    LazyHStack(spacing: 8) {
        ForEach(chips) { chip in
            ChipView(chip: chip)
        }
    }
}
```

**ScrollViewReader:** Enables programmatic scrolling to specific items.

```swift
ScrollViewReader { proxy in
    ScrollView {
        LazyVStack {
            ForEach(messages) { message in
                MessageRow(message: message).id(message.id)
            }
        }
    }
    .onChange(of: messages.last?.id) { _, newValue in
        if let id = newValue {
            withAnimation { proxy.scrollTo(id, anchor: .bottom) }
        }
    }
}
```

**`safeAreaInset(edge:)`** pins content (input bars, toolbars) above the keyboard without affecting scroll layout.

**iOS 26 additions:**
- `.scrollEdgeEffectStyle(.soft, for: .top)` -- fading edge effect
- `.backgroundExtensionEffect()` -- mirror/blur at safe area edges (use sparingly, one per screen)
- `.safeAreaBar(edge:)` -- attach bar views that integrate with scroll effects

See `../swiftui-patterns/references/scrollview.md` for full scroll patterns and iOS 26 edge effects.

## Form and Controls

### Form

Use `Form` for structured settings and input screens. Group related controls into `Section` blocks.

```swift
Form {
    Section("Notifications") {
        Toggle("Mentions", isOn: $prefs.mentions)
        Toggle("Follows", isOn: $prefs.follows)
    }
    Section("Appearance") {
        Picker("Theme", selection: $theme) {
            ForEach(Theme.allCases, id: \.self) { Text($0.title).tag($0) }
        }
        Slider(value: $fontScale, in: 0.5...1.5, step: 0.1)
    }
}
.formStyle(.grouped)
.scrollContentBackground(.hidden)
```

Use `@FocusState` to manage keyboard focus in input-heavy forms. Wrap in `NavigationStack` only when presented standalone or in a sheet.

### Controls

| Control | Usage |
|---------|-------|
| `Toggle` | Boolean preferences |
| `Picker` | Discrete choices; `.segmented` for 2-4 options |
| `Slider` | Numeric ranges with visible value label |
| `DatePicker` | Date/time selection |
| `TextField` | Text input with `.keyboardType`, `.textInputAutocapitalization` |

See `../swiftui-patterns/references/form.md` and `../swiftui-patterns/references/controls.md` for full examples.

## Searchable

Add native search UI with `.searchable`. Use `.searchScopes` for multiple modes and `.task(id:)` for debounced async results.

```swift
List {
    ForEach(results) { result in
        SearchRow(result: result)
    }
}
.searchable(
    text: $searchQuery,
    placement: .navigationBarDrawer(displayMode: .always),
    prompt: "Search"
)
.searchScopes($searchScope) {
    ForEach(SearchScope.allCases, id: \.self) { scope in
        Text(scope.title)
    }
}
.task(id: searchQuery) {
    guard !searchQuery.isEmpty else { results = []; return }
    try? await Task.sleep(for: .milliseconds(250))  // Debounce
    results = await fetchResults(query: searchQuery, scope: searchScope)
}
```

See `../swiftui-patterns/references/searchable.md` for full searchable patterns.

## Overlay and Presentation

Use `.overlay(alignment:)` for transient UI (toasts, banners) without affecting layout.

```swift
content
    .overlay(alignment: .top) {
        if let toast {
            ToastView(toast: toast)
                .transition(.move(edge: .top).combined(with: .opacity))
        }
    }
```

For global toast state, use a dedicated observable (e.g., `ToastCenter`). Keep overlays lightweight and auto-dismissible.

**fullScreenCover:** Use `.fullScreenCover(item:)` for immersive presentations that cover the entire screen (media viewers, onboarding flows).

See `../swiftui-patterns/references/overlay.md` for full overlay and toast patterns.

## Common Mistakes

1. Using non-lazy stacks for large collections -- causes all children to render immediately
2. Placing `GeometryReader` inside lazy containers -- defeats lazy loading
3. Using array indices as `ForEach` IDs -- causes incorrect diffing and UI bugs
4. Nesting scroll views of the same axis -- causes gesture conflicts
5. Heavy custom layouts inside `List` rows -- use `ScrollView` + `LazyVStack` instead
6. Missing `.contentShape(Rectangle())` on tappable rows -- tap area is text-only
7. Hard-coding frame dimensions for sheets -- use `.presentationSizing` instead
8. Running searches on empty strings -- always guard against empty queries
9. Mixing `List` and `ScrollView` in the same hierarchy -- gesture conflicts
10. Using `.pickerStyle(.segmented)` for large option sets -- use menu or inline styles

## Review Checklist

- [ ] `LazyVStack`/`LazyHStack` used for large or dynamic collections
- [ ] Stable `Identifiable` IDs on all `ForEach` items (not array indices)
- [ ] No `GeometryReader` inside lazy containers
- [ ] `List` style matches context (`.plain` for feeds, `.insetGrouped` for settings)
- [ ] `Form` used for structured input screens (not custom stacks)
- [ ] `.searchable` debounces input with `.task(id:)`
- [ ] `.refreshable` added where data source supports pull-to-refresh
- [ ] Overlays use transitions and auto-dismiss timers
- [ ] `.contentShape(Rectangle())` on tappable rows
- [ ] `@FocusState` manages keyboard focus in forms

## References

- Grid patterns: `../swiftui-patterns/references/grids.md`
- List and section patterns: `../swiftui-patterns/references/list.md`
- ScrollView and lazy stacks: `../swiftui-patterns/references/scrollview.md`
- Form patterns: `../swiftui-patterns/references/form.md`
- Controls (Toggle, Picker, Slider): `../swiftui-patterns/references/controls.md`
- Searchable patterns: `../swiftui-patterns/references/searchable.md`
- Overlay and toasts: `../swiftui-patterns/references/overlay.md`
- Architecture and state management: see `swiftui-patterns` skill
- Navigation patterns: see `swiftui-navigation` skill
