# SpendLane: Should You Switch from jOOQ to SQLDelight?

> **Analysis date:** March 22, 2026
> Project-specific assessment based on the SpendLane codebase at `/Users/martin.kotevski/workspace/spendlane`.
> For the general library health comparison, see [`jooq-vs-sqldelight-analysis.md`](./jooq-vs-sqldelight-analysis.md).

---

## Project Profile

- **Stack**: Kotlin + Ktor (server-side) + PostgreSQL + Flyway + jOOQ, with a Flutter mobile frontend
- **Architecture**: Clean domain-driven structure with `domain/{feature}/data/repository/` and `domain/{feature}/data/query/` layers
- **Database**: PostgreSQL with 17 tables, using `TEXT[]` arrays, `ON CONFLICT` upserts, complex multi-join queries with subqueries (`NOT EXISTS`), and transaction support
- **Scale**: ~80 source files, single-developer project, early-stage

---

## How jOOQ is Currently Used

1. **Code generation** from a live Postgres DB via Flyway migrations (Gradle plugin)
2. **Type-safe DSL queries** — JOINs, subqueries, `NOT EXISTS`, `ON CONFLICT ... DO UPDATE`, `COALESCE`, `fetchInto()` for DTO mapping
3. **Repo pattern** — thin repos wrapping jOOQ's `DSLContext` with a shared `Repo<T>` interface
4. **Transactions** — `database.transactionResult { config -> block(config.dsl()) }`
5. **PostgreSQL-specific features** — `TEXT[]` arrays (`BIGINT[]` for conflict tracking), `excluded.*` in upsert clauses

---

## Why SQLDelight is a Poor Fit for This Project

| Aspect | Analysis |
|---|---|
| **SQLDelight is SQL-first** | You write `.sq` files with raw SQL and it generates Kotlin type-safe accessors. But your queries are **dynamically constructed** in Kotlin using jOOQ's DSL — `ApiQueries.getUserBudgets()` builds queries conditionally, passes runtime lists into `.in()` clauses, etc. SQLDelight requires all queries defined statically at compile time. |
| **PostgreSQL support** | SQLDelight's PostgreSQL dialect support is **second-class**. It was designed for SQLite (mobile), and the Postgres/MySQL drivers are newer and less mature. Your schema uses `TEXT[]`, `BIGINT[]`, `ON CONFLICT ... DO UPDATE SET ... = excluded.*`, `COALESCE` — these PostgreSQL-specific features have limited or no support in SQLDelight. |
| **Server-side JVM** | SQLDelight shines in **Kotlin Multiplatform / mobile** (Android, iOS). For server-side Kotlin + PostgreSQL, it's not the intended use case. jOOQ was literally designed for this. |
| **Code generation model** | jOOQ generates from your database schema (Flyway → DB → jOOQ codegen). SQLDelight generates from `.sq` files. You'd need to **rewrite every query as static SQL** and lose the ability to compose queries programmatically. |
| **Complex query composition** | Your `ApiQueries.getUserBudgets()` is ~80 lines of programmatic query composition — fetching budgets, then items, then linked payments, then unmatched payments with LEFT JOINs and subqueries, then resolving conflict templates. This kind of multi-step, data-driven query construction is jOOQ's sweet spot and a pain point for SQLDelight. |
| **Upsert patterns** | `BankTransactionRepo.bulkUpsertCompleted()` uses `onConflict().doUpdate().set(DSL.field("excluded.xxx"))` — this is jOOQ-native. SQLDelight's upsert support is basic and Postgres `excluded` reference support is limited. |

---

## Pros & Cons of Switching

### Pros of switching to SQLDelight
- Apache-2.0 license (no dual-license concern)
- Better bus factor / contributor health (as analyzed in the [library comparison](./jooq-vs-sqldelight-analysis.md))
- If you ever wanted to share query logic with a KMP mobile client

### Cons of switching to SQLDelight
- **Massive rewrite** — every repo, query class, and the `Transaction` helper would need to be rewritten
- **Loss of dynamic query composition** — your most complex queries can't be expressed as static `.sq` files without workarounds
- **Worse PostgreSQL support** — `TEXT[]`, `BIGINT[]`, `excluded.*` in upserts, `COALESCE` with excluded fields
- **Wrong tool for the job** — SQLDelight targets mobile/SQLite; you're building a server-side JVM app on PostgreSQL
- **No Flyway integration** — SQLDelight has its own migration system; you'd need to abandon or bridge your Flyway setup
- **Less expressive for server-side** — jOOQ supports every PostgreSQL feature; SQLDelight supports a subset

---

## Verdict

**Do not switch. jOOQ is the right tool for SpendLane.**

Your project is a **server-side Kotlin/JVM app with PostgreSQL** that uses advanced SQL features (array types, upserts with `excluded`, multi-join queries with subqueries). This is exactly what jOOQ was built for, and exactly what SQLDelight was *not* built for.

The bus factor concern about jOOQ is real but mitigated by:
1. jOOQ has a **commercial entity** behind it (Data Geekery GmbH) — it's Lukas Eder's full-time business, not a side project
2. jOOQ is **mature and stable** (15 years) — even if development slowed, the current version would serve you for years
3. The dual-license has a **free OSS edition** that covers your use case

SQLDelight would make sense if you were building a **Kotlin Multiplatform app with local SQLite storage**. For a server-side Ktor + PostgreSQL backend, switching would be trading a perfect-fit tool for a worse one — purely to address a maintainership risk that, in jOOQ's case, is lower than it appears on paper.
