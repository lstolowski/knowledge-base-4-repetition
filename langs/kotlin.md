# Kotlin

### Kotlin cheatsheet 
https://devhints.io/kotlin

### Scope functions 

Purpose is to execute a block of code within the context of an object

docs https://kotlinlang.org/docs/scope-functions.html 
  
YT tutorial https://www.youtube.com/watch?v=Vy-dS2SVoHk 

| Function           | Object reference | Return value                                | Case to use
|--------------------|------------------|---------------------------------------------| ---
| object?.`let`      | it               | Lambda result (last line)                   | null check without if, expression as a variable in local scope
| object?.`also`     | it               | Context object                              | Additional effects
| object?.`apply`    | this             | Context object                              | Object configuration
| object?.`run`      | this             | Lambda result (last line)                   | Object configuration and computing the result
| `run {}`           | -                | Lambda result (last line)                   |
| `with (object) {}` | this             | No: takes the context object as an argument | Grouping function calls on an object

 ### Coroutines

"Lightweight threads", multiple couroutines on one thread

docs https://kotlinlang.org/docs/coroutines-overview.html

YT tutorials https://www.youtube.com/watch?v=ShNhJ3wMpvQ&list=PLQkwcJG4YTCQcFEPuYGuv54nYai_lwil_

https://www.youtube.com/watch?v=6zLyFzHc6xs&t=136s
