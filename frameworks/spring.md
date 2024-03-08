## Spring
- definicje beana (rodzaje: Service, Component, Controller, Repository)
- jak to działa ze bean jest tworzony (dawniej obiekty proxy, teraz aspekty AOP)
- Autowired, Qualifier, jak jest wybierany podczas wstrzykiwania
- wstrzykiwanie przez konstruktor czy pole, czemu tak i co lepsze (lepszy konstruktor, bo latwiejsze testowanie, czytelne api klasy)
- internals (rożne dziwne pytania o działanie kontekstu springa)
- global exception handling: @ControllerAdvice over class and @ExceptionHandler over method handling the exception

### RestClient

### WebClient in Spring 5
- https://www.baeldung.com/spring-5-webclient

### WebTestClient in Spring integration tests

Example of the integration tests setup:

```kotlin
import com.ninjasquad.springmockk.MockkBean
import com.ninjasquad.springmockk.SpykBean
import io.mockk.called
import io.mockk.coEvery
import io.mockk.coVerify
import org.assertj.core.api.Assertions.assertThat
import org.springframework.test.context.junit.jupiter.SpringExtension
import org.springframework.test.web.reactive.server.WebTestClient
import org.junit.jupiter.api.Test
import org.junit.jupiter.api.extension.ExtendWith

@ExtendWith(SpringExtension::class)
@WebFluxTest(MyExampleController::class)
class InvoicesApiTest {

    /**
     * In the example MyExampleController contains only the handler as the dependency.
     * Handler contains a service that does a job
     */
    
    @Autowired
    private lateinit var client: WebTestClient

    @MockkBean
    private lateinit var service: MyExampleService

    @SpykBean
    private lateinit var handler: MyExampleHandler

    @Test
    fun `should do something`() {
        coEvery { service.makesAJob(any()) } returns TestObject()

        val response = client.post()
            .uri("/v1/endpoint/examplePost")
            .bodyValue(TestPayload())
            .exchange()
            .expectStatus().isOk
            .expectBody<TestObject>().returnResult().responseBody

        coVerify(exactly = 1) { service.makesAJob(testPauload()) }
        assertThat(response).isEqualTo(TestObject())
    }
}
```


### XSRF/CSRF
[Cross site request forgery](https://pl.wikipedia.org/wiki/Cross-site_request_forgery):
- backend:
SecurityFilterChain filterChain: 
       http
          .csrf()
          .csrfTokenRepository(CookieCsrfTokenRepository.withHttpOnlyFalse()); 

Ustawia cookie XSRF-TOKEN z hashem/tokenem

- frontend
POST/PUT/DELETE (zmieniające stan) powinny wysyłac header X-XSRF-TOKEN z wratościa z cookie
const csrfToken = document.cookie.replace(/(?:(?:^|.*;\s*)XSRF-TOKEN\s*\=\s*([^;]*).*$)|^.*$/, '$1');

fetch(url, {
  method: 'POST',
  body: /* data to send */,
  headers: { 'X-XSRF-TOKEN': csrfToken },
})


https://www.baeldung.com/spring-security-csrf#stateless-spring-api


### Swagger in spring

To add swagger UI docs to your spring-boot project: 

gradle:
```groovy
ext {
  springdocVersion = "1.5.3"
}

# Option A) for Web config
implementation("org.springframework.boot:spring-boot-starter-web")
implementation("org.springdoc:springdoc-openapi-ui:$springdocVersion")

# Option B) for Webflux config
implementation("org.springframework.boot:spring-boot-starter-webflux")
implementation("org.springdoc:springdoc-openapi-webflux-ui:$springdocVersion")

# Needed for Kotlin projects and its models
implementation("org.springdoc:springdoc-openapi-kotlin:$springdocVersion")
```

bean
```kotlin
@Configuration
class OpenApiConfig {
  @Bean
  fun openAPI(): OpenAPI = OpenAPI().addServersItem(Server().url("/")).info(Info().title("My Awesome Service"))
}
```

example of application.yml which makes swagger ui more tidy and readable
```yaml
springdoc:
  packages-to-exclude: springfox.documentation
  swagger-ui:
    path: swagger-ui.html                   # available on https://app.../swagger-ui.html  
    disable-swagger-default-url: true       # disable petstore url
    operations-sorter: alpha                # sort controllers alphabetically
    tags-sorter: alpha                      # sort methods/endpoints alphabetically
  api-docs:
    path: /v3/api-docs
    resolve-schema-properties: true
  show-actuator: false
  writer-with-default-pretty-printer: true
```