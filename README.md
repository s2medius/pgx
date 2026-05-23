# pgx - PostgreSQL Driver and Toolkit

This is a fork of [jackc/pgx](https://github.com/jackc/pgx), a pure Go driver and toolkit for PostgreSQL.

## Features

- Pure Go implementation — no CGO required
- Low-level PostgreSQL wire protocol access
- Supports PostgreSQL 12 and higher
- Connection pooling via `pgxpool`
- Batch queries
- `COPY` protocol support
- Extensive type support including JSON, arrays, hstore, and more
- Context-aware API
- Prepared statements
- Extended query protocol

## Installation

```bash
go get github.com/your-org/pgx/v5
```

## Quick Start

```go
package main

import (
	"context"
	"fmt"
	"os"

	"github.com/your-org/pgx/v5"
)

func main() {
	conn, err := pgx.Connect(context.Background(), os.Getenv("DATABASE_URL"))
	if err != nil {
		fmt.Fprintf(os.Stderr, "Unable to connect to database: %v\n", err)
		os.Exit(1)
	}
	defer conn.Close(context.Background())

	var greeting string
	err = conn.QueryRow(context.Background(), "SELECT 'Hello, pgx!'").Scan(&greeting)
	if err != nil {
		fmt.Fprintf(os.Stderr, "QueryRow failed: %v\n", err)
		os.Exit(1)
	}

	fmt.Println(greeting)
}
```

## Connection Pooling

For production use, prefer `pgxpool` to manage a pool of connections:

```go
import "github.com/your-org/pgx/v5/pgxpool"

pool, err := pgxpool.New(context.Background(), os.Getenv("DATABASE_URL"))
if err != nil {
	log.Fatal(err)
}
defer pool.Close()
```

## Development

This project uses a Dev Container for a consistent development environment.

### Prerequisites

- [Docker](https://www.docker.com/)
- [VS Code](https://code.visualstudio.com/) with the [Dev Containers extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)

### Getting Started

1. Clone the repository
2. Open in VS Code and reopen in container when prompted
3. The PostgreSQL instance will be available automatically

### Running Tests

```bash
export DATABASE_URL="postgres://postgres:password@localhost:5432/pgx_test"
go test ./...
```

## License

MIT License — see [LICENSE](LICENSE) for details.

## Acknowledgements

This project is a fork of [jackc/pgx](https://github.com/jackc/pgx). All credit for the original implementation goes to Jack Christensen and contributors.
