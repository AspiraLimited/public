Java Style Guide — Agent Edition
================================

**Getting updates**

```bash
curl https://raw.githubusercontent.com/AspiraLimited/public/refs/heads/master/CodeStyleJava.md > CODESTYLE.md
```


**Baseline**

*   Follow Google Java Style Guide (except formatting). Use IntelliJ “Default” auto-formatter.

**Architecture Values**

*   **DRY**, **KISS**, **YAGNI**.
*   Fail fast.
*   Avoid overly defensive code. Don't validate config at runtime — validate it at generation.

**Language & APIs**

*   Lombok is **required**.
*   In `@UtilityClass` classes, all members except the constructor must be explicitly marked `static`.
*   `var` is **forbidden**.
*   Prefer simple `for` loops. Do **not** use `collection.forEach(lambda)` unless the lambda is a **method reference**.
*   Keep Streams simple; if readability is in doubt, use a loop.
*   Avoid defensive copies (`List.copyOf` / `Set.copyOf` / `Map.copyOf` / `new ArrayList<>(other)`) unless the source collection is actually mutated after being handed over (e.g. a caller-provided mutable list stored in a field).
*   FQCNs are forbidden unless colliding with other class name.
*   Use the simplest correct concurrency primitive: no real concurrency → `volatile`, not `Atomic*`; use `AtomicReference` only if `compareAndSet` is used.

**Nullability & Optional**

*   Nullability contracts use JSpecify - `org.jspecify.annotations.Nullable` and `org.jspecify.annotations.NonNull`. `@NullMarked` / `@NullUnmarked` are allowed only on methods and records; forbidden on classes and in package-info.java. Usage of alternative nullability contract annotations (such as `org.springframework.lang.Nullable`, `javax.annotation.Nullable`, `javax.annotation.Nonnull`, `org.jetbrains.annotations.*`, `edu.umd.cs.findbugs.annotations.*`) is **forbidden**.
*   Use wrapper types (`Integer`/`Long`/`Boolean`/…) only when the value is nullable (then it must be marked `@Nullable`), when boxing is required by generics (`List<@NonNull Integer>`, or `List<Integer>` inside a `@NullMarked` scope), or when an inherited or externally defined API signature requires a wrapper type (e.g. overriding `@NonNull Integer getCode()`).
*   Outside a `@NullMarked` scope, mark every non-null reference in a signature with `@NonNull`, including type arguments (e.g. `List<@NonNull String>`).
*   Use `@NullMarked` only when it replaces at least 3 `@NonNull` annotations in the signature.
*   Use `@lombok.NonNull` only for runtime arguments null checks when they may realistically fail and improve stack trace readability. Do not use such checks universally.
*   Never use `Optional` in fields, method parameters, or to wrap collections. Exceptions: declaring a return type from standard JDK APIs, terminal Streams, or Spring Data repositories.
*   Unwrap immediately at the boundary via `.orElse(null)` or `.orElseThrow()`. Do not construct `Optional` instances to chain methods; favor simple imperative null checks (`if (x != null)`).
*   Add `org.jspecify.annotations.Nullable` and `org.jspecify.annotations.NonNull` to `lombok.copyableAnnotations` in `lombok.config`.

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

*   TDD: for new features and bug fixes, write the test and run it to confirm it fails before implementing. For a bug fix, the test must reproduce the bug and fail because of it. Exceptions: pure UI changes (templates, CSS, JS), config-only changes, dependency bumps, and cases where an automated test is not feasible.
*   Prefer using real objects, use Mockito or Wiremock for mocking external systems only.
*   Avoid complex and fragile mocks; convert such tests to integration tests if that reduces test size.
*   Don't write tests that just "cement config".
*   Recommended unit test method name patterns:
    *   `shouldExpectedBehavior_whenStateUnderTest`
    *   `givenPreconditions_whenStateUnderTest_thenExpectedBehavior`

**DTOs**

*   DTO fields are `private`; expose via getters.

**Spring Boot**

*   Prefer constructor injection via `@RequiredArgsConstructor`; dependencies `final`. Avoid `@Autowired` unless strongly justified.
*   Application code must not reference profile names. Forbidden: `env.acceptsProfiles("prod")`, `@Profile("prod")`, `System.getProperty("spring.profiles.active")`.

**Configuration**

The principle of configuration responsibility is as follows:
* `configService.getConfig()` — configuration for the service’s business logic
* `/config/application.yml` (auto-deployed), `/config/application-{PROFILE}.yml` (local dev configs, excluded from git), `/config/application-{PROFILE}-example.yml` (example local dev configs),  — settings that depend on the environment where the service runs
* `/resources/application.yml` — configuration of Spring components that does not depend on the environment

Default values in Spring yml/properties and in business logic configs are forbidden.
  
**Maven**

*  Do not move single-use versions into properties unless that improves clarity.
*  Keep versions close to the consuming module when the dependency is truly local.
  
