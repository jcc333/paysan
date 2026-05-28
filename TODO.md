# paysan TODO

## Broad Design

- A `warp`-like server building library.
- Includes some examples.
- Operates on `RequestWrapper` and `ResponseWrapper`s

## Tasks

| Status | # | Task | Priority |
|---|---|------|----------|
|   | 1 | Implement Filter combinators: `&&`, `||`, `not`, path/method matching | HIGH |
|   | 2 | Implement Middleware composition: `chain`, `then`, `around` | HIGH |
|   | 3 | Implement `Router.add(route Filter, middleware chain, responder)` | HIGH |
|   | 4 | Integrate httprouter for path matching + param extraction | HIGH |
|   | 5 | Implement `Server` struct (task-per-request, ties Router to http.Server) | HIGH |
|   | 6 | Add common middleware: logging, error handling, metrics, timeout | MEDIUM |
|   | 7 | Add Filter helpers: `pathRegex`, `queryParam`, `header`, `pathParam` | MEDIUM |
|   | 8 | Write example endpoints demonstrating the stack | MEDIUM |
|   | 9 | Add tests for Filter, Middleware, and Router | MEDIUM |


## Dependency Graph (DAG)

```
                    ┌───────────────┐
                    │  1. Request   │
                    │  Response     │
                    │   Types       │
                    └───────┬───────┘
                            │
        ┌───────────────────┼───────────────────┐
        ▼                   ▼                   ▼
┌───────────────┐   ┌───────────────┐   ┌───────────────┐
│  2. Filter     │   │  3. Middleware│   │  5. httprouter │
│  Combinators   │   │  Composition  │   │  Integration   │
└───────┬───────┘   └───────┬───────┘   └───────┬───────┘
        │                   │                   │
        └───────────────────┼───────────────────┘
                            ▼
                    ┌───────────────┐
                    │  4. Router    │
                    │  Implementation│
                    └───────┬───────┘
                            │
        ┌───────────────────┼───────────────────┐
        ▼                   ▼                   ▼
┌───────────────┐   ┌───────────────┐   ┌───────────────┐
│  7. Common     │   │  8. Filter    │   │  6. Server    │
│  Middleware    │   │  Helpers      │   │  (task/req)   │
└───────────────┘   └───────────────┘   └───────┬───────┘
                                                    │
        ┌───────────────────────────────────────────┘
        ▼
┌───────────────┐
│  9. Example    │
│  Endpoints     │
└───────┬───────┘
        │
┌───────▼───────┐
│  10. Tests    │
└───────────────┘
```

**Dependencies:**
- **1** → 2, 3, 5, 6
- **2** → 4, 8
- **3** → 4, 7
- **4** → 6
- **5** → 4
- **6** → 9
- **7** → 9
- **8** → 9
- **9** → 10

**Build Order:** 1 → {2,3,5} → 4 → 6 → {7,8} → 9 → 10
