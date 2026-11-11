# Templates for `writing-junit-tests`

Use these templates as starting points after you have already decided the repository profile and test type. Always adapt names, imports, setup, and assertion style to the repository conventions.

Selection order:
- `legacy` profile: Java 8 modules with JUnit 4 or older Spring conventions
- `modern` profile: Java 25 + Spring Boot 4 style modules with JUnit Jupiter and test slices
- repository-specific template: when the module already uses a custom controller base or test bootstrap

## Table of Contents

1. [Legacy Service Unit Test](#1-legacy-service-unit-test) — Java 8 + JUnit 4
2. [Legacy or Modern Service Unit Test with Jupiter](#2-legacy-or-modern-service-unit-test-with-jupiter) — JUnit 5
3. [Legacy Controller Unit Test Without Spring Context](#3-legacy-controller-unit-test-without-spring-context) — plain object
4. [Modern Controller Unit Test Without Spring Context](#4-modern-controller-unit-test-without-spring-context) — JUnit 5 plain object
5. [Controller Unit Test with Isolated MockMvc](#5-controller-unit-test-with-isolated-mockmvc) — standaloneSetup
6. [Modern Repository Integration with Spring Boot 4 JPA Slice](#6-modern-repository-integration-with-spring-boot-4-jpa-slice) — @DataJpaTest
7. [Modern Controller MVC Slice with @WebMvcTest](#7-modern-controller-mvc-slice-with-webmvctest) — MVC slice
8. [Repository-Specific Controller Test Base](#8-repository-specific-controller-test-base) — custom base class
9. [Full Context Smoke Test](#9-full-context-smoke-test) — @SpringBootTest

## 1) Legacy Service Unit Test

Use when the module is Java 8 and nearby tests clearly use JUnit 4.

```java
import static org.assertj.core.api.Assertions.assertThat;
import static org.mockito.Mockito.when;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.junit.MockitoJUnitRunner;

@RunWith(MockitoJUnitRunner.class)
public class OrderServiceTest {

    @Mock
    private OrderRepository orderRepository;

    @InjectMocks
    private OrderService service;

    @Test
    public void shouldReturnOrderWhenIdExists() {
        // Arrange
        Order expected = new Order(1L);
        when(orderRepository.findById(1L)).thenReturn(expected);

        // Act
        Order result = service.findById(1L);

        // Assert
        assertThat(result)
            .isNotNull()
            .extracting(Order::getId)
            .isEqualTo(1L);
    }
}
```

Pass exact arguments directly. Do not write `when(orderRepository.findById(eq(1L))).thenReturn(expected);` unless the call is already using other Mockito matchers and consistency requires matcher form.

## 2) Legacy or Modern Service Unit Test with Jupiter

Use when nearby service tests already use JUnit 5 or the module is clearly modern.

```java
import static org.assertj.core.api.Assertions.assertThat;
import static org.mockito.Mockito.when;

import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.junit.jupiter.MockitoExtension;

@ExtendWith(MockitoExtension.class)
class OrderServiceTest {

    @Mock
    private OrderRepository orderRepository;

    @InjectMocks
    private OrderService service;

    @Test
    void shouldReturnOrderWhenIdExists() {
        Order expected = new Order(1L);
        when(orderRepository.findById(1L)).thenReturn(expected);

        Order result = service.findById(1L);

        assertThat(result)
            .isNotNull()
            .extracting(Order::getId)
            .isEqualTo(1L);
    }
}
```

Prefer direct argument values in stubbing and verification when the inputs are exact and already known.

## 3) Legacy Controller Unit Test Without Spring Context

Use when the controller can be exercised directly as a plain object and no HTTP layer is needed.

```java
import static org.assertj.core.api.Assertions.assertThat;
import static org.mockito.Mockito.when;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.junit.MockitoJUnitRunner;

@RunWith(MockitoJUnitRunner.class)
public class UserControllerTest {

    @Mock
    private UserService userService;

    @InjectMocks
    private UserController controller;

    @Test
    public void shouldReturnOkResponseWhenUserExists() {
        UserDTO dto = new UserDTO("john");
        when(userService.getUser("john")).thenReturn(dto);

        Response response = controller.getUser("john");

        assertThat(response)
            .extracting(Response::getStatus, Response::getBody)
            .containsExactly(200, dto);
    }
}
```

## 4) Modern Controller Unit Test Without Spring Context

Use when the module is modern but the controller can still be tested as a plain object, without HTTP infrastructure.

```java
import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.mockito.Mockito.when;

import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.junit.jupiter.MockitoExtension;

@ExtendWith(MockitoExtension.class)
class UserControllerTest {

    @Mock
    private UserService userService;

    @InjectMocks
    private UserController controller;

    @Test
    void shouldReturnOkResponseWhenUserExists() {
        UserDto dto = new UserDto("john");
        when(userService.getUser("john")).thenReturn(dto);

        Response response = controller.getUser("john");

        assertEquals(200, response.getStatus());
        assertEquals(dto, response.getBody());
    }
}
```

## 5) Controller Unit Test with Isolated MockMvc

```java
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;
import static org.mockito.Mockito.when;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.junit.jupiter.MockitoExtension;
import org.springframework.test.web.servlet.MockMvc;
import org.springframework.test.web.servlet.setup.MockMvcBuilders;

@ExtendWith(MockitoExtension.class)
class UserControllerStandaloneTest {

    private MockMvc mockMvc;

    @Mock
    private UserService userService;

    @InjectMocks
    private UserController controller;

    @BeforeEach
    void setup() {
        mockMvc = MockMvcBuilders.standaloneSetup(controller).build();
    }

    @Test
    void shouldReturnOkWhenUserExists() throws Exception {
        when(userService.exists("john")).thenReturn(true);

        mockMvc.perform(get("/users/john"))
            .andExpect(status().isOk());
    }
}
```

This is still unit-style because it does not load a Spring context.

## 6) Modern Repository Integration with Spring Boot 4 JPA Slice

Use when the module is modern and repository integration tests already use JPA slices.

```java
import static org.junit.jupiter.api.Assertions.assertEquals;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.data.jpa.test.autoconfigure.DataJpaTest;
import org.springframework.boot.jpa.test.autoconfigure.TestEntityManager;

@DataJpaTest
class ReturnProviderRepositoryTest {

    @Autowired
    private TestEntityManager entityManager;

    @Autowired
    private ReturnProviderRepository repository;

    @BeforeEach
    void setup() {
        entityManager.persist(new ReturnProviderEntity(100L, 1, 5001L, "OLD", "USR"));
    }

    @Test
    void shouldUpdateSapReturnNumberWhenCompanyAndProcessMatch() {
        long updatedRows = repository.updateSapReturnNumber(1, 5001L, "4500001234", "FRM0");
        entityManager.flush();
        entityManager.clear();

        ReturnProviderEntity entity = entityManager.find(ReturnProviderEntity.class, 100L);
        assertEquals(1L, updatedRows);
        assertEquals("4500001234", entity.getSapReturnNumber());
        assertEquals("FRM0", entity.getModifierUser());
    }
}
```

Keep the exact import package that the current module already uses. Spring Boot 4 repositories may not match older examples.
If a Mockito stub for this test uses exact values, prefer `when(repository.call(value1, value2))` over `when(repository.call(eq(value1), eq(value2)))`.

## 7) Modern Controller MVC Slice with `@WebMvcTest`

Use when the repository does focused MVC integration through Spring's web test slice.

```java
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.jsonPath;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.webmvc.test.autoconfigure.WebMvcTest;
import org.springframework.test.context.bean.override.mockito.MockitoBean;
import org.springframework.test.web.servlet.MockMvc;

@WebMvcTest(UserController.class)
class UserControllerWebMvcTest {

    @Autowired
    private MockMvc mockMvc;

    @MockitoBean
    private UserService userService;

    @Test
    void shouldReturnUserWhenRequestIsValid() throws Exception {
        mockMvc.perform(get("/users/john"))
            .andExpect(status().isOk())
            .andExpect(jsonPath("$.username").value("john"));
    }
}
```

Use the local import package if the module shows a different Boot generation. Keep `@MockBean` only when the repository is on an older established Spring Boot generation and nearby tests already use it.

## 8) Repository-Specific Controller Test Base

Use when the module already has a stable custom test base for controllers, like the Java 25 sample project.

```java
import static org.mockito.Mockito.times;
import static org.mockito.Mockito.verify;
import static org.mockito.Mockito.when;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.post;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

import ec.com.kruger.spring.ws.v3.controller.test.MockMvcControllerBaseTest;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.junit.jupiter.MockitoExtension;
import org.springframework.http.MediaType;
import org.springframework.restdocs.RestDocumentationContextProvider;
import org.springframework.test.context.junit.jupiter.SpringExtension;
import org.springframework.test.web.servlet.result.MockMvcResultHandlers;

@ExtendWith({MockitoExtension.class, SpringExtension.class})
class UserControllerTest extends MockMvcControllerBaseTest {

    @InjectMocks
    private UserController controller;

    @Mock
    private UserService userService;

    @BeforeEach
    void setup(RestDocumentationContextProvider provider) throws Exception {
        super.setUpBase(controller, provider);
    }

    @Test
    void shouldCreateUser() throws Exception {
        UserRequest request = new UserRequest("john");
        when(userService.exists("john")).thenReturn(true);

        mockMvc.perform(post("/users")
                .contentType(MediaType.APPLICATION_JSON)
                .content(objectMapper.writeValueAsString(request)))
            .andExpect(status().isOk())
            .andDo(MockMvcResultHandlers.print());

        verify(userService, times(1)).exists("john");
    }
}
```

Follow the custom base when it is the dominant repository pattern. Do not replace it with `@WebMvcTest` just because that is the generic Spring recommendation.

## 9) Full Context Smoke Test

Use `@SpringBootTest` only when the user asks for full-context behavior or when existing tests for that module already use it.

```java
import org.junit.jupiter.api.Test;
import org.springframework.boot.test.context.SpringBootTest;

@SpringBootTest
class ApplicationContextTest {

    @Test
    void contextLoads() {
    }
}
```
