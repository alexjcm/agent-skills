---
name: writing-junit-tests
description: Generates new Java tests for repository codebases that use JUnit 4, JUnit 5, Spring Boot, Mockito, or Spring test slices. Use when the user asks for unit tests, integration tests, controller tests, repository tests, JUnit tests, or help writing Java tests in Java 8 projects or modern Java 25 + Spring Boot 4 modules. Also trigger when the user says "write tests for", "add tests", "test this class", "generate test coverage", or asks to create tests following existing conventions. Follows the closest repository convention. Do NOT use for migrating existing tests between JUnit versions, debugging or fixing existing tests, refactoring existing test code, non-JUnit test frameworks (TestNG, Spock, pytest, Cypress), Kotlin tests, or documenting code with JavaDoc.
---

# Writing JUnit Tests

Generate new Java tests by following the nearest valid repository convention before using any fallback.

## Scope

This skill supports two repository profiles:

- `legacy`: Java 8 projects that use JUnit 4 or JUnit 5
- `modern`: Java 21+/25 projects, especially Spring Boot 4 modules that use JUnit Jupiter, `jakarta.*`, Gradle 9+, and Spring test slices

Out of scope:
- migrating existing tests between JUnit versions
- non-Java or non-JUnit requests
- test frameworks outside JUnit

## Workflow

1. Find the smallest build unit that owns the target class.
2. Decide whether the user asked for `unit` or `integration` behavior.
3. Detect the local stack from build files and nearby tests.
4. Follow the nearest clear convention for the same test type.
5. Read [`references/templates.md`](references/templates.md) only after the profile and test type are known.
6. Apply the local assertion style using [`references/assertions.md`](references/assertions.md).
7. Verify imports, annotations, source set, and test scope before finalizing code.

## Local Evidence

- `Current package`: the same Java package as the target class, within the relevant test source set.
- `Current module`: the smallest build unit that contains the target class and runs its tests.
- `Clear package convention`: at least 2 valid nearby tests of the same test type using the same style.
- `Clear module convention`: a simple majority inside the current module when the package does not provide enough evidence.
- Do not infer conventions from outside the current module unless the user explicitly asks.

## Stack Detection

Treat these signals as strong evidence for the `modern` profile:

- `org.junit.jupiter.*`, `MockitoExtension`, `@BeforeEach`, `@AfterEach`
- `spring-boot-starter-test` plus `junit-jupiter-*`
- `src/integration-test` or `integrationTest` source sets
- `jakarta.*` imports in the module
- Spring Boot 4 test imports such as `org.springframework.boot.data.jpa.test.autoconfigure.DataJpaTest`
- Gradle 9+ or repo docs that explicitly require Java 21+/25
- local test infrastructure such as `MockMvcControllerBaseTest`, `TestConfiguration`, or custom test bootstraps

Treat these signals as evidence for the `legacy` profile:

- `org.junit.*`, `@RunWith(...)`, `@Before`, `@After`
- `SpringRunner`, `MockitoJUnitRunner`, or older Spring Boot test imports
- modules that clearly target Java 8 and have no strong Jupiter convention

Use executable tests as the strongest evidence when available. If you cannot run tests, prefer strong static evidence from the same module over generic defaults.

## Decision Policy

1. If the user does not specify test type, generate `unit tests`.
2. Generate integration tests only when the user explicitly asks for integration behavior or when the repository already uses an integration source set for that exact target type.
3. Do not infer integration only because the class is a `Controller` or `Repository`.
4. Resolve JUnit version in this order, always for the same test type:
   explicit user request, existing test for the same class, package convention, module convention, stack profile default.
5. `legacy` default: JUnit 4 only when no stronger local evidence exists.
6. `modern` default: JUnit 5 Jupiter.
7. If the user explicitly asks for JUnit 5 against a clearly JUnit 4-only legacy module, warn before diverging.
8. Do not migrate or rewrite existing tests just to align versions.

## Core Rules

- Follow repository conventions only when they are locally clear for the same test type.
- Prefer the narrowest test scope that proves the behavior.
- Keep naming behavior-oriented: `should...When...` or `given...When...Then`.
- When stubbing or verifying Mockito calls with exact known values, pass the values directly instead of wrapping them with `eq(...)`.
- Use Mockito matchers such as `eq(...)`, `any(...)`, or `argThat(...)` only when matcher semantics are actually needed.
- Keep exception assertion lambdas limited to the single invocation expected to throw (SonarQube `java:S5778`).
  Pre-calculate all arguments, builders, fixtures, and helper calls into local variables before `assertThrows` or `assertThatThrownBy`.
  The lambda body must contain only the method call under test with already-resolved arguments — no inline `request(...)`, `filter(...)`, `Vo.builder()...build()`, or date arithmetic like `DAY.plusDays(1)`.
