## **Code style (Java)**

* We adopt the [Google Java Style Guide](https://google.github.io/styleguide/javaguide.html) as the baseline (except the code-formatting provisions).

* Always format code using an automatic formatter (IDEA Default).

* Using Lombok is mandatory.

* Using `var` is prohibited (it reduces readability—especially during code review—and hampers type navigation, particularly in parameterized classes).

* Do not use `collection.forEach(lambda)` if the lambda is **not** a method reference. A simple `for` loop will take the same number of lines, can modify variables outside the loop body, runs faster, uses fewer resources, and is easier to debug. [ChatGPT’s opinion](https://chat.openai.com/share/85bad93c-8fb2-44da-9e6b-b325ee432a3b)

* Constants used only in a single place in the entire codebase are welcome **only** when replacing magic numbers/strings (example: `SPORT_ID_SOCCER = 8`) or to improve code expressiveness. In other cases, single-use constants are discouraged (especially in SQL queries).

* When calling static methods or using static fields from other classes, prefer using the fully qualified name (e.g., `OtherClass.staticMethod()`) instead of importing that member into your class’s namespace. This lowers the risk of errors when deciding which class to import from again (e.g., when copying code snippets or moving a class to another module). This rule does **not** apply to `assert*` methods from JUnit.

* In Java code, all timestamps must be stored as `long` (milliseconds since 1970—preferred) or as `java.util.Date` or `java.time.Instant`.

* In Java code, all time intervals stored in integer types (`int`, `long`) are assumed by default to be expressed in **milliseconds** and must be stored as `long`. If a duration is expressed in other units, the unit must be explicitly indicated in the variable/field/parameter/method name. If not indicated in the name, it must be stated explicitly in a comment above the declaration. Intervals expressed with `java.time.Duration` do not require special naming, but they use more memory (`long + int` wrapped in an object); they **can and should** be used when switching away from `Duration` would not yield more than \~0.1% memory/CPU savings in production.

* Code readability is paramount. Write code so that reading it (where possible) does not require a qualification higher than junior. Avoid rarely used Stream features—prefer a simple `for` loop over a Stream if there’s any chance readers might doubt what the Stream is doing.

* Recommended unit-test method naming patterns:  
   `shouldExpectedBehavior_whenStateUnderTest`  
   `givenPreconditions_whenStateUnderTest_thenExpectedBehavior`  
   More info: [1](https://www.baeldung.com/java-unit-testing-best-practices#3-test-case-naming-convention), [2](https://dzone.com/articles/7-popular-unit-test-naming)

* DTO class fields must be `private`; use getters to access fields from outside the DTO class.

* Spring Boot best practices

  * Prefer constructor injection via `@RequiredArgsConstructor` (don’t use `@Autowired` without a strong reason); mark bean dependencies as `final`.

  * Application code should have zero knowledge of Spring profile names. Forbidden examples: `env.acceptsProfiles("prod")`, `@Profile("prod")`, `System.getProperty("spring.profiles.active")`.
