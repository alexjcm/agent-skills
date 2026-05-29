# Assertions Guide for Java + JUnit 4 or JUnit 5

Use this guide to keep assertions readable, maintainable, and aligned with repository conventions. Assertion style should follow the local module before any default preference in this skill.

## Selection Order

Choose assertion style in this order:

1. Existing test for the same class
2. Nearby package convention
3. Module convention
4. Only if there is no clear convention, choose the clearest style already supported by the module

Do not introduce AssertJ just because it is available if the module consistently uses `Assertions` or JUnit assertions.

If the chosen library is JUnit Jupiter assertions, prefer static imports unless nearby tests consistently use qualified `Assertions.*` calls.

## Group Assertions by Subject

Prefer chaining or grouping assertions on the same object when the local assertion library supports it:

```java
assertThat(order)
    .isNotNull()
    .extracting(Order::getStatus, Order::getCurrency)
    .containsExactly("CREATED", "USD");
```

This usually reads better than multiple disconnected assertions over the same variable.

If the repository mostly uses JUnit assertions, grouping can still be done by keeping related checks adjacent:

```java
import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.junit.jupiter.api.Assertions.assertNotNull;

assertNotNull(order);
assertEquals("CREATED", order.getStatus());
assertEquals("USD", order.getCurrency());
```

## Readability-Friendly Patterns

### 1) Keep related checks together

```java
assertThat(result)
    .isNotNull()
    .extracting(Result::getCode, Result::getMessage)
    .containsExactly("OK", "done");
```

Prefer this over splitting the same subject into many disconnected assertions when a single grouped assertion is clearer.

### 2) Prefer semantic collection assertions

```java
assertThat(items)
    .hasSize(2)
    .containsExactly(itemA, itemB);
```

### 3) Exception assertions

Keep the lambda focused on the single call expected to throw. Prepare inputs, mocks, and fixtures before the assertion.

```java
assertThatThrownBy(() -> service.process(null))
    .isInstanceOf(IllegalArgumentException.class)
    .hasMessageContaining("input");
```

Avoid patterns like these:

```java
assertThatThrownBy(() -> {
    Request request = new Request();
    request.setType("INVALID");
    service.prepare(request);
    service.process(request);
})
    .isInstanceOf(IllegalArgumentException.class);
```

The setup obscures which call is actually expected to fail. Keep preparation outside the assertion and leave only the throwing invocation inside the lambda.

## When Repository Convention Differs

If the project already standardizes on another style:
- keep consistency with existing tests
- avoid introducing mixed assertion styles in the same test class
- prioritize repository conventions over this guide

Common examples:
- keep JUnit assertions if the module consistently uses `org.junit.Assert`
- prefer static Jupiter imports such as `assertEquals(...)` when the module uses JUnit 5 assertions but has no strong qualified-call convention
- keep `org.junit.jupiter.api.Assertions` qualified calls if the module consistently uses `Assertions.*`
- keep Hamcrest if the module already uses matcher-based assertions
- prefer AssertJ only when it matches existing tests or is clearly supported

## Anti-Patterns to Avoid

- Mixing many assertion libraries in one test class without a reason.
- Over-fragmented assertions for the same subject when a single chain is clearer.
- Putting setup, mock configuration, or multiple actions inside an exception assertion lambda.
- Introducing skip or assumption mechanisms to hide slow tests instead of correcting test scope or test design.
- Asserting implementation details that do not matter for behavior.

## Related Mockito Style

Even though this file is about assertions, the same readability rule applies to Mockito stubbing and verification:

```java
when(repository.findByCompanyAndStatus(companyId, "ACTIVE")).thenReturn(records);
verify(repository).findByCompanyAndStatus(companyId, "ACTIVE");
```

Prefer that form over:

```java
when(repository.findByCompanyAndStatus(eq(companyId), eq("ACTIVE"))).thenReturn(records);
verify(repository).findByCompanyAndStatus(eq(companyId), eq("ACTIVE"));
```

Use `eq(...)` only when you are already using matchers in the same invocation or when matcher semantics are genuinely needed.
