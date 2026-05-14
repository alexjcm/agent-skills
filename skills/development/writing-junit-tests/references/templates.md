# Templates for `writing-junit-tests`

Use these templates as starting points and adapt names, imports, and assertion style to the repository conventions. Prefer the version already established for the same class, package, or module. If no clear local convention exists, start from the JUnit 4 template for the requested test type.

## 1) Service Unit Test

### JUnit 4 + Mockito

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
        assertThat(result).isNotNull();
        assertThat(result.getId()).isEqualTo(1L);
    }
}
```

### JUnit 5 + Mockito

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

        assertThat(result).isNotNull();
        assertThat(result.getId()).isEqualTo(1L);
    }
}
```

## 2) Controller Unit Test (No Spring Test Context)

### JUnit 4 + Mockito

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

        assertThat(response.getStatus()).isEqualTo(200);
        assertThat(response.getBody()).isEqualTo(dto);
    }
}
```

### JUnit 5 + Mockito

```java
import static org.assertj.core.api.Assertions.assertThat;
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
        UserDTO dto = new UserDTO("john");
        when(userService.getUser("john")).thenReturn(dto);

        Response response = controller.getUser("john");

        assertThat(response.getStatus()).isEqualTo(200);
        assertThat(response.getBody()).isEqualTo(dto);
    }
}
```

## 3) Controller Isolated Test with `MockMvcBuilders.standaloneSetup(...)`

This is still an isolated or unit-style test because it does not load a Spring test context.

### JUnit 4

```java
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;
import static org.mockito.Mockito.when;

import org.junit.Before;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.junit.MockitoJUnitRunner;
import org.springframework.test.web.servlet.MockMvc;
import org.springframework.test.web.servlet.setup.MockMvcBuilders;

@RunWith(MockitoJUnitRunner.class)
public class UserControllerStandaloneTest {

    private MockMvc mockMvc;

    @Mock
    private UserService userService;

    @InjectMocks
    private UserController controller;

    @Before
    public void setup() {
        mockMvc = MockMvcBuilders.standaloneSetup(controller).build();
    }

    @Test
    public void shouldReturnOkWhenUserExists() throws Exception {
        when(userService.exists("john")).thenReturn(true);

        mockMvc.perform(get("/users/john"))
            .andExpect(status().isOk());
    }
}
```

### JUnit 5

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

## 4) Repository Integration Test (JPA Slice)

This is integration testing, not unit testing.

### JUnit 4

```java
import static org.assertj.core.api.Assertions.assertThat;

import org.junit.Before;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.orm.jpa.DataJpaTest;
import org.springframework.boot.test.autoconfigure.orm.jpa.TestEntityManager;
import org.springframework.test.context.junit4.SpringRunner;

@RunWith(SpringRunner.class)
@DataJpaTest
public class ReturnProviderRepositoryTest {

    @Autowired
    private TestEntityManager entityManager;

    @Autowired
    private ReturnProviderRepository repository;

    @Before
    public void setup() {
        entityManager.persist(new ReturnProviderEntity(100L, 1, 5001L, "OLD", "USR"));
    }

    @Test
    public void shouldUpdateSapReturnNumberWhenCompanyAndProcessMatch() {
        // Act
        long updatedRows = repository.updateSapReturnNumber(1, 5001L, "4500001234", "FRM0");
        entityManager.flush();
        entityManager.clear();

        // Assert
        ReturnProviderEntity entity = entityManager.find(ReturnProviderEntity.class, 100L);
        assertThat(updatedRows).isEqualTo(1L);
        assertThat(entity)
            .extracting(ReturnProviderEntity::getSapReturnNumber, ReturnProviderEntity::getModifierUser)
            .containsExactly("4500001234", "FRM0");
    }
}
```

### JUnit 5

```java
import static org.assertj.core.api.Assertions.assertThat;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.orm.jpa.DataJpaTest;
import org.springframework.boot.test.autoconfigure.orm.jpa.TestEntityManager;

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
        assertThat(updatedRows).isEqualTo(1L);
        assertThat(entity)
            .extracting(ReturnProviderEntity::getSapReturnNumber, ReturnProviderEntity::getModifierUser)
            .containsExactly("4500001234", "FRM0");
    }
}
```

## 5) Controller Integration Test (Web MVC Slice)

### JUnit 4

```java
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.jsonPath;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
import org.springframework.boot.test.mock.mockito.MockBean;
import org.springframework.test.context.junit4.SpringRunner;
import org.springframework.test.web.servlet.MockMvc;

@RunWith(SpringRunner.class)
@WebMvcTest(UserController.class)
public class UserControllerWebMvcTest {

    @Autowired
    private MockMvc mockMvc;

    @MockBean
    private UserService userService;

    @Test
    public void shouldReturnUserWhenRequestIsValid() throws Exception {
        mockMvc.perform(get("/users/john"))
            .andExpect(status().isOk())
            .andExpect(jsonPath("$.username").value("john"));
    }
}
```

### JUnit 5

```java
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.jsonPath;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
import org.springframework.boot.test.mock.mockito.MockBean;
import org.springframework.test.web.servlet.MockMvc;

@WebMvcTest(UserController.class)
class UserControllerWebMvcTest {

    @Autowired
    private MockMvc mockMvc;

    @MockBean
    private UserService userService;

    @Test
    void shouldReturnUserWhenRequestIsValid() throws Exception {
        mockMvc.perform(get("/users/john"))
            .andExpect(status().isOk())
            .andExpect(jsonPath("$.username").value("john"));
    }
}
```
