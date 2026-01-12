# AGENTS.md Templates

Templates for common project types. Adapt based on actual project analysis.

## PHP/Symfony Project

```markdown
# AGENTS.md

## Project Overview
[Description of what this application does]

## Tech Stack
- **Language:** PHP 8.x
- **Framework:** Symfony 6.x
- **Database:** MySQL 8.0
- **Cache:** Redis
- **Queue:** RabbitMQ (if applicable)

## Getting Started

### Prerequisites
- PHP 8.2+
- Composer 2.x
- MySQL 8.0+
- Docker (optional)

### Setup
```bash
composer install
cp .env.example .env
php bin/console doctrine:database:create
php bin/console doctrine:migrations:migrate
```

### Development
```bash
symfony serve
# or
docker-compose up -d
```

## Common Commands

| Command | Description |
|---------|-------------|
| `composer install` | Install dependencies |
| `php bin/console` | Symfony console |
| `php bin/phpunit` | Run tests |
| `php bin/console doctrine:migrations:migrate` | Run migrations |
| `php bin/console doctrine:migrations:diff` | Generate migration |
| `php bin/console cache:clear` | Clear cache |

## Code Style
- PSR-12 (enforced by PHP-CS-Fixer)
- Run `composer run cs-fix` before commit

## Testing
```bash
php bin/phpunit
php bin/phpunit --filter ClassName
php bin/phpunit tests/Unit/
```

## Before Committing
1. `composer run lint`
2. `php bin/phpunit`
3. `php bin/console lint:container`
```

## Node.js/TypeScript Project

```markdown
# AGENTS.md

## Project Overview
[Description]

## Tech Stack
- **Runtime:** Node.js 20.x
- **Language:** TypeScript 5.x
- **Framework:** [Express/Fastify/NestJS]
- **Database:** [PostgreSQL/MongoDB]
- **ORM:** [Prisma/TypeORM/Drizzle]

## Getting Started

### Prerequisites
- Node.js 20+
- pnpm 8+ (or npm/yarn)

### Setup
```bash
pnpm install
cp .env.example .env
pnpm db:migrate
```

### Development
```bash
pnpm dev
```

## Common Commands

| Command | Description |
|---------|-------------|
| `pnpm dev` | Start dev server with hot reload |
| `pnpm build` | Build for production |
| `pnpm test` | Run tests |
| `pnpm lint` | Run ESLint |
| `pnpm typecheck` | Run TypeScript checks |
| `pnpm db:migrate` | Run database migrations |
| `pnpm db:generate` | Generate Prisma client |

## Code Style
- Biome for formatting and linting
- Run `pnpm lint:fix` to auto-fix

## Testing
```bash
pnpm test
pnpm test --watch
pnpm test:coverage
```

## Before Committing
1. `pnpm typecheck`
2. `pnpm lint`
3. `pnpm test`
```

## React/Next.js Frontend

```markdown
# AGENTS.md

## Project Overview
[Description]

## Tech Stack
- **Framework:** Next.js 14 (App Router)
- **Language:** TypeScript
- **Styling:** Tailwind CSS
- **State:** [Zustand/Redux/React Query]
- **Testing:** Vitest + Testing Library

## Getting Started

### Setup
```bash
pnpm install
cp .env.example .env.local
```

### Development
```bash
pnpm dev
# Opens http://localhost:3000
```

## Common Commands

| Command | Description |
|---------|-------------|
| `pnpm dev` | Start dev server |
| `pnpm build` | Production build |
| `pnpm start` | Start production server |
| `pnpm test` | Run Vitest |
| `pnpm lint` | Run ESLint |
| `pnpm storybook` | Start Storybook (if available) |

## Project Structure
```
src/
├── app/           # Next.js App Router pages
├── components/    # Reusable UI components
├── hooks/         # Custom React hooks
├── lib/           # Utilities and helpers
├── services/      # API client functions
└── types/         # TypeScript types
```

## Code Style
- Components: PascalCase (`UserCard.tsx`)
- Hooks: camelCase with `use` prefix (`useAuth.ts`)
- Use Tailwind for styling, avoid inline styles

## Testing
```bash
pnpm test
pnpm test:ui  # Interactive mode
```

## Before Committing
1. `pnpm typecheck`
2. `pnpm lint`
3. `pnpm test`
```

