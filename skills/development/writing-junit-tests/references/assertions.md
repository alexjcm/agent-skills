# Assertions Guide for Java + JUnit 4 or JUnit 5

Use this guide to keep assertions readable, maintainable, and aligned with repository conventions. Assertion style should follow the local module before any default preference in this skill.

## Preferred Pattern: Group Assertions by Subject

Prefer chaining assertions on the same object:

```java
assertThat(order)
    .isNotNull()
    .extracting(Order::getStatus, Order::getCurrency)
    .containsExactly("CREATED", "USD");
```

This usually reads better than multiple disconnected assertions over the same variable.
When several checks describe the same result object, grouping them can also avoid common static-analysis warnings about fragmented assertions.

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
- keep Hamcrest if the module already uses matcher-based assertions
- prefer AssertJ only when it matches existing tests or is clearly supported

## Anti-Patterns to Avoid

- Mixing many assertion libraries in one test class without a reason.
- Over-fragmented assertions for the same subject when a single chain is clearer.
- Putting setup, mock configuration, or multiple actions inside an exception assertion lambda.
- Introducing skip or assumption mechanisms to hide slow tests instead of correcting test scope or test design.
- Asserting implementation details that do not matter for behavior.
