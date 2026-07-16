---
name: documenting-java-code
description: Generates JavaDoc for Java classes, constructors, and methods with consistent templates. Use when the user asks to document Java code, add/update JavaDoc, or generate documentation for classes and methods.
---

# Documenting Java Code

This skill generates concise and consistent JavaDoc for Java code, following the local style of the project (legacy or modern) instead of a single rigid convention.

## Activation Signals

Use this skill when the user asks to:
- document Java code
- add JavaDoc
- update JavaDoc
- document a class, constructor, or method

## Out of Scope

- generating unit or integration tests
- migrating to JUnit 5
- documenting non-Java files

## Before Documenting

- Review 2-3 nearby classes (same package, or module if the package has none) to confirm the local language, level of detail, and whether the project already documents fields as an established convention.
- Prefer a clear local convention over the defaults below.

## Decision Policy (Mandatory)

1. Default language: English. Use Spanish instead when the file, its package, or nearby classes are predominantly documented in Spanish.
2. Use English regardless of rule 1 when the user explicitly requests it, or when documenting VOs/DTOs for an external contract whose fields and names are already in English.
3. Never include `@author`. If the user explicitly asks for it in a specific request, follow that instruction only for that request; do not carry it forward as a default.
4. Never include `@throws`.

## Core Rules

### JavaDoc Placement & Scope
- Place **BEFORE** Java annotations.
- Document: Classes, public methods, constructors.
- **DO NOT** document: Fields, unless the project's own convention already documents fields or a field encodes a non-obvious critical business rule.
- **NO** HTML tags (`<p>`, `<br>`).
- No `@author`, no `@throws` in any output (see Decision Policy).

## Rules by Artifact Type

- **VOs/DTOs with Lombok** (`@Data`, `@Value`, `@Getter`/`@Setter`, `@Builder`, `@NoArgsConstructor`, `@AllArgsConstructor`) **or Java `record`**: document only the class/record responsibility. Do not document Lombok-generated or record-generated accessors. Same field rule as above applies.
- **Interfaces**: document each public method that defines the contract (purpose, params, return) — this is the source of truth implementations rely on.
- **Implementations**: use `{@inheritDoc}` when the interface's JavaDoc already covers the contract and the implementation adds no new semantics. Document directly only when the implementation introduces behavior the interface doesn't cover.
- **Endpoints/Controllers**: document the operation exposed, its parameters, and its response, in the language already used by that endpoint/controller.
- **Utility classes**: document the class's responsibility and its relevant public methods.

## Templates

**Class:**
```java
/**
 * [Brief description of class responsibility].
 */
```

**Constructor:**
```java
/**
 * Constructor for dependency injection.
 */
```

**Method:**
```java
/**
 * [Action verb + brief description].
 * 
 * @param paramName short description
 * @return short description
 */
```

**Override:**
```java
/** 
 * {@inheritDoc} 
 */
```

## After Documenting

- This skill only adds/updates JavaDoc; it must not change code logic, signatures, or class/package names.
- Run `git diff --check` on the change.
- If documenting required adding an import (e.g. for `@link`/`@see`), or the diff shows any unintended change to names/imports/signatures, compile the affected module with the project's build tool (Maven or Gradle) when feasible.

## Final Verification Checklist

- Confirm scope is only classes/public methods/constructors, with field documentation added only per the stated exceptions.
- Confirm no `@author` or `@throws` was added.
- Confirm JavaDoc is placed before annotations.
- Confirm language follows local convention, or the explicit English exceptions (user request / external-contract VO).
- Confirm Lombok- or record-generated accessors were not documented.
- Confirm interface contracts are documented and implementations use `{@inheritDoc}` where appropriate.
- Confirm `git diff --check` was run, and the module was compiled if signatures/imports/packages changed.
