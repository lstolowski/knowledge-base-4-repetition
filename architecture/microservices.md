# Microservices

- Patterns in micoservices
  - https://towardsdatascience.com/microservice-architecture-and-its-10-most-important-design-patterns-824952d7fa41
  - Saga https://docs.aws.amazon.com/prescriptive-guidance/latest/modernization-data-persistence/saga-pattern.html
  - CQRS i Event Sourcing [see software architecture](./software-patterns.md)
  - More patterns https://microservices.io/patterns/microservices.html
  - circuit breaker https://softwareskill.pl/circuit-breaker-pattern
  - retry pattern https://softwareskill.pl/retry-pattern

### Communication in microservices

- Kafka
  - intro https://bulldogjob.pl/readme/apache-kafka-opis-dzialania-i-zastosowania
  - spring kafka https://www.baeldung.com/spring-kafka 
  - partitions https://softwareskill.pl/apache-kafka-ile-partycji
- AWS SQS/SNQ , Kinesis

- REST
  - metody, różnice, co można wysyłać (json, headers, request params)
- Rest vs async (queue/topics) https://softwareskill.pl/integracja-rest-czy-system-kolejkowy  
- Authentication vs authorization
    - różnice
    - jakieś przykłady (jwt, serwery autentykami, OAUth2)
    - spring security - zgrubsza [see spring](../frameworks/spring.md)

- Websocket
  - connection client-server, more efficient than Polling or Long-Polling, client and server listen each other 
  - WS w javascript: https://bulldogjob.pl/readme/prosto-o-websocket 
  - WS in Java (via STOMP protocol) https://spring.io/guides/gs/messaging-stomp-websocket/

### Frameworks
- Spring boot (obvious)
- Micronaut https://micronaut.io/
  - lekki, szybko startuje, zajmuje mniej pamięci
  - podobna koncepcja comtrollerów co spring