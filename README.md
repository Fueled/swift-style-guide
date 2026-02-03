# Fueled Swift Style Guide

- [Introduction](#introduction)
  - [Purpose](#purpose)
  - [Scope](#scope)
  - [Supported Swift Versions](#supported-swift-versions)
  - [Audience](#audience)
  - [How This Guide Is Used (Humans & MCP)](#how-this-guide-is-used-humans--mcp)

- [Core Principles](#core-principles)
  - [Readability](#readability)
  - [Safety](#safety)
  - [Explicitness](#explicitness)
  - [Consistency](#consistency)
  - [Performance Awareness](#performance-awareness)

- [Source File Structure](#source-file-structure)
  - [File Naming](#file-naming)
  - [File Encoding](#file-encoding)
  - [File Headers](#file-headers)
  - [File Comments](#file-comments)
  - [Non-Documentation Comments](#non-documentation-comments)
  - [Import Statements](#import-statements)
    - [Module Imports](#module-imports)
    - [Submodule Imports](#submodule-imports)
    - [@testable Imports](#testable-imports)
  - [Declaration Order](#declaration-order)
  - [Overloaded Declarations Ordering](#overloaded-declarations-ordering)
  - [Access Control Placement & Rules](#access-control-placement--rules)
  - [One Primary Type per File](#one-primary-type-per-file)
  - [MARK Sections](#mark-sections)

- [General Formatting](#general-formatting)
- [Whitespace](#whitespace)
- [Line Wrapping](#line-wrapping)

- [Naming](#naming)

- [Language Fundamentals](#language-fundamentals)
  - [Immutability (`let` vs `var`)](#immutability-let-vs-var)
  - [Type Inference](#type-inference)
  - [Optionals](#optionals)
  - [Implicitly Unwrapped Optionals](#implicitly-unwrapped-optionals)
  - [Control Flow](#control-flow)
  - [Trailing Closure Rules](#trailing-closure-rules)
  - [Pattern Matching](#pattern-matching)
  - [Tuples](#tuples)
  - [Typealiases](#typealiases)
  - [String & Literal Rules](#string--literal-rules)

- [Types & APIs](#types--apis)
  - [Struct vs Class](#struct-vs-class)
  - [Enums](#enums)
  - [Enum Case Layout Rules](#enum-case-layout-rules)
  - [Protocol Design](#protocol-design)
  - [Extensions](#extensions)
  - [Initializers](#initializers)
  - [Factory Methods](#factory-methods)
  - [Operators](#operators)
  - [Public API Design](#public-api-design)

- [Initializers & Instantiation](#initializers--instantiation)

- [Concurrency](#concurrency)

- [Error Handling](#error-handling)

- [Documentation](#documentation)

- [Testing](#testing)

- [Performance](#performance)

- [Anti-Patterns](#anti-patterns)

</br>

## Introduction

### Purpose

This document defines the **Swift style guide** used by the [Fueled](https://fueled.com) team.

Its purpose is to:
- Establish **consistent, readable, and safe Swift code** across all projects
- Encode **language-level rules only**, independent of frameworks or architectures
- Serve as a **single source of truth** for humans, tooling, and MCP-based systems

This guide focuses strictly on **Swift the language**. Application architecture, platform conventions, and third-party frameworks are intentionally out of scope and will be defined in separate documents.

### Scope

The rules in this guide apply to:
- All production Swift code
- All modules and packages written in Swift
- Shared libraries and internal tooling

The guide does **not** define:
- UI architecture (UIKit or SwiftUI)
- Navigation patterns
- Dependency-management frameworks
- Product-specific or feature-specific conventions

### Supported Swift Versions

- **Swift 6** is the default and primary target
- **Swift 5.9** is supported **only** for backward compatibility
- All rules assume **Swift 6 language mode** and **strict concurrency**
- Code must not rely on deprecated or transitional Swift 5 behaviors

### Audience

This guide is intended for:
- Swift engineers writing or maintaining production code
- Code reviewers enforcing consistency and correctness
- Tooling and automation systems (linters, formatters, CI)
- MCP and LLM-based assistants querying rules programmatically


### How This Guide Is Used (Humans & MCP)

- Defines the **authoritative ruleset** for Swift code across the organization
- Acts as the **reference baseline** during code reviews and discussions
- Serves as the **input source** for automated enforcement (linters, CI)
- Enables **deterministic rule lookup** by MCP and LLM-based systems
- Is treated as **read-only output**, not the canonical rule storage


## Core Principles

### Readability

Code is written **to be read by humans first**, not optimized for brevity or cleverness.

- Prefer clear, explicit constructs over concise but opaque ones
- Optimize for the reader who is unfamiliar with the codebase
- Make intent obvious without requiring comments
- Avoid patterns that rely on deep language knowledge to understand
- Favor linear, easy-to-follow control flow

Readability is evaluated over the **lifetime of the code**, including:
- Code reviews
- Debugging
- Refactoring
- Onboarding new engineers

When multiple correct implementations exist, choose the one that is **easiest to understand**, even if it is more verbose.


### Safety

Code is written to **prevent incorrect behavior by construction** rather than relying on discipline or conventions.

- Prefer language features that eliminate entire classes of bugs
- Make invalid states unrepresentable
- Avoid unsafe constructs even when they appear convenient
- Fail early and explicitly when assumptions are violated

Safety principles include:
- Avoiding force unwraps and force casts
- Favoring `guard` and explicit error handling
- Using strong typing to prevent misuse
- Treating concurrency and shared state as safety concerns

Safety takes precedence over performance and brevity unless there is a clear, measured reason to trade it off.


### Explicitness

Code should make its behavior and intent **obvious at the point of use**, without relying on hidden assumptions or implicit knowledge.

- Prefer explicit names over shortened or inferred ones
- Make side effects visible in APIs
- Avoid relying on implicit defaults when they affect behavior
- Require explicit handling of error and concurrency boundaries

Explicitness applies especially to:
- Public APIs
- Concurrency and actor boundaries
- Initialization and state changes
- Error propagation

When implicit behavior improves readability without hiding meaning, it is acceptable.  
When in doubt, prefer being explicit.


### Consistency

Code should follow **the same patterns everywhere**, regardless of author or module.

- Similar problems are solved in similar ways
- Naming, structure, and formatting do not vary by personal preference
- Established patterns are followed even when alternatives exist
- Deviations require a clear, documented reason

Consistency enables:
- Faster code reviews
- Easier navigation across the codebase
- Reliable automation and tooling
- Predictable MCP rule application

Consistency is enforced at the **codebase level**, not per file or per team.

### Performance Awareness

Code is written with an understanding of **performance characteristics**, without sacrificing clarity or safety prematurely.

- Be aware of the cost of allocations, copying, and synchronization
- Prefer value semantics while understanding their copying behavior
- Avoid unnecessary work in hot paths
- Measure before optimizing

Performance considerations include:
- Value vs reference semantics
- Copy-on-write behavior
- Async task creation and cancellation
- Actor isolation and contention

Performance optimizations must be:
- Justified by measurement
- Localized and documented
- Reviewed with the same rigor as correctness changes

Readability and safety are not compromised unless there is a clear, demonstrated performance need.


## Source File Structure

### File Naming

Source file names describe the **primary declaration** contained in the file.

- All Swift source files use the `.swift` extension
- A file containing a single primary type is named after that type
- File names use **UpperCamelCase**, matching the type name exactly
- Do not include prefixes, suffixes, or abbreviations unrelated to the type

```swift
// UserProfile.swift
struct UserProfile { }
```

If a file contains a primary type and closely related helpers, the file is still named after the primary type.

```swift
// Order.swift
struct Order { }

func calculateTax(for order: Order) -> Decimal { }
```


#### Extensions and Protocol Conformance

When a file contains extensions that add protocol conformance, the file name combines:

- The type name
- The protocol name
- A + separator

```swift
// UserProfile+Codable.swift
extension UserProfile: Codable { }
```

If multiple extensions are grouped for organization rather than a single protocol, use a descriptive suffix prefixed with the type name.

```swift
// UserProfile+Extensions.swift
extension UserProfile { }
```

Files that contain related top-level declarations without a clear primary type are named descriptively.

```swift
// MathUtilities.swift
func clamp(_ value: Int, min: Int, max: Int) -> Int { }
```

File names must be predictable and stable, allowing engineers and tools to locate code without opening files.


### File Encoding

All Swift source files are encoded using **UTF-8**.

- UTF-8 is required for all source files
- Do not use alternative encodings
- Source control must preserve UTF-8 encoding without byte-order marks (BOM)

String literals may contain Unicode characters when they improve readability.

```swift
let title = "Überblick"
let emoji = "🚀"
```

Invisible or non-printable characters must not appear directly in source files and should be represented using explicit Unicode escape sequences when required.

UTF-8 encoding is mandatory to ensure consistent behavior across toolchains, platforms, and MCP-based processing.

### File Headers

All Swift source files must begin with the following standardized file header:

```swift
// Created for APP_NAME in 2026
// Using Swift 6.0
```

The header is mandatory for all new Swift files

`APP_NAME` must match the Xcode project or module name

Author names, copyright notices, and dates other than the year are forbidden

The header must appear at the very top of the file, before any imports


### File Comments

File-level comments are **optional** and should be used only when they add meaningful context.

- Do not add file comments when a file contains a single primary type
- Prefer documentation comments on the type itself
- Use file comments only to explain **why** the file exists, not **what** it contains

File comments are appropriate when:
- A file groups multiple related types
- The file provides shared or cross-cutting functionality
- Additional context is required to understand the file’s role

```swift
// Contains shared parsing utilities used by multiple decoders.
// These types are intentionally internal to avoid leaking abstractions.
```

Avoid file comments that restate obvious information.

```swift
// AVOID: Redundant and uninformative
// This file defines the UserProfile struct.
struct UserProfile { }
```

File comments must not be used to:

- Explain implementation details
- Replace proper naming or documentation comments
- Track authorship or change history


### Non-Documentation Comments

Non-documentation comments are used sparingly to explain why code exists, not what the code does.

#### Allowed Comment Style
- Use single-line comments only: `//`
- Block comments (`/* */`) are forbidden
- Do not mix comment styles within a file

```swift
// GOOD
// This cache exists to avoid repeated decoding in hot paths.
```

```swift
// FORBIDDEN
/*
 This cache exists to avoid repeated decoding in hot paths.
*/
```

#### When to Use Comments
- Non-documentation comments are allowed only when they:
- Explain non-obvious intent
- Justify a trade-off or constraint
- Document a known limitation
- Explain a workaround for external behavior

```swift
// Required to work around radar FB123456
let timeout = legacyTimeout
```

#### When Comments Are Forbidden
Do **not** use comments to:
- Restate what the code clearly expresses
- Explain syntax or language features
- Replace good naming or structure
- Track authorship or history

```swift
// AVOID
// Increment index by one
index += 1
```

#### Comment Placement
- Place comments immediately above the code they describe
- Do not place comments inline unless unavoidable
- Do not trail comments at the end of lines

```swift
// GOOD
// Ensures stable ordering across launches
items.sort()
```

```swift
// AVOID
items.sort() // stable ordering
```

#### Comment Tone and Content
- Comments must be factual and concise
- Avoid speculation or personal notes
- Avoid TODOs in comments; use issue tracking instead

```swift
// AVOID
// TODO: fix this later
```

#### Comment Lifecycle
- Comments must be updated or removed when code changes
- Outdated comments are considered bugs
- Comments must never contradict the code


### Import Statements

Import statements declare a file’s **explicit dependencies** and define which modules are available within that file.

The following rules apply to **all** import statements, regardless of type:

- Import statements must appear at the top of the file, after the file header and file comments
- Only import modules that are directly used in the file
- Do not rely on transitive imports
- Remove unused imports immediately
- Imports must be deterministic and predictable
- Imports are sorted **alphabetically within each group**

Import statements are grouped and ordered as follows:

1. Module and submodule imports used in production code
2. Individual declaration imports (when explicitly allowed)
3. `@testable` imports (test targets only)

Each group:
- Is ordered lexicographically (A–Z)
- Is separated from other groups by a single blank line

#### Example

```swift
import CoreLocation
import Foundation
import UIKit

import func Darwin.C.isatty

@testable import MyModuleUnderTest
```

#### Module Imports

Module imports are the **default and preferred** form of importing dependencies.

- Import full top-level modules using `import ModuleName`
- Do not import submodules unless required (see Submodule Imports)
- Do not import individual symbols unless explicitly justified
- Each imported module must be directly used in the file

Module imports must be:
- Explicit
- Minimal
- Alphabetically ordered within their group

##### Examples

```swift
// GOOD
import CoreLocation
import Foundation
import UIKit
```

```swift
// AVOID: Unused import
import Foundation
import UIKit  // UIKit is not used in this file
```

```swift
// AVOID: Relying on transitive imports
import UIKit
// Uses Date without importing Foundation
```

When multiple modules are required, prefer clarity over clever consolidation.
If a module is not used, it must not be imported.

#### Submodule Imports

Submodule imports are **allowed only as an exception** to module imports.

- Prefer importing the top-level module whenever possible
- Import submodules only when required to:
  - Reduce compile time
  - Avoid namespace conflicts
  - Access APIs not exposed by the parent module
- Submodule imports must be justified by necessity, not preference

Submodule imports follow the same ordering rules as module imports:
- Alphabetically ordered within their group
- Grouped with other production imports

##### Examples

```swift
// GOOD: Submodule required for specific API
import SwiftUI.Layout
```

```swift
// GOOD: Submodule import to avoid namespace collision
import SwiftUI.Layout
```

```swift
// AVOID: Unnecessary submodule import
import Foundation.URL
// Foundation import would be sufficient
```

Do not mix submodule imports with individual symbol imports.
If a submodule import becomes common across the codebase, reconsider importing the parent module instead.

#### @testable Imports

`@testable` imports are **restricted to test targets only** and must never appear in production code.

- `@testable` is allowed **only** in test bundles
- Production targets must not depend on `@testable` access
- Use `@testable` to test internal behavior that cannot reasonably be exposed via public APIs
- Do not use `@testable` as a substitute for proper design or dependency injection

`@testable` imports follow these rules:
- Appear **after all non-test imports**
- Are grouped separately
- Are alphabetically ordered if multiple are present

##### Examples

```swift
// GOOD: Test target
import XCTest

@testable import MyApp
```

```swift
// AVOID: Using @testable in production code
@testable import MyApp
```

```swift
// AVOID: Mixing @testable with production imports
@testable import MyApp
import Foundation
```

Excessive reliance on `@testable` is a signal that internal boundaries may need to be revisited.


### Declaration Order

Declarations within a source file follow a **consistent, top-to-bottom order** to improve readability and predictability.

The required order is:

1. Imports  
2. File-level type aliases (if any)  
3. Primary type declaration  
4. Extensions adding protocol conformances  
5. Extensions adding public or internal functionality  
6. **Private extensions containing private helpers**  
7. Nested helper types (if any)

This order must be preserved unless there is a strong, documented reason to deviate.


#### Primary Type

The primary type of the file appears first and matches the file name.

```swift
struct UserProfile {
    let id: UserID
}
```

#### Protocol Conformance Extensions
Use extensions to group protocol conformances.

- One protocol per extension
- No private helpers inside conformance extensions

```swift
extension UserProfile: Codable {
}
```

#### Functional Extensions
Public or internal functionality that is not protocol-driven lives in dedicated extensions.

```swift
extension UserProfile {
    func displayName() -> String {
        "\(firstName) \(lastName)"
    }
}
```

#### Private Extensions
All private functions and private computed properties must be placed in a `private` extension at the bottom of the file.

- Do not mix private helpers with public or internal APIs
- Private extensions exist solely for implementation details
- One or more private extensions are allowed if logically grouped

```swift
private extension UserProfile {
    func normalizedName() -> String {
        displayName().lowercased()
    }
}
```

#### Nested Types
Nested helper types appear after all extensions unless they are essential to the primary type’s definition.

```swift
private extension UserProfile {
    enum ValidationError: Error {
        case missingName
    }
}
```

This structure ensures:

- Clear separation between API and implementation
- Easier scanning for public surface area
- Deterministic structure for tooling and MCP systems


### Overloaded Declarations Ordering

Overloaded declarations must be grouped contiguously to improve readability, discoverability, and correctness.

---

#### General Rules
- Overloaded functions, methods, or initializers must appear adjacent
- No unrelated code may appear between overloads
- Overloads must be ordered from most specific to most general

```swift
// GOOD
func loadUser(id: UserID) async throws -> User
func loadUser(id: UserID, forceRefresh: Bool) async throws -> User
```

```swift
// AVOID
func loadUser(id: UserID) async throws -> User

private func helper() { }

func loadUser(id: UserID, forceRefresh: Bool) async throws -> User
```

#### Initializer Overloads
Initializer overloads must also be grouped together.

```swift
// GOOD
init(id: UserID)
init(id: UserID, name: String)
```

```swift
// AVOID
init(id: UserID)

func validate() { }

init(id: UserID, name: String)
```

#### Overloads Across Extensions
- Overloads may be split across extensions only when:
  - Each extension has a clear, documented purpose
  - Overloads remain contiguous within that extension

```swift
// GOOD
extension UserProfile {
    func update(name: String)
    func update(name: String, email: String)
}
```

```swift
// AVOID
extension UserProfile {
    func update(name: String)
}

extension UserProfile {
    func update(name: String, email: String)
}
```

#### Protocol Conformance and Overloads
- Do not interleave overloads with protocol conformances
- Overloads implementing protocol requirements must remain grouped

```swift
// AVOID
func save()
extension UserProfile: Codable { }
func save(force: Bool)
```

### Access Control Placement & Rules
Access control defines a type’s intended visibility and API surface.
It must be applied intentionally and consistently, without unnecessary verbosity.

---

#### General Rules
- Always use the most restrictive access level possible
- Do not widen access beyond what is required
- Access control is part of the API contract

Access levels, from most restrictive to least:
1. `private`
2. `fileprivate`
3. `internal`
4. `public`
5. `open`


#### Default Access
- Relying on Swift’s default internal access is allowed
- Explicit internal is optional, not required
- Explicit access modifiers are required only when not internal

```swift
// GOOD
struct UserProfile {
}
```

```swift
// ALSO GOOD
internal struct UserProfile {
}
```

```swift
// REQUIRED
public struct UserProfile {
}
```

#### Type-Level Access Control
- Declare access control at the type level when the type is not `internal`
- Members must not have broader access than the enclosing type

```swift
public struct Configuration {
    public let timeout: TimeInterval
}
```

```swift
// AVOID
public struct Configuration {
    let timeout: TimeInterval
}
```

#### Member-Level Access Control

- Apply access control at the member level only when it differs from the type’s access
- Prefer grouping members by access using extensions

```swift
struct UserProfile {
    let id: UserID
}

extension UserProfile {
    func displayName() -> String {
        "\(firstName) \(lastName)"
    }
}

private extension UserProfile {
    func normalizedName() -> String {
        displayName().lowercased()
    }
}
```

#### `private` vs `fileprivate`
- Prefer `private`
- Use `fileprivate` only when multiple types in the same file must share implementation details

```swift
// ACCEPTABLE
fileprivate let cacheKey = "user_cache"
```

```swift
// AVOID
fileprivate func helper() { }
```

If `fileprivate` is common, the file structure likely needs refactoring.

#### Extensions and Access Control
- Do not mix access levels within the same extension
- Each extension has a single access intent

```swift
extension UserProfile {
    func updateName(_ name: String) { }
}

private extension UserProfile {
    func validate() { }
}
```

```swift
// AVOID
extension UserProfile {
    func updateName(_ name: String) { }
    private func validate() { }
}
```

#### `public` and `open`
- `public` is the default for exposed APIs
- `open` is forbidden unless subclassing or overriding is explicitly required
- Public APIs must be stable and documented

```swift
public struct APIClient {
}
```

```swift
// AVOID
open class APIClient {
}
```

#### Access Control and Protocols
- Protocol requirements define the minimum required visibility
- Conforming members must meet or exceed protocol visibility

```swift
public protocol Loadable {
    func load() async throws
}

public struct Loader: Loadable {
    public func load() async throws {
    }
}
```

#### Access Control in Tests
- Production code must not change access levels for testing
- Use `@testable` only in test targets
- Do not expose internals solely for tests


### One Primary Type per File

Each source file must declare **one primary type** that represents the file’s main responsibility.

- A file’s name must match its primary type
- The primary type defines the file’s public surface area
- Additional declarations are allowed only if they directly support the primary type

```swift
// GOOD
// UserProfile.swift
struct UserProfile {
    let id: UserID
}
```

```swift
// AVOID: Multiple primary types in one file
struct UserProfile { }
struct UserSettings { }
```

#### Supporting Declarations

Supporting declarations are allowed when they are:

- Closely related to the primary type
- Not meaningful as standalone types

Examples of allowed supporting declarations:

- Nested types
- Extensions of the primary type
- Private helper types scoped to the primary type

```swift
struct UserProfile {
    let id: UserID

    enum Status {
        case active
        case inactive
    }
}
```

```swift
private extension UserProfile {
    func isValid() -> Bool {
        id.isEmpty == false
    }
}
```

#### Exceptions

Multiple primary types in a single file are allowed only when:

- The file defines tightly coupled value types (e.g. small DTOs)
- Splitting the file would reduce readability

Such exceptions must be rare and justified during code review.

The default rule remains: **one** primary type per file.

### MARK Sections

`MARK:` comments are used to **visually organize code** within a file and must be applied consistently.

- Use `// MARK:` to separate logical sections
- `MARK:` comments improve navigation in Xcode’s jump bar
- Do not overuse `MARK:`; only introduce them when they add clarity
- `MARK:` sections must reflect the file’s declaration order

#### Recommended Usage

Use `MARK:` to separate:
- Lifecycle sections
- Protocol conformances
- Public API vs private implementation
- Logical feature groupings

```swift
struct UserProfile {
    let id: UserID
}

// MARK: - Codable

extension UserProfile: Codable {
}

// MARK: - Public API

extension UserProfile {
    func displayName() -> String {
        "\(firstName) \(lastName)"
    }
}

// MARK: - Private Helpers

private extension UserProfile {
    func normalizedName() -> String {
        displayName().lowercased()
    }
}
```

#### Formatting Rules

- Always include a dash after MARK: when naming a section
- Section titles use Title Case
- Do not nest MARK: sections
- Avoid empty MARK: sections

```swift
// GOOD
// MARK: - Private Helpers

// AVOID
// MARK: Private helpers
// MARK:
// MARK: -- Helpers
```

`MARK:` sections are a navigation aid, not a substitute for good structure or naming.

## General Formatting

General formatting rules ensure **consistent appearance**, improve readability, and enable reliable automated formatting.

These rules apply to all Swift source files unless explicitly stated otherwise.

---

### Line Length

- Maximum line length is **120 characters**
- Lines exceeding the limit must be wrapped
- Do not exceed the limit by disabling formatter rules

```swift
// GOOD
let description = "This is a readable line that fits within the allowed limit."
```

```swift
// AVOID
let description = "This line is far too long and makes the code harder to read and review in diffs, the line should be wrapped."
```

### Indentation

- Use 4 spaces for indentation
- Tabs are forbidden
- Indentation reflects logical nesting, not alignment tricks

```swift
if isValid {
    performAction()
}
```

```swift
// AVOID
if isValid {
  performAction()
}
```

### Braces

- Use K&R brace style
- Opening brace is on the same line
- Closing brace aligns with the start of the declaration

```swift
if isReady {
    start()
}
```

```swift
// AVOID
if isReady
{
    start()
}
```

### Semicolons

- Semicolons are forbidden
- Swift allows semicolons, but they must not be used

```swift
// GOOD
let a = 1
let b = 2
```

```swift
// AVOID
let a = 1; let b = 2
```

### One Statement per Line

- Exactly one statement per line
- Do not combine statements for brevity

```swift
// GOOD
configure()
start()
```

```swift
// AVOID
configure(); start()
```

### Parentheses Usage

- Avoid unnecessary parentheses
- Use parentheses only to clarify precedence

```swift
// GOOD
if isEnabled && isAvailable {
    run()
}
```

```swift
// AVOID
if (isEnabled == true) {
    run()
}
```

### Trailing Commas

- Trailing commas are required in multi-line collections
- Trailing commas are forbidden in single-line collections

```swift
// GOOD
let values = [
    1,
    2,
    3,
]
```

```swift
// AVOID
let values = [1, 2, 3,]
```

### Attributes Formatting

- Place attributes on their own line above the declaration
- Do not inline attributes with declarations

```swift
@MainActor
final class ViewModel {
}
```

```swift
// AVOID
@MainActor final class ViewModel { }
```

### Generated Code Exceptions

- Generated files may ignore formatting rules
- Generated code must be clearly marked
- Do not manually edit generated files

```swift
// This file is generated. Do not edit.
```

Generated code exceptions must be explicit and limited to generated sources only.

## Whitespace

Whitespace is used to **separate logical concepts**, not to control layout or duplicate formatting rules defined elsewhere.

This section intentionally excludes indentation and brace placement, which are defined in **General Formatting**.

---

### Horizontal Whitespace

- Use a single space around binary operators
- No spaces around unary operators
- No trailing whitespace at the end of lines

```swift
// GOOD
let result = a + b
let isValid = !items.isEmpty
```

```swift
// AVOID
let result=a+b
let isValid = ! items.isEmpty
```

### Vertical Whitespace

- Use blank lines to separate logical sections
- Use a single blank line between:
  - Property groups
  - Methods
  - Extensions
- Do not use multiple consecutive blank lines

```swift
struct UserProfile {
    let id: UserID
    let name: String

    func displayName() -> String {
        name
    }
}
```

```swift
// AVOID
struct UserProfile {
    let id: UserID


    let name: String
}
```

### Alignment Rules
- Do not align code using whitespace to form visual columns
- Avoid manual alignment that depends on spacing
- Let structure be expressed through declarations, not spacing

```swift
// GOOD
let shortName = "A"
let longerName = "Alice"
```

```swift
// AVOID
let shortName  = "A"
let longerName = "Alice"
```

### Forbidden Whitespace
The following are forbidden in all Swift source files:

- Trailing whitespace
- Multiple spaces used for alignment
- Blank lines at the beginning or end of files

Forbidden whitespace must be removed automatically by tooling.

Whitespace rules exist to keep diffs clean, intent clear, and formatting deterministic across editors and platforms.

## Line Wrapping

Line wrapping is used to **preserve readability** and **respect the maximum line length** without obscuring intent.

Line wrapping must prioritize clarity over compactness.

---

### General Rules

- Wrap lines only when exceeding the maximum line length
- Prefer wrapping at logical boundaries
- Wrapped lines must be indented consistently
- Do not wrap lines to achieve visual symmetry

### Function Declarations

- Wrap parameters onto separate lines when they do not fit comfortably
- Each parameter appears on its own line
- Closing parenthesis aligns with the declaration

```swift
// GOOD
func fetchUserProfile(
    id: UserID,
    includeSettings: Bool,
    cachePolicy: CachePolicy
) async throws -> UserProfile
```

```swift
// AVOID
func fetchUserProfile(id: UserID, includeSettings: Bool, cachePolicy: CachePolicy) async throws -> UserProfile
```

### Function Calls
- Follow the same rules as function declarations
- One argument per line when wrapped
- Do not mix wrapped and unwrapped arguments

```swift
// GOOD
fetchUserProfile(
    id: userID,
    includeSettings: true,
    cachePolicy: .reloadIgnoringCache
)
```

```swift
// AVOID
fetchUserProfile(id: userID,
                 includeSettings: true,
                 cachePolicy: .reloadIgnoringCache)
```

### Type Declarations

- Prefer keeping inheritance and protocol conformance lists on a **single line**
- Wrap only when the line length limit is exceeded
- Do not wrap solely for visual symmetry

```swift
// GOOD
struct UserProfile: Codable, Equatable, Sendable {
}
```

```swift
// AVOID: Unnecessary wrapping
struct UserProfile:
    Codable,
    Equatable,
    Sendable {
}
```

Wrapping is justified only when required by line length constraints.


### Control Flow Statements

- Wrap conditions when they exceed the line limit
- Each condition appears on its own line
- Logical operators begin the wrapped line

```swift
// GOOD
if isEnabled
    && isAuthorized
    && hasValidSession {
    proceed()
}
```

```swift
// AVOID
if isEnabled && isAuthorized &&
   hasValidSession {
    proceed()
}
```

### Expressions

Expressions should remain **on a single line whenever they fit within the line length limit**.

- Do **not** split expressions unless required by line length
- Do **not** split identifiers or literals
- Preserve natural evaluation order when wrapping
- Prefer readability over aggressive line breaking

When wrapping is required:
- Break lines at operators
- Keep operands visually grouped
- Avoid wrapping that obscures operator precedence

```swift
// GOOD: Fits on one line
let result = baseValue + adjustment * multiplier
```

```swift
// GOOD: Wrapped only because of line length
let result =
    baseValue
    + adjustment
    * multiplier
```

```swift
// AVOID: Unnecessary wrapping
let result =
    baseValue + adjustment * multiplier
```

```swift
// AVOID: Splitting identifiers or literals
let result = base
    Value + adjustment
```

Expressions should be wrapped only when necessary and in a way that preserves clear, left-to-right evaluation.

### Generics & where Clauses

- Place where clauses on a new line when complex
- Each constraint appears on its own line when wrapped

```swift
// GOOD
func process<T>(
    _ value: T
) where
    T: Decodable,
    T: Sendable {
}
```

```swift
// AVOID
func process<T>(_ value: T) where T: Decodable, T: Sendable {
}
```

Line wrapping should make code easier to scan and review.
If wrapping reduces clarity, reconsider the structure of the code.

## Naming

Naming follows the **Swift API Design Guidelines** and prioritizes clarity, consistency, and intent at the call site.

Names are part of the API. Poor naming is considered a correctness issue.

---

### General Rules

- Names must be clear, descriptive, and unambiguous
- Prefer full words over abbreviations
- Avoid encoding type information in names
- Do not use prefixes for namespacing
- Choose names that read naturally at the point of use

### Identifiers

- Use ASCII characters only
- Avoid symbols and emojis in identifiers
- Do not use reserved or overloaded terminology inconsistently

```swift
// GOOD
let userProfile: UserProfile

// AVOID
let usrPrfl: UserProfile
```

### Types

- Use UpperCamelCase
- Use nouns or noun phrases
- Avoid abbreviations unless universally understood

```swift
struct UserProfile { }
enum NetworkError { }
final class ImageLoader { }
```

```swift
// AVOID
struct user_profile { }
class ImgLdr { }
```

### Properties

- Use lowerCamelCase
- Use nouns for values
- Use adjectives or boolean prefixes (is, has, can) for flags

```swift
let title: String
let isEnabled: Bool
let hasPermission: Bool
```

```swift
// AVOID
let Title: String
let enabledFlag: Bool
```

### Functions & Methods

- Use lowerCamelCase
- Use verb-based names
- Names must read naturally at the call site

```swift
func loadUserProfile(id: UserID)
func refreshCache()
```

```swift
// AVOID
func userProfileLoad(id: UserID)
func doRefresh()
```

### Argument Labels

- Use argument labels to clarify intent
- Omit labels only when meaning is obvious

```swift
// GOOD
updateUserProfile(id: userID, name: name)

// AVOID
updateUserProfile(userID, name)
```

### Initializers

- Use clear, labeled parameters
- Avoid ambiguous or overloaded initializers

```swift
init(id: UserID, name: String)
```

```swift
// AVOID
init(_ id: UserID, _ name: String)
```

### Protocols

- Name protocols after capabilities or roles
- Use adjectives or nouns ending in -able when appropriate

```swift
protocol Cacheable { }
protocol Identifiable { }
```

```swift
// AVOID
protocol CacheProtocol { }
protocol UserManagerProtocol { }
```

### Generics

- Use descriptive names for generic parameters
- Use single-letter names only for well-known conventions

```swift
// GOOD
struct Repository<Element> { }

// ACCEPTABLE
struct Stack<T> { }
```

```swift
// AVOID
struct Repository<X> { }
```

### Abbreviations & Acronyms
- Avoid abbreviations unless they are widely understood
- Acronyms of three or more letters are capitalized fully

```swift
let url: URL
let htmlContent: String
```

```swift
// AVOID
let uRL: URL
let HtmlContent: String
```

### Static & Class Members

- Follow the same naming rules as instance members
- Avoid using type names in member names

```swift
static let defaultTimeout: TimeInterval
```

```swift
// AVOID
static let userProfileDefaultTimeout: TimeInterval
```

### Global Constants

- Avoid global constants when possible
- If required, use clear, scoped names

```swift
enum Defaults {
    static let maxRetryCount = 3
}
```

### Delegate & Data Source Methods

- Follow Apple’s established naming conventions
- Method names must clearly express timing and responsibility

```swift
func userProfileDidUpdate(_ profile: UserProfile)
func userProfileWillDelete(_ profile: UserProfile)
```

Naming consistency is enforced across the entire codebase.

If a name requires explanation, it is likely incorrect.



## Language Fundamentals

This section defines **core Swift language usage rules** that apply across all codebases.

---

### Immutability (`let` vs `var`)

- Use `let` by default
- Use `var` only when mutation is required
- Prefer immutable data structures and value semantics

```swift
// GOOD
let timeout: TimeInterval = 10

// AVOID
var timeout: TimeInterval = 10
```

### Type Inference

- Prefer type inference when the type is obvious
- Explicitly specify types at API boundaries and when clarity improves

```swift
// GOOD
let count = 5

// GOOD (API boundary)
let count: Int = 5
```

```swift
// AVOID: Unnecessary explicit type
let count: Int = Int(5)
```

### Optionals

- Use optionals to represent the absence of a value
- Avoid deeply nested optionals
- Prefer early exit with `guard`

```swift
// GOOD
guard let token else { return }
```

```swift
// AVOID
if token != nil {
    use(token!)
}
```


### Implicitly Unwrapped Optionals

- Implicitly unwrapped optionals are forbidden
- Do not use ! for stored properties

```swift
// AVOID
var token: String!
```

If a value is required, it must be:

- Non-optional
- Provided at initialization time
- Or handled explicitly through control flow

### Control Flow

Control flow must prioritize **clarity, exhaustiveness, and explicit intent**.

- Prefer `guard` for early exits
- Prefer `switch` over long or complex `if-else` chains
- Avoid deeply nested conditional logic

#### Early Exit with guard
Use `guard` to handle invalid conditions up front and keep the main logic linear.

```swift
// GOOD
guard isAuthorized else {
    showUnauthorized()
    return
}

loadProtectedData()
```

#### Use `switch` when:
- Handling enums
- Matching multiple conditions
- Exhaustiveness improves safety and readability

```swift
// GOOD
switch state {
case .loading:
    showLoading()
case .loaded(let data):
    showData(data)
case .failed(let error):
    showError(error)
}
```

`switch` statements must be exhaustive.

Use a default case only when all remaining cases are intentionally handled the same way.

#### Avoiding if-else Chains

Long `if-else` chains reduce readability and are prone to errors.

```swift
// AVOID
if state == .loading {
    showLoading()
} else if state == .loaded {
    showData()
} else if state == .failed {
    showError()
}
```

### Trailing Closure Rules

Trailing closures are used to improve **call-site readability** when working with closure parameters.  
They must make intent clearer, not shorter.

#### Single Closure Rule

- Use trailing closure syntax when a function accepts **a single closure**
- The closure must be the **last parameter**
- Trailing closure usage must read naturally at the call site

```swift
// GOOD
items.forEach { item in
    process(item)
}
```

```swift
// AVOID
perform(action: { doWork() })
```

#### Multiple Closures
When a function accepts multiple closure parameters, use named trailing closures.
This is the preferred form for multiple closures in modern Swift.

```swift
// GOOD
loadData {
    handleSuccess()
} onFailure: { error in
    handleError(error)
}
```

```swift
// AVOID
loadData(
    onSuccess: {
        handleSuccess()
    },
    onFailure: { error in
        handleError(error)
    }
)
```

#### Readability Over Brevity

Prefer the form that is **most idiomatic and readable at the call site**, even if it is more concise.

For APIs where the trailing closure clearly represents the primary behavior (such as animations), **trailing closure syntax is preferred**.

```swift
// PREFERRED
animate(duration: 0.3) {
    view.alpha = 1
}
```

```swift
// AVOID: Verbose without added clarity
animate(
    duration: 0.3,
    animations: {
        view.alpha = 1
    }
)
```

#### Nested Trailing Closures
- Avoid deeply nested trailing closures
- Extract logic into named functions or variables

```swift
// AVOID
items.filter {
    $0.isValid
}.map {
    transform($0)
}.forEach {
    save($0)
}
```

```swift
// GOOD
let validItems = items.filter(isValid)
let transformed = validItems.map(transform)
transformed.forEach(save)
```

#### Trailing Closures and Async
- Do not use trailing closures to simulate asynchronous APIs
- Prefer `async` / `await` over callback-style closures

```swift
// GOOD
let data = try await loadData()
```

```swift
// AVOID
loadData { data in
    handle(data)
}
```

### Pattern Matching

- Prefer pattern matching for clarity
- `switch` statements must be exhaustive

```swift
switch result {
case .success(let value):
    handle(value)
case .failure(let error):
    handle(error)
}
```

### Tuples

- Use tuples only for short-lived, local grouping
- Do not use tuples as public return types or stored properties

```swift
// GOOD
let (x, y) = point
```

```swift
// AVOID
func fetch() -> (Int, String)
```

#### Tuple Pattern Matching

Tuples may be used in `switch` statements when matching **multiple related values** improves clarity.

- Use tuple pattern matching only when values are conceptually related
- Prefer exhaustive `switch` statements
- Avoid tuple patterns that obscure intent

```swift
// GOOD
switch (state, isRetrying) {
case (.loading, _):
    showLoading()
case (.failed, true):
    retry()
case (.failed, false):
    showError()
case (.loaded, _):
    showContent()
}
```

```swift
// AVOID: Tuple pattern hides intent
switch (state, isRetrying) {
case let (.failed, retry):
    handle(state, retry)
}
```

#### Wildcard (`_`) Usage in Tuples
- Use `_` only when a value is truly irrelevant
- Avoid using `_` for multiple values in the same pattern

```swift
// GOOD
case (.success, _):
```

```swift
// AVOID
case (_, _):
```

Overuse of `_` reduces readability and exhaustiveness clarity.

#### `where` Clauses with Tuples
Use `where` clauses to add simple constraints, not complex logic.

```swift
// GOOD
switch point {
case let (x, y) where x == y:
    handleDiagonal()
default:
    handleOther()
}
```

```swift
// AVOID
case let (x, y) where x > 0 && y > 0 && x.isMultiple(of: 2):
```

Extract complex conditions into named functions.

#### Avoid Tuple Overuse
Tuples must not:
- Replace domain types
- Encode business logic compactly
- Grow beyond two or three elements

```swift
// AVOID
let result: (Int, String, Bool, Date)
```

If a tuple requires documentation, it should be a struct.



### Typealiases

- Use `typealias` to clarify intent
- Avoid aliases that obscure meaning or hide structure

```swift
typealias UserID = UUID
```

```swift
// AVOID
typealias Payload = [String: Any]
```

### String & Literal Rules

This section defines strict rules for string and literal usage to ensure correctness, readability, and predictable behavior across tools and platforms.

#### String Literals
- Prefer plain string literals (`"..."`) for simple text
- Use multiline string literals (`"""`) only when structure or formatting matters
- Avoid unnecessary interpolation

```swift
// GOOD
let title = "User Profile"

// AVOID
let title = "\( "User" ) Profile"
```

#### Multiline String Literals
Use multiline strings only for:
- Templates
  - Large blocks of text
  - Test fixtures
- Do not indent content for visual alignment unless indentation is meaningful

```swift
let message = """
Welcome back.
Please sign in to continue.
"""
```

```swift
// AVOID: Accidental indentation
let message = """
    Welcome back.
    Please sign in to continue.
"""
```

#### String Interpolation
- Use interpolation only when it improves clarity
- Avoid complex expressions inside interpolation
- Do not perform logic inside interpolation

```swift
// GOOD
let message = "Hello, \(user.name)"
```

```swift
// AVOID
let message = "Total: \(items.filter { $0.isValid }.count)"
```

Precompute values instead.

#### Unicode and Escapes
- Unicode characters are allowed when they improve readability
- Avoid mixing visible Unicode with escape sequences for the same purpose
- Invisible or control characters must use explicit escapes

```swift
// GOOD
let title = "Café"
```

```swift
// GOOD
let newline = "\n"
```

```swift
// AVOID: Mixed representation
let text = "Caf\u{00E9}"
```

#### Invisible Characters
- Invisible Unicode characters (zero-width space, variation selectors) must not appear directly in source
- Use escape sequences when such characters are required

```swift
// GOOD
let zeroWidthSpace = "\u{200B}"
```

#### Numeric Literals
- Use `_` to group digits for readability
- Group digits consistently by thousands or domain meaning

```swift
let maxUsers = 10_000
let timeoutInNanoseconds = 1_000_000_000
```

```swift
// AVOID
let value = 10000
let other = 1_00_00
```

#### Boolean Literals
- Do not compare booleans explicitly to `true` or `false`

```swift
// GOOD
if isEnabled {
}
```

```swift
// AVOID
if isEnabled == true {
}
```

#### Collection Literals
- Prefer literal syntax over initializers when clear
- Use explicit type annotations when the literal is ambiguous

```swift
// GOOD
let values = [1, 2, 3]
```

```swift
// GOOD (clarity)
let values: [Int] = []
```

```swift
// AVOID
let values = Array<Int>()
```

#### Dictionary Literals

- Keys and values must be clearly readable
- Avoid deeply nested dictionary literals
- Do not use `[String: Any]` as a general-purpose structure

```swift
let headers = [
    "Accept": "application/json",
    "Authorization": token,
]
```

#### Literal Consistency
- Use the same literal style for the same concept across the codebase
- Do not mix numeric bases or representations without reason

```swift
// GOOD
let mask = 0b1101
```

```swift
// AVOID
let mask = 13
```

## Types & APIs

This section defines rules for designing **clear, safe, and stable APIs** using Swift’s type system.

---

### Struct vs Class

- Prefer `struct` by default
- Use `class` only when **identity**, **shared mutable state**, or **inheritance** is required
- Do not use classes solely for reference semantics without justification

```swift
// GOOD
struct ViewState {
    let title: String
}
```

```swift
// ACCEPTABLE
final class ImageCache {
    private var storage: [URL: Image] = [:]
}
```

### Enums

- Use enums to model finite states
- Prefer associated values over parallel data structures
- Keep cases concise and expressive

```swift
enum LoadState {
    case idle
    case loading
    case loaded(Data)
    case failed(Error)
}
```

- Avoid “catch-all” cases that hide intent

```swift
// AVOID
enum Status {
    case ok
    case error
    case unknown
}
```

### Enum Case Layout Rules

Enum cases must be laid out to maximize **readability**, **exhaustiveness**, and **long-term API stability**.

#### One Case per Line

- Declare **one enum case per line**
- Do not combine multiple cases on a single line

```swift
// GOOD
enum LoadState {
    case idle
    case loading
    case loaded
    case failed
}
```

```swift
// AVOID
enum LoadState {
    case idle, loading, loaded, failed
}
```

#### Associated Values Formatting
- Each case with associated values appears on its own line
- Prefer named associated values for clarity

```swift
// GOOD
enum ResultState {
    case success(value: Data)
    case failure(error: Error)
}
```

```swift
// AVOID
enum ResultState {
    case success(Data)
    case failure(Error)
}
```

#### Grouping Related Cases
- Group logically related cases together
- Order cases to reflect lifecycle or state flow
- Use a blank line to separate groups only when it improves readability

```swift
enum ConnectionState {
    case disconnected
    case connecting
    case connected

    case failed(error: Error)
}
```

#### Raw Value Enums
- Declare raw values explicitly when clarity improves
- Do not rely on implicit raw value ordering for behavior

```swift
enum HTTPMethod: String {
    case get = "GET"
    case post = "POST"
    case put = "PUT"
    case delete = "DELETE"
}
```

#### Case Order Stability
- Do not reorder cases without a strong reason
- Case order affects diffs, reviews, and public API expectations

#### Exhaustiveness Expectations
- Design enums to support exhaustive `switch` statements
- Avoid “catch-all” cases unless explicitly justified

```swift
// AVOID
enum Status {
    case success
    case failure
    case unknown
}
```


### Protocol Design

- Design protocols around capabilities, not implementations
- Keep protocols small and focused
- Avoid “god protocols”

```swift
protocol Cacheable {
    func load() throws
    func save() throws
}
```

```swift
// AVOID
protocol UserManagerProtocol {
    func load()
    func save()
    func delete()
    func refresh()
}
```

### Extensions

- Use extensions to organize functionality
- Group protocol conformances in dedicated extensions

```swift
extension UserProfile: Codable {
}
```
```swift
extension UserProfile {
    func displayName() -> String {
        "\(firstName) \(lastName)"
    }
}
```

### Initializers

- Prefer explicit, labeled initializers
- Avoid ambiguous or overloaded initializers
- Do not use unlabeled parameters unless meaning is obvious

```swift
init(id: UserID, name: String)
```

```swift
// AVOID
init(_ id: UserID, _ name: String)
```

### Factory Methods

- Use factory methods when initialization requires logic
- Factory methods must clearly describe the created variant

```swift
static func cached(_ value: Value) -> CacheEntry
```

```swift
// AVOID
static func create(_ value: Value) -> CacheEntry
```

### Operators

- Avoid custom operators unless they model a well-understood domain concept
- Operator behavior must be unsurprising
- Prefer named functions for clarity

```swift
// AVOID
infix operator <~>
```

### Public API Design

- Public APIs must be minimal and intentional
- Do not expose implementation details
- Favor immutability in public interfaces

```swift
public struct Configuration {
    public let timeout: TimeInterval
}
```

```swift
// AVOID
public var internalState: [String: Any]
```

API design decisions are hard to change.

Favor clarity, stability, and long-term maintainability over convenience.

## Initializers & Instantiation

This section defines rules for **object creation**, **initializer behavior**, and **construction semantics**.  
It complements *Types & APIs* by focusing on **how instances are created**, not just how they are named.

Initialization must be **deterministic, explicit, and side-effect free**.

---

### Designated vs Convenience Initializers

- Prefer **designated initializers**
- Avoid `convenience init` unless required by:
  - Class inheritance
  - Objective-C interop
- `convenience init` must delegate immediately and perform no additional logic

```swift
// GOOD
final class Cache {
    let capacity: Int

    init(capacity: Int) {
        self.capacity = capacity
    }
}
```

```swift
// AVOID
convenience init() {
    self.init(capacity: 100)
    preload() // side effect
}
```

### Memberwise Initializers
- Rely on compiler-synthesized memberwise initializers for `struct`s when possible
- Defining a custom initializer disables the synthesized one
- Be intentional when exposing memberwise initializers in public APIs

```swift
// GOOD
struct Configuration {
    let timeout: TimeInterval
    let retries: Int
}
```

```swift
// AVOID: Unnecessary custom init
struct Configuration {
    let timeout: TimeInterval
    let retries: Int

    init(timeout: TimeInterval, retries: Int) {
        self.timeout = timeout
        self.retries = retries
    }
}
```

If a `struct` is `public`, define an explicit initializer only when:
- Validation is required
- Invariants must be enforced
- Defaults alone are insufficient

### Failable Initializers (`init?`)
- Use failable initializers only when failure is expected and local
- Prefer `throws` when failure needs explanation or recovery
- Do not use `init?` for validation that can throw meaningful errors

```swift
// ACCEPTABLE
init?(rawValue: String)
```

```swift
// PREFERRED
init(value: String) throws
```

Avoid chaining failable initializers that obscure failure reasons.

### Default Values vs Initializer Overloads
- Prefer default parameter values over multiple initializers
- Avoid initializer explosion

```swift
// GOOD
init(timeout: TimeInterval = 10, retries: Int = 3)
```

```swift
// AVOID
init(timeout: TimeInterval)
init(timeout: TimeInterval, retries: Int)
init(timeout: TimeInterval, retries: Int, cache: Bool)
```

Defaults improve readability and reduce API surface area.

### Initialization Side Effects
Initializers must be **pure construction only**.

Forbidden inside `init`:
- Network calls
- Disk I/O
- Async work
- Task creation
- Global state mutation
- Singleton access

```swift
// FORBIDDEN
init() {
    Task {
        await load()
    }
}
```

Initialization must:
- Assign stored properties
- Enforce invariants
- Perform lightweight validation only

### Async Setup and Factories
- Initializers cannot be `async`
- Use factory methods for async setup

```swift
// GOOD
static func load() async throws -> Repository
```

```swift
// AVOID
init() async throws
```

Factories clearly communicate suspension and failure.

### Instantiation Consistency
- Object creation must be deterministic
- Initializers must not depend on:
  - Global mutable state
  - Singletons
  - Hidden environment assumptions

```swift
// AVOID
init() {
    self.client = NetworkClient.shared
}
```

Dependencies must be injected explicitly.

### Actor Initialization
- Actors may initialize stored properties only
- Do not perform cross-actor calls in `init`
- Use post-init async setup if required

```swift
actor Cache {
    private let capacity: Int

    init(capacity: Int) {
        self.capacity = capacity
    }
}
```

### Instantiation Clarity
- Prefer explicit construction over helpers with hidden behavior
- Instance creation must be obvious at the call site

```swift
// GOOD
let cache = Cache(capacity: 100)
```

```swift
// AVOID
let cache = makeDefaultCache()
```

Unless the factory name clearly communicates behavior.

## Concurrency

Concurrency rules prioritize **safety, clarity, and correctness** under **Swift 6 strict concurrency**.

All concurrent code must be written assuming **data races are compile-time errors**, not runtime bugs.

---

### async / await

- Use `async` / `await` exclusively for asynchronous work
- Completion handlers are forbidden in new code
- Async functions must clearly express suspension points

```swift
// GOOD
func loadUser() async throws -> User
```

```swift
// AVOID
func loadUser(completion: @escaping (Result<User, Error>) -> Void)
```

Async functions must:

- Be cancellable when appropriate
- Propagate errors using `throws`

### Tasks

- Use `Task {}` only at concurrency boundaries
- Avoid creating detached tasks without strong justification
- Prefer structured concurrency

```swift
// GOOD
Task {
    await viewModel.load()
}
```

```swift
// AVOID
Task.detached {
    await load()
}
```

Detached tasks are allowed only when:

- No parent task exists
- Cancellation and lifetime are explicitly handled


### Cancellation

- Long-running async operations must support cancellation
- Check for cancellation at logical boundaries

```swift
try Task.checkCancellation()
```

```swift
if Task.isCancelled {
    return
}
```

Ignoring cancellation is considered a correctness issue.


### Actors

- Use actors to protect mutable shared state
- Do not manually synchronize actor-protected state
- Actor boundaries must be explicit

```swift
actor ImageCache {
    private var storage: [URL: Image] = [:]

    func image(for url: URL) -> Image? {
        storage[url]
    }
}
```

Actors replace locks, queues, and manual synchronization.

### @MainActor

- Use @MainActor for UI-facing state and logic
- Do not perform heavy work on the main actor

```swift
@MainActor
final class ViewModel {
    func refresh() async {
    }
}
```

Avoid hopping between actors unnecessarily.

### Isolation

- Respect actor isolation boundaries
- Avoid `nonisolated` unless strictly required

```swift
await cache.image(for: url)
```

Breaking isolation rules is forbidden.

### Sendable

- All types crossing concurrency boundaries must conform to `Sendable`
- Prefer compiler-checked `Sendable` conformance
- Avoid `@unchecked Sendable` as much as possible

```swift
struct Request: Sendable {
    let id: UUID
}
```

```swift
// AVOID
final class State: @unchecked Sendable {
}
```

### @Sendable Closures

- Closures passed across concurrency boundaries must be `@Sendable`
- Captured values must be immutable or `Sendable`

```swift
let work: @Sendable () async -> Void = {
    await perform()
}
```

### Thread Safety Rules

- Shared mutable state outside actors is forbidden
- Do not use locks, semaphores, or dispatch queues for synchronization
- Actors are the default synchronization primitive


Concurrency code must be correct by construction.

If concurrency safety is unclear, the design must be revisited.


## Error Handling

Error handling must be **explicit, intentional, and type-driven**.  
Errors are part of the API contract and must be designed, documented, and propagated deliberately.

---

### Error Types

- Define errors as concrete `enum` types conforming to `Error`
- Error cases must be descriptive and domain-specific
- Avoid throwing opaque or generic errors without context

```swift
enum NetworkError: Error {
    case offline
    case unauthorized
    case timeout
}
```

```swift
// AVOID: No defined error domain
func fetchData() throws -> Data
```

Functions that throw must define or document the error types they can produce.

### Error Contracts

- Public and internal APIs must clearly communicate failure modes
- Thrown errors must be:
  - Defined via a concrete error type, or
  - Explicitly documented in the API contract


```swift
/// Loads user data.
/// - Throws: `NetworkError.offline` if no connection is available.
///           `NetworkError.unauthorized` if authentication fails.
func loadUser() async throws -> User
```

Undocumented error behavior is forbidden.


### `throws` vs `Result`

- Prefer `throws` for synchronous and `async` functions
- Use `Result` only when required by:
  - Legacy APIs
  - Callback-based interfaces
  - Value-based error composition


```swift
// GOOD
func loadData() async throws -> Data
```

```swift
// ACCEPTABLE (API boundary)
func loadData(completion: (Result<Data, NetworkError>) -> Void)
```

Do not wrap `throws` inside `Result` without a strong reason.


### `try` Usage

- Use `try` for recoverable failures
- Use `try?` only when failure is intentionally ignored and safe
- `try!` is forbidden

```swift
// GOOD
let data = try loadData()
```

```swift
// ACCEPTABLE
let cached = try? loadCachedData()
```

```swift
// FORBIDDEN
let data = try! loadData()
```

### Error Propagation

- Propagate errors upward when the caller can handle them
- Do not silently swallow errors
- Catch errors only when meaningful handling or mapping occurs

```swift
func refresh() async throws {
    try await service.load()
}
```

```swift
// AVOID: Swallowing errors
do {
    try load()
} catch {
}
```

### Error Mapping

- Map low-level errors to higher-level domain errors at boundaries
- Do not leak implementation-specific errors across layers or modules

```swift
func fetchUser() async throws -> User {
    do {
        return try await api.fetch()
    } catch {
        throw UserError.loadingFailed
    }
}
```

Error mapping must preserve intent and avoid information loss when possible.


### Error Handling in Control Flow

- Handle specific error cases explicitly
- Prefer `switch` or specific `catch` clauses
- Avoid broad `catch` blocks when behavior differs by error type

```swift
do {
    try perform()
} catch NetworkError.offline {
    showOfflineMessage()
} catch {
    showGenericError()
}
```

### Fatal Errors

- `fatalError` is forbidden in production code
- Allowed only for:
  - Unreachable states
  - Programmer errors
  - Test-only helpers

```swift
fatalError("Unreachable code path")
```

Recoverable failures must never use fatalError.


### Error Documentation

- All throwing public APIs must document their errors
- Documentation must describe **when** and **why** errors occur

```swift
/// - Throws: `UserError.invalidID` if the identifier is malformed.
func loadUser(id: UserID) async throws
```

Error handling must make failure explicit, predictable, and intentional.

If a failure is possible, it must be modeled, documented, and handled deliberately.



## Documentation

Documentation exists to **explain intent, behavior, and contracts**.  
It complements clear code but does not replace it.

### General Rules

- Write documentation for **public APIs**
- Internal code is documented only when intent is not obvious
- Documentation must describe **what** and **why**, not **how**
- Keep documentation accurate and up to date

Redundant or outdated documentation is worse than no documentation.

### Documentation Comments

- Use Swift documentation comments (`///`)
- Place comments immediately above the documented declaration
- Use complete sentences

```swift
/// Loads the user profile for the given identifier.
func loadUserProfile(id: UserID) async throws -> UserProfile
```

```swift
// AVOID
// Loads user
func loadUserProfile(id: UserID) async throws -> UserProfile
```

### Public vs Internal APIs

- **Public APIs** must be documented
- **Internal APIs** should be documented only if non-trivial
- **Private APIs** are not documented

```swift
/// Represents a user-visible configuration value.
public struct Configuration {
    public let timeout: TimeInterval
}
```

### Parameters and Return Values

- Document parameters when their meaning is not obvious
- Document return values when behavior is non-trivial

```swift
/// Fetches data from the server.
/// - Parameter forceRefresh: Forces a network request when true.
/// - Returns: Cached or freshly fetched data.
func fetchData(forceRefresh: Bool) async throws -> Data
```

### Documentation Style

- Use concise, professional language
- Avoid implementation details
- Avoid restating type names or obvious information

```swift
// AVOID
/// Loads user data from the database and parses it and assigns it to properties.
```

### Examples in Documentation

- Use examples when behavior is not obvious
- Keep examples minimal and correct

```swift
/// Example:
/// ```swift
/// let user = try await service.loadUser()
/// ```
```

### TODOs and Notes

- Avoid TODOs in documentation
- Use issue tracking instead
- Temporary notes must not ship in production code



## Testing

All tests are written using **Swift Testing**.  
`XCTest` is not used in new or existing code.

Testing rules focus on **language usage, structure, and clarity**, not test architecture.


---

### Testing Framework

- Use **Swift Testing** exclusively
- Do not introduce `XCTest` imports
- Tests must compile and run under Swift 6 strict concurrency


```swift
import Testing
```

```swift
// AVOID
import XCTest
```

### Test Suites

- Each test file defines a single test suite
- Test suites are declared using `@Suite`
- The suite name must match the file name
- Test suites are declared as `struct`

```swift
@Suite("UserProfileTests")
struct UserProfileTests {
}
```

### Test Structure

- Tests are declared inside the test suite `struct`
- Each test validates one behavior
- Tests must be small and focused

```swift
@Suite("UserProfileTests")
struct UserProfileTests {

    @Test("Loading a valid user returns a populated profile")
    func loadingUserReturnsProfile() async throws {
        let user = try await loadUser()
        #expect(user.id.isEmpty == false)
    }
}
```

### Test Naming

- Function names use **lowerCamelCase**
- Function names describe the behavior under test
- The `@Test` attribute message provides a human-readable description

```swift
@Test("Saving a profile persists the data")
func savingProfilePersistsData()
```

```swift
// AVOID
@Test("Test saving")
func testSave()
```

### Assertions

- Use `#expect` for all assertions
- Assertions must be explicit and readable
- Avoid combining multiple conditions in one assertion

```swift
#expect(count == 3)
```

```swift
// AVOID
#expect(count == 3 && items.isEmpty == false)
```

### Async Tests

- Async behavior is tested using `async` test functions
- Do not block threads or use sleeps
- Await real suspension points

```swift
@Test("Fetching data returns a non-empty result")
func fetchDataReturnsResult() async throws {
    let data = try await fetchData()
    #expect(data.isEmpty == false)
}
```

### Error Testing

- Error cases must be tested explicitly
- Prefer matching specific error cases

```swift
@Test("Loading without network throws offline error")
func loadingWithoutNetworkThrowsOfflineError() async {
    await #expect(throws: NetworkError.offline) {
        try await loadUser()
    }
}
```

### Test Isolation

- Tests must be independent
- No shared mutable state between tests
- Tests must be order-independent


### What Tests Must Not Do

- Depend on execution order
- Rely on global mutable state
- Use sleeps or timing assumptions
- Test multiple unrelated behaviors


## Performance

Performance rules focus on **Swift language and runtime behavior**.  
They do not define application-level optimization strategies.

Performance considerations must never compromise **correctness, safety, or readability** without measurement.

---

### General Principles

- Do not optimize without evidence
- Measure before and after any optimization
- Prefer clear code unless performance impact is proven
- Optimize locally and document the reason

Premature optimization is forbidden.

### Value vs Reference Semantics

- Prefer value types (`struct`, `enum`)
- Be aware of copying costs
- Understand copy-on-write behavior

```swift
struct Configuration {
    let timeout: TimeInterval
}
```

Value types are cheap until mutated.
Avoid unnecessary mutation of large value types.

### Copy-on-Write Awareness

- Copy-on-write collections are efficient when not mutated
- Mutating shared values may trigger full copies

```swift
var values = largeArray
values.append(1) // may trigger a copy
```

Avoid mutating shared large collections in hot paths.

### Allocation Awareness

- Avoid unnecessary object creation
- Prefer reuse when safe and meaningful
- Be mindful of temporary allocations in tight loops

```swift
// AVOID
for _ in 0..<1000 {
    let formatter = DateFormatter()
}
```

```swift
// GOOD
let formatter = DateFormatter()
for _ in 0..<1000 {
}
```

### `final` Usage

- Mark classes as `final` unless subclassing is required
- `final` enables better compiler optimizations

```swift
final class ImageLoader {
}
```

### `lazy` Properties

- Use `lazy` for expensive initialization
- Do not use `lazy` to hide architectural problems

```swift
lazy var formatter = DateFormatter()
```

### Inlining and Visibility

- Avoid @inlinable unless you understand its impact
- Do not expose implementation details for performance
- Let the compiler optimize by default

Manual inlining is rarely necessary.


### Async Performance

- Avoid unnecessary task creation
- Prefer structured concurrency
- Be mindful of actor contention

```swift
// AVOID If not needed
Task {
    await doWork()
}
```

Create tasks only at defined concurrency boundaries.


### Performance Documentation

- Document non-obvious performance decisions
- Include reasoning and measurement context

```swift
// Performance note:
// This cache avoids repeated JSON decoding in hot paths.
```

## Anti-Patterns

This section lists **forbidden or strongly discouraged patterns** that lead to fragile, unsafe, or unmaintainable Swift code.

Anti-patterns are violations of intent, not personal style differences.

---

### Force Unwrapping (`!`)

- Force unwrapping is forbidden
- All optional values must be handled explicitly

```swift
// FORBIDDEN
let token = token!
```

Use `guard`, `if let`, or redesign the API.


### Force Casting (`as!`)

- Force casting is forbidden
- Use safe casting with `as?` or redesign types

```swift
// FORBIDDEN
let view = object as! UIView
```

### Implicitly Unwrapped Optionals

- Implicitly unwrapped optionals are forbidden
- SwiftUI does not require them

```swift
// FORBIDDEN
var value: String!
```

### Shared Mutable State

- Shared mutable state outside of actors is forbidden
- Global mutable state is forbidden

```swift
// FORBIDDEN
var cache: [String: Any] = [:]
```

### Overuse of `Any`

- Avoid `Any` and `AnyObject` if possible
- Use strongly typed models instead

### Massive Types

- Types with too many responsibilities are forbidden
- Large types must be split or refactored

Indicators of massive types:
- Hundreds of lines
- Many unrelated methods
- High cognitive load

### Silent Failure

- Swallowing errors is forbidden
- Ignoring return values that indicate failure is forbidden

```swift
// FORBIDDEN
do {
    try perform()
} catch {
}
```

### Magic Numbers and Strings

- Avoid hard-coded literals with unclear meaning
- Use named constants

```swift
// FORBIDDEN
if retries > 3 {
}
```

```swift
// GOOD
if retries > maxRetryCount {
}
```

### Premature Optimization

- Optimizing without measurement is forbidden
- Performance changes must be justified

```swift
// FORBIDDEN
var buffer = UnsafeMutablePointer<Int>.allocate(capacity: 1024)
```

### Excessive Comments

- Comments that explain what the code does are discouraged
- Code should be self-explanatory

```swift
// AVOID
// Increment i by one
i += 1
```