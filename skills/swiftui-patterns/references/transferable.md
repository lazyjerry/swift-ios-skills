# Transferable, Drag & Drop, and ShareLink

## Intent

Adopt the `Transferable` protocol to enable sharing, drag and drop, copy/paste, and `ShareLink` with a unified API. Available iOS 16+.

> **Docs:** [Transferable](https://sosumi.ai/documentation/coretransferable/transferable) · [Choosing a transfer representation](https://sosumi.ai/documentation/coretransferable/choosing-a-transfer-representation-for-a-model-type)

## Transferable protocol overview

`Transferable` describes how a type converts to and from transfer representations (clipboard, drag, share sheet). Conform by implementing a static `transferRepresentation` property.

```swift
struct Note: Codable, Identifiable {
    let id: UUID
    var title: String
    var body: String
}

extension Note: Transferable {
    static var transferRepresentation: some TransferRepresentation {
        CodableRepresentation(contentType: .note)
        ProxyRepresentation(exporting: \.body) // fallback: plain text
    }
}

extension UTType {
    static let note = UTType(exportedAs: "com.example.note")
}
```

Representation order matters — place the most specific first, with broader fallbacks after.

## Built-in conformances

These types already conform to `Transferable` out of the box:

| Type | Content type |
|------|-------------|
| `String` | `.plainText`, `.utf8PlainText` |
| `Data` | `.data` |
| `URL` | `.url` |
| `AttributedString` | `.rtf` |
| `Image` (SwiftUI) | `.image` |
| `Color` (SwiftUI) | `.color` |

## TransferRepresentation types

### CodableRepresentation

For types conforming to `Codable`. Serializes to JSON by default:

```swift
static var transferRepresentation: some TransferRepresentation {
    CodableRepresentation(contentType: .myType)
}
```

### ProxyRepresentation

Delegate to another `Transferable` type. Ideal for quick text or URL fallbacks:

```swift
ProxyRepresentation(exporting: \.title)            // export only
ProxyRepresentation(\.url)                          // import + export via URL
```

### DataRepresentation

Full control over binary serialization:

```swift
DataRepresentation(contentType: .png) { image in
    try image.pngData()
} importing: { data in
    try MyImage(data: data)
}
```

Use `DataRepresentation(exportedContentType:)` for export-only representations.

### FileRepresentation

For large content best transferred as files:

```swift
FileRepresentation(contentType: .movie) { video in
    SentTransferredFile(video.fileURL)
} importing: { receivedFile in
    let dest = FileManager.default.temporaryDirectory.appendingPathComponent(receivedFile.file.lastPathComponent)
    try FileManager.default.copyItem(at: receivedFile.file, to: dest)
    return Video(url: dest)
}
```

## ShareLink

Present the system share sheet with a `Transferable` item:

```swift
ShareLink(item: note, preview: SharePreview(note.title)) {
    Label("Share", systemImage: "square.and.arrow.up")
}

// Multiple items
ShareLink(items: selectedNotes) { note in
    SharePreview(note.title)
}

// Simple string sharing
ShareLink(item: "Check out this app!", subject: Text("Cool App"))
```

`ShareLink` requires the item to conform to `Transferable`. The preview provides a title, optional image, and optional icon for the share sheet.

## Drag and drop

### Making views draggable

```swift
struct NoteCard: View {
    let note: Note

    var body: some View {
        Text(note.title)
            .draggable(note) // Note must be Transferable
    }
}
```

Use `.draggable(note) { DragPreview(note) }` to provide a custom drag preview.

### Drop destination

```swift
struct NoteBoard: View {
    @State private var notes: [Note] = []

    var body: some View {
        VStack {
            ForEach(notes) { NoteCard(note: $0) }
        }
        .dropDestination(for: Note.self) { droppedNotes, location in
            notes.append(contentsOf: droppedNotes)
            return true
        } isTargeted: { isOver in
            // Highlight drop zone
        }
    }
}
```

For reordering within a list, combine `.draggable` with `.dropDestination` or use `onMove` on `ForEach` inside `List`.

### Handling multiple types

Accept multiple content types with separate `.dropDestination` modifiers or use `DropDelegate` for advanced logic:

```swift
.dropDestination(for: String.self) { strings, _ in
    notes.append(contentsOf: strings.map { Note(id: UUID(), title: $0, body: "") })
    return true
}
```

## Pasteboard integration

For direct clipboard access outside SwiftUI's drag/drop system, use `UIPasteboard`:

```swift
// Copy
UIPasteboard.general.string = note.title

// Paste
if let text = UIPasteboard.general.string {
    // use text
}
```

For `Transferable` types with custom content types, export to `Data` first:

```swift
let data = try await note.exported(as: .note)
UIPasteboard.general.setData(data, forPasteboardType: UTType.note.identifier)
```

Prefer SwiftUI's `.copyable`, `.cuttable`, and `.pasteDestination` modifiers (iOS 16+) over direct `UIPasteboard` usage when possible — they integrate with the Edit menu and keyboard shortcuts automatically.

## Common patterns

### Transferable enum with multiple representations

```swift
enum SharedContent: Transferable {
    case text(String)
    case url(URL)

    static var transferRepresentation: some TransferRepresentation {
        ProxyRepresentation { content in
            switch content {
            case .text(let s): return s
            case .url(let u): return u.absoluteString
            }
        }
    }
}
```

### Export-only conformance

When your type should be sharable but not importable:

```swift
extension Report: Transferable {
    static var transferRepresentation: some TransferRepresentation {
        DataRepresentation(exportedContentType: .pdf) { report in
            try report.renderPDF()
        }
    }
}
```

## Pitfalls

- Always declare custom `UTType` identifiers in Info.plist under Exported/Imported Type Identifiers.
- Representation order matters — the first matching representation wins. Put the richest format first.
- `FileRepresentation` files are temporary; copy them if you need to persist.
- `Transferable` conformance must be on the main type, not an extension in a different module, to avoid linker issues.
- Test drag and drop on device — Simulator haptics and drop targeting differ from hardware.