- Do not name test helper methods with Java restricted identifiers (SonarQube `java:S6213`): avoid `record(...)`, `yield(...)`, `var(...)`, and `sealed(...)` as method names. Prefer descriptive alternatives like `buildRecord(...)`, `createRecord(...)`, or `toRecord(...)`.
- Do not introduce skip or assumption annotations to hide slow tests.
- Do not add trivial getter/setter tests unless the user asks.

## Unit Test Guidance

Use for isolated logic with no Spring application context.

Preferred shapes:
- JUnit 4 legacy: `@RunWith(MockitoJUnitRunner.class)`, `@Mock`, `@InjectMocks`, `@Before`
- JUnit 5 modern: `@ExtendWith(MockitoExtension.class)`, `@Mock`, `@InjectMocks`, `@BeforeEach`
- Controller unit tests may be plain method-level tests or isolated `MockMvc` tests without loading Spring

Unit constraints:
- Do not use `@DataJpaTest`, `@WebMvcTest`, or `@SpringBootTest` for pure unit tests.
- `MockMvcBuilders.standaloneSetup(...)` is still unit-style because it does not load a Spring context.
- If the repository has a custom unit-style controller base such as `MockMvcControllerBaseTest`, follow it when it is the local convention.
- Avoid useless Mockito matcher wrappers like `eq(companyId)` when the stubbed method can be written with direct arguments such as `companyId`.

## Integration Test Guidance

Use integration tests only when the user asks for integration behavior or when existing local tests for that target type already use an integration source set.

Choose the narrowest useful scope:
- Repository integration: `@DataJpaTest`
- MVC/controller slice: `@WebMvcTest` or the repository's equivalent focused HTTP test base
- Full context: `@SpringBootTest` only when slice tests are not enough

For Spring Boot 4 and modern modules:
- Prefer Spring slices before `@SpringBootTest`.
- Do not add `SpringExtension` if the chosen slice already provides it unless the repository clearly does so everywhere.
- Prefer `@MockitoBean` for Boot 4 MVC slice collaborators unless the repository already uses another established convention.
- Use imports that match the current module's Spring Boot version instead of assuming older package names.
- Recognize `src/integration-test` as a valid integration-test source set.
- Allow `@TestConfiguration`, `@Import`, `SecurityContextHolder`, and custom test base classes when they are part of the repository's established test infrastructure.

## Assertions Policy

- Follow local assertion style first.
- If the module clearly prefers JUnit Jupiter assertions, prefer static imports such as `assertEquals(...)` instead of qualified calls like `Assertions.assertEquals(...)`.
- If the module already uses qualified `Assertions.*` consistently in nearby tests, keep that local convention instead of rewriting style.
- If the module clearly prefers AssertJ, use AssertJ.
- Group related assertions on the same subject when that improves readability.
- Keep setup outside exception assertion lambdas.

See [`references/assertions.md`](references/assertions.md) for concrete patterns.

## Templates

Use [`references/templates.md`](references/templates.md) after you know which profile applies:
- `legacy` templates for Java 8 + JUnit 4/JUnit 5
- `modern` templates for Java 25 + Spring Boot 4
- repository-specific controller templates when the module uses a custom base class

## Final Verification Checklist

- Confirm requested test type and stack profile.
- Confirm whether the user explicitly requested a JUnit version.
- Confirm the nearest package or module convention for the same test type.
- Confirm the chosen imports and annotations match the module's Spring/JUnit generation.
- Confirm unit tests do not accidentally load Spring context.
- Confirm integration tests use the narrowest slice that satisfies the request.
- Confirm assertion style follows local convention.
- Confirm JUnit assertion methods use static imports unless nearby tests clearly prefer qualified `Assertions.*` calls.
- Confirm Mockito stubs and verifies do not use useless `eq(...)` wrappers for exact known values.
- Confirm exception assertion lambdas contain only the throwing call with pre-resolved arguments — no inline builders, helpers, or computations (`java:S5778`).
- Confirm no test helper method is named with a Java restricted identifier such as `record`, `yield`, `var`, or `sealed` (`java:S6213`).
- Confirm no skip or assumption mechanism was introduced to hide scope problems.
- Confirm the test belongs in the right source set, including `src/integration-test` when applicable.
