# Java

### Basics
- Java 8 / Java 11 / Java 19 - whats new
  - 8-11 https://nullpointerexception.pl/zmiany-w-javie-od-wersji-8-do-java-11/
  - 11-17 https://nullpointerexception.pl/zmiany-w-javie-od-wersji-11-do-java-17/
- hashCode + equals contract https://www.baeldung.com/java-equals-hashcode-contracts
- composition vs inheritance, polimorphism
  - composition is better - testing is easier
- Collections 
  - Set vs List, ArrayList, LinkedList (diffs and how work), how to search
  - Maps (hashcode, equals in search) 
  - stream api (map, filter, collect, flatMap etc)
  - map vs flatMap https://www.baeldung.com/java-difference-map-and-flatmap
  - złożoność obliczeniowa algorytmów w kolekcjach
  
- Exceptions
  - hierarchy, checked/unchecked, handling https://rollbar.com/blog/java-exceptions-hierarchy-explained/# 
  - try-with-resources https://www.baeldung.com/java-try-with-resources

- Garbage collector
  - co to jest i po co (usuwanie obiektów z pamięci które stracili referencje w kodzie)
  - przejrzeć i wiedzieć ze sa tam w GC swego rodzaju stage’e od najmłodszych do najstarszych obiektów
  - 2-3 słowa o metodach optymalizacji


### Functional interface
- https://www.baeldung.com/java-8-functional-interfaces
- https://www.geeksforgeeks.org/functional-interfaces-java/

### Concurency in Java
- overview https://www.baeldung.com/java-concurrency
- [Threads in Java](./java-threads.md)
- Concurrent API https://www.baeldung.com/java-util-concurren
  - concurrent collections