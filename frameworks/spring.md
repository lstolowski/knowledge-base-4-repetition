### Spring
- definicje beana (rodzaje: Service, Component, Controller, Repository)
- jak to działa ze bean jest tworzony (dawniej obiekty proxy, teraz aspekty AOP)
- Autowired, Qualifier, jak jest wybierany podczas wstrzykiwania
- wstrzykiwanie przez konstruktor czy pole, czemu tak i co lepsze (lepszy konstruktor, bo latwiejsze testowanie, czytelne api klasy)
- internals (rożne dziwne pytania o działanie kontekstu springa)

#### RestClient

#### WebClient in Spring 5
- https://www.baeldung.com/spring-5-webclient


#### XSRF/CSRF
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
