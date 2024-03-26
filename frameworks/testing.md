## Testing

### Unit testing Java
- JUnit5 https://www.baeldung.com/junit-5
- Mockito 
  - in nutshell https://softwareskill.pl/mockito-w-pigulce 
  - quick guide https://www.baeldung.com/mockito-annotations
  - matchers https://www.baeldung.com/mockito-argument-matchers
- assertions
  - Junit asserts https://www.baeldung.com/junit-assertions
  - assertJ, Hamcrest
- Wiremock
- Testcontainers
- https://dzone.com/articles/7-awesome-libraries-for-java-unit-amp-integration

### Unit testing Kotlin
- mockk https://blog.kotlin-academy.com/mocking-is-not-rocket-science-basics-ae55d0aadf2b
- kotest https://www.baeldung.com/kotlin/kotest


### Integration testing examples

#### @SpringBootTest and MockMvc

pom.xml
```xml
    <dependency>
      <groupId>org.junit.jupiter</groupId>
      <artifactId>junit-jupiter-engine</artifactId>
      <version>5.8.2</version>
      <scope>test</scope>
    </dependency>
    <dependency>
      <groupId>org.junit.jupiter</groupId>
      <artifactId>junit-jupiter-api</artifactId>
      <version>5.8.2</version>
      <scope>test</scope>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-test</artifactId>
      <version>6.0.13</version>
      <scope>test</scope>
    </dependency>
```

Test

```kotlin
@SpringBootTest
@AutoConfigureMockMvc(addFilters = false)
@ContextConfiguration(initializers = [SsoClientSharedCookieLibInitializer::class])
@ActiveProfiles("test")
class HomeownerFlowControllerIntegrationTest(
  @Autowired private val mockMvc: MockMvc,
) {
    // use  @MockBean to mock dependencies
    
  @Test // in case of async calls
  fun `test getObject`() = runTest { // -> allow to invoke suspende functions (i.e. when mock) 
    val mvcResult = mockMvc.get("/my/endpoint") {
          param("myParam", "xx")
      }
        .andExpect { request { asyncStarted() } } // <-- asyncStarted
        .andDo { MockMvcResultHandlers.log() }
        .andReturn()

    mockMvc.perform(MockMvcRequestBuilders.asyncDispatch(mvcResult))
      .andExpect(status().isOk())
      .andExpect(content().contentTypeCompatibleWith(MediaType.APPLICATION_JSON))
      .andExpect(jsonPath("$.buildingId").value(buildingId))
  } 
  @Test // in case of sync calls
  fun `test getObject`() = runTest { // -> allow to invoke suspende functions (i.e. when mock)
      mockMvc.get("/my/endpoint") {
          param("myParam", "xx")
      }
          .andExpect {
              status { isOk() }
              jsonPath("$.id") { value(1) }
          }
  }
}
```






### API Contract tests 
- contract testing concept: https://ichi.pro/pl/podstawy-testow-kontraktowych-160597205567520
- spring https://pater.dev/spring-cloud-contract-testy-kontraktowe/
- pact https://pkubowicz.pl/testy-kontraktowe-pact-bez-pact-brokera/

- Consumer-Driven Contract

## Other tests

- mutating
- 


## Optimizing tests
- https://softwareskill.pl/przyspieszenie-testow-spring
