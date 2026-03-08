---
name: swift-testing
description: "Swift Testing framework guide for writing tests with @Test, @Suite, #expect, #require, confirmation, parameterized tests, test tags, traits, withKnownIssue, XCTest UI testing, XCUITest, test plan, mocking, test doubles, testable architecture, snapshot testing, async test patterns, test organization, and test-driven development in Swift. Use when writing or migrating tests with Swift Testing framework, implementing parameterized tests, working with test traits, converting XCTest to Swift Testing, or setting up test organization and mocking patterns."
---

# Swift Testing

Swift Testing is the modern testing framework for Swift (Xcode 16+, Swift 6+). Prefer it over XCTest for all new unit tests. Use XCTest only for UI tests, performance benchmarks, and snapshot tests.

## Contents

- [References](#references)
- [Basic Tests](#basic-tests)
- [@Test Traits](#test-traits)
- [#expect and #require](#expect-and-require)
- [@Suite and Test Organization](#suite-and-test-organization)
- [Parameterized Tests](#parameterized-tests)
- [Confirmation (Async Event Testing)](#confirmation-async-event-testing)
- [Tags](#tags)
- [Known Issues](#known-issues)
- [TestScoping (Custom Test Lifecycle)](#testscoping-custom-test-lifecycle)
- [Mocking and Test Doubles](#mocking-and-test-doubles)
- [Testable Architecture](#testable-architecture)
- [Async and Concurrent Test Patterns](#async-and-concurrent-test-patterns)
- [XCTest: UI Testing](#xctest-ui-testing)
- [Common Mistakes](#common-mistakes)
- [Test Attachments](#test-attachments)
- [Exit Testing](#exit-testing)
- [Review Checklist](#review-checklist)
- [MCP Integration](#mcp-integration)

## References

- Testing patterns: `references/testing-patterns.md`

---

## Basic Tests

```swift
import Testing

@Test("User can update their display name")
func updateDisplayName() {
    var user = User(name: "Alice")
    user.name = "Bob"
    #expect(user.name == "Bob")
}
```

## @Test Traits

```swift
@Test("Validates email format")                                    // display name
@Test(.tags(.validation, .email))                                  // tags
@Test(.disabled("Server migration in progress"))                   // disabled
@Test(.enabled(if: ProcessInfo.processInfo.environment["CI"] != nil)) // conditional
@Test(.bug("https://github.com/org/repo/issues/42"))               // bug reference
@Test(.timeLimit(.minutes(1)))                                     // time limit
@Test("Timeout handling", .tags(.networking), .timeLimit(.seconds(30))) // combined
```

## #expect and #require

```swift
// #expect records failure but continues execution
#expect(result == 42)
#expect(name.isEmpty == false)
#expect(items.count > 0, "Items should not be empty")

// #expect with error type checking
#expect(throws: ValidationError.self) {
    try validate(email: "not-an-email")
}

// #expect with specific error value
#expect {
    try validate(email: "")
} throws: { error in
    guard let err = error as? ValidationError else { return false }
    return err == .empty
}

// #require records failure AND stops test (like XCTUnwrap)
let user = try #require(await fetchUser(id: 1))
#expect(user.name == "Alice")

// #require for optionals -- unwraps or fails
let first = try #require(items.first)
#expect(first.isValid)
```

**Rule: Use `#require` when subsequent assertions depend on the value. Use `#expect` for independent checks.**

## @Suite and Test Organization

See `references/testing-patterns.md` for suite organization, parameterized tests, confirmation patterns, and known-issue handling.

## Parameterized Tests

Use `@Test(arguments:)` for parameterized cases. See `references/testing-patterns.md` for full examples.

```swift
@Test("Email validation", arguments: [
    ("user@example.com", true),
    ("user@", false),
    ("@example.com", false),
    ("", false),
])
func validateEmail(email: String, isValid: Bool) {
    #expect(EmailValidator.isValid(email) == isValid)
}

// From CaseIterable
@Test(arguments: Currency.allCases)
func currencyHasSymbol(currency: Currency) {
    #expect(currency.symbol.isEmpty == false)
}

// Two collections: cartesian product of all combinations
@Test(arguments: [1, 2, 3], ["a", "b"])
func combinations(number: Int, letter: String) {
    #expect(number > 0)
}

// Use zip for 1:1 pairing instead of cartesian product
@Test(arguments: zip(["USD", "EUR"], ["$", "€"]))
func currencySymbols(code: String, symbol: String) {
    #expect(Currency(code: code).symbol == symbol)
}
```

Each argument combination runs as an independent test case reported separately.

## Confirmation (Async Event Testing)

Use `confirmation` for async event testing. See `references/testing-patterns.md` for full examples.

## Tags

Tags must be declared as static members in an extension on `Tag`. See `references/testing-patterns.md` for tag patterns.

## Known Issues

Use `withKnownIssue` to mark expected failures. See `references/testing-patterns.md` for full patterns.

Mark expected failures so they do not cause test failure:

```swift
withKnownIssue("Propane tank is empty") {
    #expect(truck.grill.isHeating)
}

// Intermittent / flaky failures
withKnownIssue(isIntermittent: true) {
    #expect(service.isReachable)
}

// Conditional known issue
withKnownIssue {
    #expect(foodTruck.grill.isHeating)
} when: {
    !hasPropane
}

// Match specific issues only
try withKnownIssue {
    let level = try #require(foodTruck.batteryLevel)
    #expect(level >= 0.8)
} matching: { issue in
    guard case .expectationFailed(let expectation) = issue.kind else { return false }
    return expectation.isRequired
}
```

If no known issues are recorded, Swift Testing records a distinct issue notifying you the problem may be resolved.

## TestScoping (Custom Test Lifecycle)

Use `TestScoping` to consolidate setup/teardown logic. See `references/testing-patterns.md` for full examples.

## Mocking and Test Doubles

See `references/testing-patterns.md` for mocking patterns, protocol-based doubles, and testable architecture examples.

## Testable Architecture

See `references/testing-patterns.md` for view model testing and environment injection examples.

## Async and Concurrent Test Patterns

See `references/testing-patterns.md` for async test patterns, clock injection, and error path testing.

## XCTest: UI Testing

Swift Testing does not support UI testing. Use XCTest with XCUITest for all UI tests. See `references/testing-patterns.md` for UI test and snapshot examples.

See `references/testing-patterns.md` for page object, performance testing, snapshot testing, and test file organization patterns.

## Common Mistakes

1. **Testing implementation, not behavior.** Test what the code does, not how.
2. **No error path tests.** If a function can throw, test the throw path.
3. **Flaky async tests.** Use `confirmation` with expected counts, not `sleep` calls.
4. **Shared mutable state between tests.** Each test sets up its own state via `init()` in `@Suite`.
5. **Missing accessibility identifiers in UI tests.** XCUITest queries rely on them.
6. **Using `sleep` in tests.** Use `confirmation`, clock injection, or `withKnownIssue`.
7. **Not testing cancellation.** If code supports `Task` cancellation, verify it cancels cleanly.
8. **Mixing XCTest and Swift Testing in one file.** Keep them in separate files.
9. **Non-Sendable test helpers shared across tests.** Ensure test helper types are Sendable when shared across concurrent test cases. Annotate MainActor-dependent test code with `@MainActor`.

## Test Attachments

Attach diagnostic data to test results for debugging failures. See `references/testing-patterns.md` for full examples.

```swift
@Test func generateReport() async throws {
    let report = try generateReport()
    Attachment(report.data, named: "report.json").record()
    #expect(report.isValid)
}
```

## Exit Testing

Test code that calls `exit()`, `fatalError()`, or `preconditionFailure()`. See `references/testing-patterns.md` for details.

```swift
@Test func invalidInputCausesExit() async {
    await #expect(processExitsWith: .failure) {
        processInvalidInput()  // calls fatalError()
    }
}
```

## Review Checklist

- [ ] All new tests use Swift Testing (`@Test`, `#expect`), not XCTest assertions
- [ ] Test names describe behavior (`fetchUserReturnsNilOnNetworkError` not `testFetchUser`)
- [ ] Error paths have dedicated tests
- [ ] Async tests use `confirmation()`, not `Task.sleep`
- [ ] Parameterized tests used for repetitive variations
- [ ] Tags applied for filtering (`.critical`, `.slow`)
- [ ] Mocks conform to protocols, not subclass concrete types
- [ ] No shared mutable state between tests
- [ ] Cancellation tested for cancellable async operations

## MCP Integration

- **xcodebuildmcp**: Build and run tests directly — full suites, individual functions, tag-filtered runs.
