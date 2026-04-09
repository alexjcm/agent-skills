---
name: documenting-java-code
description: Generates JavaDoc for Java classes, constructors, and methods with consistent templates. Use when the user asks to document Java code, add/update JavaDoc, or generate documentation for classes and methods.
---

# Documenting Java Code

This skill generates concise and consistent JavaDoc for Java code.

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

## Decision Policy (Mandatory)

1. If the user does not specify language, generate JavaDoc in English.
2. If class-level JavaDoc is requested and author is missing, ask the user for the author value before generating final output.
3. Include `@author` only at class level.

## Core Rules

### JavaDoc Placement & Scope
- Place **BEFORE** Java annotations
- Document: Classes, Public methods, Constructors
- **DO NOT** document: Fields
- Include `@author` **only** in class-level JavaDoc.
- Do **NOT** include `@author` in method or constructor JavaDoc.

### Language & Format
- Default: **English**, unless the user explicitly requests another language.
- **NO** HTML tags (`<p>`, `<br>`)
- Include `@throws` when the documented method can throw relevant exceptions.

## Templates

**Class:**
```java
/**
 * [Brief description of class responsibility].
 * 
 * @author [AuthorName]
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
 * @throws ExceptionType short condition when relevant
 */
```

**Override:**
```java
/** 
 * {@inheritDoc} 
 */
```

## Final Verification Checklist

- Confirm scope is only class/public methods/constructors.
- Confirm field documentation was not added.
- Confirm JavaDoc is placed before annotations.
- Confirm class-level JavaDoc includes `@author`.
- Confirm method/constructor JavaDoc does not include `@author`.
- Confirm `@throws` is included when relevant.
- Confirm language requested by user (default English).
