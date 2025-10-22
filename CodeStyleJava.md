Java Style Guide — Agent Edition
================================

**Baseline**

*   Follow Google Java Style Guide (except formatting). Use IntelliJ “Default” auto-formatter.

**Language & APIs**

*   Lombok is **required**.
*   `var` is **forbidden**.
*   Prefer simple `for` loops. Do **not** use `collection.forEach(lambda)` unless the lambda is a **method reference**.
*   Keep Streams simple; if readability is in doubt, use a loop.

**Constants**

*   Single-use constants only for replacing magic numbers/strings or to improve clarity (e.g., `SPORT_ID_SOCCER = 8`). Avoid elsewhere, especially in SQL.

**Static Members**

*   Prefer `ClassName.member` over static imports. Exception: allow JUnit `assert*` static imports.

**Time & Durations**

*   Timestamps: store as `long` (milliseconds since epoch, preferred) or `java.util.Date` / `java.time.Instant`.
*   Durations in primitives default to **milliseconds** and must be `long`.
*   If using other units, state the unit in the **name** (e.g., `timeoutSec`) or a **comment** above the declaration.

**Readability**

*   Optimize for junior-level readability. Favor clear, straightforward code over cleverness.

**Testing**

*   Recommended unit test method name patterns:
    *   `shouldExpectedBehavior_whenStateUnderTest`
    *   `givenPreconditions_whenStateUnderTest_thenExpectedBehavior`

**DTOs**

*   DTO fields are `private`; expose via getters.

**Spring Boot**

*   Prefer constructor injection via `@RequiredArgsConstructor`; dependencies `final`. Avoid `@Autowired` unless strongly justified.
*   Application code must not reference profile names. Forbidden: `env.acceptsProfiles("prod")`, `@Profile("prod")`, `System.getProperty("spring.profiles.active")`.
