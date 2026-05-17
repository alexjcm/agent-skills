---
name: writing-junit-tests
description: Generates new Java tests in JUnit 4 or JUnit 5, following the closest repository convention for the same class, package, or module. Defaults to unit tests unless the user explicitly requests integration tests, defaults to JUnit 4 when no stronger convention is found, and does not migrate existing tests.
---

# Writing JUnit Tests

This skill generates new Java tests with deterministic selection of test type and JUnit version. It follows the nearest valid repository convention before falling back to defaults.

## Activation Signals

Use this skill when the user asks to:
- write tests for Java code
- create unit tests
- create integration tests
- write JUnit 5 or Jupiter tests
- write tests using `@ExtendWith`, `@BeforeEach`, or `@AfterEach`
- generate test classes for service/controller/repository code

## Out of Scope

- migrating existing tests between JUnit versions
- non-Java or non-JUnit requests
- test frameworks outside JUnit

## Analysis Scope

- `Current package`: the same Java package as the target class, within the same test source set or equivalent test tree.
- `Current module`: the smallest build unit that contains the target class and runs its tests, typically the nearest parent with `pom.xml`, `build.gradle`, or `build.gradle.kts`.
- `Clear package convention`: at least 2 valid executable tests of the same test type using the same JUnit version inside the current package, without equally strong nearby conflict.
- `Clear module convention`: a visible simple majority of the same test type using the same JUnit version inside the current module, used only when the current package does not provide enough evidence.
- Do not infer conventions from outside the current package or current module unless the user explicitly asks for that.

## Decision Policy (Mandatory)

1. If user does not specify test type, generate **unit tests**.
2. Generate integration tests only when user explicitly asks for integration behavior.
3. Do not infer integration only because the class is a `Controller` or `Repository`.
4. Resolve JUnit version in this order, always for the same test type when possible:
   explicit user request, existing test for the same class, current package convention, current module convention.
5. If no local convention is clear, default to **JUnit 4**.
6. If the user explicitly asks for `JUnit 5` but the current module only shows valid `JUnit 4` tests, warn the user and ask for confirmation before generating code.
7. Do not migrate or rewrite existing tests just to align versions.

## Core Rules

- Target stack: Java 8 with JUnit 4 or JUnit 5.
- Follow repository conventions only when they are locally clear for the same test type.
- Prefer AssertJ style when compatible with repository conventions.
- Use static imports for assertions and Mockito helpers where possible.
- Treat a single isolated JUnit 5 file as weak evidence; prefer the nearest clear convention.
- Use JUnit-appropriate annotations and imports consistently within the chosen version.
- Keep naming explicit and behavior-oriented (`should...When...` or `given...When...Then`).
- Keep exception assertion lambdas limited to the single invocation expected to throw; move setup and preparation outside the lambda.
- Prefer chained or grouped assertions when validating multiple properties of the same subject and that form is clearer.
- Do not introduce `@Ignore`, `Assume`, `@Disabled`, `Assumptions`, or equivalent skip mechanisms just to hide slow tests; fix the test scope or the underlying cause instead.
- Do not add unnecessary tests for trivial getters/setters unless requested.

## JUnit Version Detection

Use executable tests as the strongest evidence when available.

- JUnit 5 signals: `org.junit.jupiter.api.*`, `@BeforeEach`, `@AfterEach`, `@ExtendWith(...)`
- JUnit 4 signals: `org.junit.*`, `@Before`, `@After`, `@RunWith(...)`
- Treat static analysis as secondary evidence if execution is not available.
- A single matching file is not enough to establish package convention by itself.

If tests cannot be executed during analysis, treat static evidence as secondary and avoid using a weak signal to override a clearer local convention.

## Unit Test Guidance

Use for isolated logic, no Spring context.

Recommended annotations and structure:
- JUnit 4: `@RunWith(MockitoJUnitRunner.class)`, `@Mock`, `@InjectMocks`, `@Before`, `@Test`
- JUnit 5: `@ExtendWith(MockitoExtension.class)`, `@Mock`, `@InjectMocks`, `@BeforeEach`, `@Test`
- AAA flow: Arrange, Act, Assert

Unit constraints:
- Do not use `@DataJpaTest`, `@WebMvcTest`, or `@SpringBootTest`.
- Prefer mocking collaborators and asserting behavior/output.
- `MockMvcBuilders.standaloneSetup(...)` is an isolated or unit-style controller test, not a Spring integration test.

## Integration Test Guidance

Use only when explicitly requested by the user.

Choose the narrowest scope:
- Repository integration: `@DataJpaTest`
- Controller integration: `@WebMvcTest`
- Full context: `@SpringBootTest` only when needed

Integration expectations:
- Use real Spring slice/context components as appropriate.
- Verify observable behavior at boundary level (HTTP, JPA interaction, transactional effects).
- `@WebMvcTest` is an integration-style test when the user asked for integration or when that is the established convention for that type in the current module.

## Assertions Policy

- Default preference: AssertJ.
- If project convention or dependencies use another assertion style, follow the repository.
- Prefer chained assertions on the same subject and `extracting(...).containsExactly(...)` patterns when they improve readability.
- Keep exception assertions focused on the throwing call only; avoid mixing setup or multiple actions inside the assertion lambda.

Detailed assertion examples: [`references/assertions.md`](references/assertions.md)

## Templates

Use templates from:
- [`references/templates.md`](references/templates.md)

Template set includes:
- Service unit test in JUnit 4 and JUnit 5.
- Controller unit test in JUnit 4 and JUnit 5.
- Controller standalone `MockMvc` test in JUnit 4 and JUnit 5.
- Repository integration test in JUnit 4 and JUnit 5.
- Controller integration test in JUnit 4 and JUnit 5.

## Final Verification Checklist

- Confirm requested test type (unit vs integration) before generating code.
- Confirm whether the user explicitly requested a JUnit version.
- Confirm whether the target class already has a test and whether it is valid.
- Confirm the nearest package or module convention for the same test type.
- If the user requested JUnit 5 against a JUnit 4-only module, warn and ask for confirmation.
- Confirm Java 8 + the selected JUnit version annotations/imports.
- Confirm no Spring context annotations appear in unit tests.
- Confirm integration tests use the correct Spring slice for the request.
- Confirm assertion style follows repository conventions.
- Confirm exception assertions keep setup outside the lambda and only execute the call expected to throw.
- Confirm related checks on the same subject are grouped when that improves clarity.
- Confirm no skip or assumption mechanism was introduced to hide a slow test.
- Confirm target tests compile and the requested test class/method can be executed.
