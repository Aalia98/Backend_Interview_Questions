# 18. Backend with Go (frequently asked)

### How do you build an HTTP server in Go?
`net/http`: `http.HandleFunc("/users", handler)` and `http.ListenAndServe(":8080", nil)`. A handler has the signature `func(w http.ResponseWriter, r *http.Request)`. Go 1.22 added method and path patterns: `mux.HandleFunc("GET /users/{id}", h)`. Popular routers: chi, gin, echo. Each request is served in its own goroutine.

### What is middleware?
A function that wraps a handler to add common behaviour (logging, auth, recovery from panic, CORS) before/after the real handler: `func(next http.Handler) http.Handler`.

### How do you work with JSON?
`json.NewDecoder(r.Body).Decode(&v)` to read, `json.NewEncoder(w).Encode(v)` to write. Use struct tags (`json:"name"`). Validate input, limit body size, return proper status codes.

### How do you use a database in Go?
`database/sql` with a driver (for MySQL `go-sql-driver/mysql`). `sql.DB` is a connection pool, safe for concurrent use; create it once. Use `QueryContext`/`ExecContext`, `defer rows.Close()`, check `rows.Err()`. Always use placeholders (`?`) to prevent SQL injection. Configure `SetMaxOpenConns`, `SetMaxIdleConns`, `SetConnMaxLifetime`. Transactions: `tx, _ := db.BeginTx(ctx, nil)`, `defer tx.Rollback()`, `tx.Commit()`.

### How do you do graceful shutdown?
Listen for SIGINT/SIGTERM (`signal.NotifyContext`), then call `server.Shutdown(ctx)` with a timeout so in-flight requests finish before exit. Important with Docker/Kubernetes.

### Typical project structure
`cmd/app/main.go`, `internal/` (handlers, services, repositories, models), `pkg/` for public code, `config/`, `migrations/`. Layers: handler (HTTP) -> service (business logic) -> repository (DB), connected through interfaces.

### How do you handle config and secrets?
Environment variables (or a config file) loaded at start; never commit secrets, use `.env` + `.gitignore`.

### How do you design a REST API in Go?
Proper HTTP methods and status codes, input validation, consistent error JSON, pagination, versioning (`/v1/`), timeouts, structured logging, request IDs, context everywhere, authentication (JWT) in middleware.