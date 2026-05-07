# migrate

A fork of [golang-migrate/migrate](https://github.com/golang-migrate/migrate) — Database migrations written in Go. Use as CLI or import as library.

[![Go Reference](https://pkg.go.dev/badge/github.com/your-org/migrate.svg)](https://pkg.go.dev/github.com/your-org/migrate)
[![CI](https://github.com/your-org/migrate/actions/workflows/ci.yaml/badge.svg)](https://github.com/your-org/migrate/actions/workflows/ci.yaml)
[![Go Report Card](https://goreportcard.com/badge/github.com/your-org/migrate)](https://goreportcard.com/report/github.com/your-org/migrate)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

## Features

- **Stateless** — no state is stored in your database (unless you use the advisory lock feature)
- **Multi-database support** — PostgreSQL, MySQL, SQLite, MongoDB, and more
- **Multiple source drivers** — filesystem, Go embed, S3, GitHub, and more
- **CLI and library** — use as a standalone CLI tool or import as a Go library
- **Extensible** — add custom database and source drivers

## Supported Databases

| Database       | Driver              |
|----------------|---------------------|
| PostgreSQL     | `postgres`          |
| MySQL          | `mysql`             |
| SQLite3        | `sqlite3`           |
| MongoDB        | `mongodb`           |
| CockroachDB    | `cockroachdb`       |
| ClickHouse     | `clickhouse`        |
| Cassandra      | `cassandra`         |

## Installation

### CLI

```bash
# Using Homebrew
brew install your-org/tap/migrate

# Using Go
go install github.com/your-org/migrate/cmd/migrate@latest

# Using Docker
docker pull your-org/migrate
```

### Library

```bash
go get github.com/your-org/migrate/v4
```

## Usage

### CLI

```bash
# Apply all up migrations
migrate -path ./migrations -database "postgres://localhost:5432/mydb?sslmode=disable" up

# Rollback the last migration
migrate -path ./migrations -database "postgres://localhost:5432/mydb?sslmode=disable" down 1

# Show current migration version
migrate -path ./migrations -database "postgres://localhost:5432/mydb?sslmode=disable" version

# Create a new migration
migrate create -ext sql -dir ./migrations -seq create_users_table
```

### Library

```go
import (
    "github.com/your-org/migrate/v4"
    _ "github.com/your-org/migrate/v4/database/postgres"
    _ "github.com/your-org/migrate/v4/source/file"
)

func main() {
    m, err := migrate.New(
        "file://./migrations",
        "postgres://localhost:5432/mydb?sslmode=disable",
    )
    if err != nil {
        log.Fatal(err)
    }
    if err := m.Up(); err != nil && err != migrate.ErrNoChange {
        log.Fatal(err)
    }
}
```

## Migration Files

Migration files follow the naming convention:

```
{version}_{title}.up.{extension}
{version}_{title}.down.{extension}
```

Example:
```
000001_create_users_table.up.sql
000001_create_users_table.down.sql
000002_add_email_index.up.sql
000002_add_email_index.down.sql
```

## Development

### Prerequisites

- Go 1.21+
- Docker (for running integration tests)

### Running Tests

```bash
# Unit tests
go test ./...

# Integration tests (requires Docker)
make test-integration
```

### Building

```bash
make build
```

## Contributing

Contributions are welcome! Please open an issue or submit a pull request.

1. Fork the repository
2. Create your feature branch (`git checkout -b feat/my-feature`)
3. Commit your changes (`git commit -m 'feat: add my feature'`)
4. Push to the branch (`git push origin feat/my-feature`)
5. Open a Pull Request

## License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.

## Acknowledgements

This project is a fork of [golang-migrate/migrate](https://github.com/golang-migrate/migrate). All credit for the original implementation goes to the original authors and contributors.