## Python Project

```markdown
# AGENTS.md

## Project Overview
[Description]

## Tech Stack
- **Language:** Python 3.11+
- **Framework:** [FastAPI/Django/Flask]
- **Database:** PostgreSQL
- **ORM:** SQLAlchemy / Django ORM

## Getting Started

### Prerequisites
- Python 3.11+
- Poetry (or pip + venv)

### Setup
```bash
poetry install
cp .env.example .env
poetry run alembic upgrade head
```

### Development
```bash
poetry run uvicorn app.main:app --reload
```

## Common Commands

| Command | Description |
|---------|-------------|
| `poetry install` | Install dependencies |
| `poetry run pytest` | Run tests |
| `poetry run alembic upgrade head` | Run migrations |
| `poetry run alembic revision --autogenerate -m "msg"` | Create migration |
| `poetry run ruff check .` | Lint code |
| `poetry run ruff format .` | Format code |

## Code Style
- Ruff for linting and formatting
- Type hints required for all functions
- Docstrings for public APIs

## Testing
```bash
poetry run pytest
poetry run pytest -k test_name
poetry run pytest --cov=app
```

## Before Committing
1. `poetry run ruff check .`
2. `poetry run ruff format .`
3. `poetry run pytest`
```

## Go Project

```markdown
# AGENTS.md

## Project Overview
[Description]

## Tech Stack
- **Language:** Go 1.22+
- **Framework:** [Gin/Echo/Chi/stdlib]
- **Database:** PostgreSQL
- **ORM:** sqlc / GORM

## Getting Started

### Prerequisites
- Go 1.22+
- Docker (for local DB)

### Setup
```bash
go mod download
cp .env.example .env
make migrate-up
```

### Development
```bash
make run
# or
go run ./cmd/server
```

## Common Commands

| Command | Description |
|---------|-------------|
| `make run` | Start server |
| `make test` | Run tests |
| `make build` | Build binary |
| `make lint` | Run golangci-lint |
| `make migrate-up` | Run migrations |
| `make generate` | Generate code (sqlc, mocks) |

## Project Structure
```
cmd/
├── server/        # Main application entry
internal/
├── handler/       # HTTP handlers
├── service/       # Business logic
├── repository/    # Data access
└── model/         # Domain models
pkg/               # Public packages
```

## Code Style
- Follow Effective Go
- golangci-lint enforces style
- Table-driven tests preferred

## Testing
```bash
go test ./...
go test -v ./internal/service/...
go test -cover ./...
```

## Before Committing
1. `make lint`
2. `make test`
3. `go mod tidy`
```

## Docker-based Project

```markdown
# AGENTS.md

## Project Overview
[Description]

## Tech Stack
[List main technologies]

## Getting Started

### Prerequisites
- Docker 24+
- Docker Compose 2.x

### Setup
```bash
cp .env.example .env
docker-compose up -d
```

### Development
```bash
docker-compose up
# App available at http://localhost:3000
```

## Common Commands

| Command | Description |
|---------|-------------|
| `docker-compose up -d` | Start all services |
| `docker-compose down` | Stop all services |
| `docker-compose logs -f app` | Follow app logs |
| `docker-compose exec app sh` | Shell into app container |
| `docker-compose exec db psql -U postgres` | Database shell |

## Services
| Service | Port | Description |
|---------|------|-------------|
| app | 3000 | Main application |
| db | 5432 | PostgreSQL |
| redis | 6379 | Cache |

## Running Tests
```bash
docker-compose exec app pnpm test
```

## Before Committing
1. Ensure `docker-compose up` works
2. Run tests in container
3. Check logs for errors
```
