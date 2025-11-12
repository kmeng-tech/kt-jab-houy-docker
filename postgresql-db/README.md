# PostgreSQL DB

This directory provides a simple **Docker Compose setup** for running a **PostgreSQL 16** database inside a container.  
It is designed for local development environments or small-scale deployments where a lightweight and isolated PostgreSQL instance is needed.

---

## Directory Structure

```shell
kt-jab-houy-docker/postgresql-db/
├── .env.example # Example environment variables
├── .gitignore # Git ignore configuration
└── docker-compose.yml # Docker Compose configuration
```

---

## Environment Variables

Before running the container, create a `.env` file in the root directory by copying the provided example:

```bash
cp .env.example .env
```

You can modify the variables as needed:

| Variable               | Description                        | Default              |
| ---------------------- | ---------------------------------- | -------------------- |
| `COMPOSE_PROJECT_NAME` | Name of the Docker Compose project | `kt-jab-houy-docker` |
| `POSTGRES_USER`        | PostgreSQL username                | `myuser`             |
| `POSTGRES_PASSWORD`    | PostgreSQL password                | `mypassword`         |
| `POSTGRES_DB`          | Default database name              | `postgres`           |

---

## Docker Compose Configuration

### Service Overview

| Service         | Description                      | Port        |
| --------------- | -------------------------------- | ----------- |
| `postgresql-db` | PostgreSQL 16 (Alpine) container | `5454:5432` |

### Key Points

- Uses the **official** `postgres:16-alpine` image.
- Persists data using a **named Docker volume** `kt-jab-houy-pgdata-v16`.
- Connects to an **external Docker network** `kt-network` for communication with other containers.

---

## Getting Started

### 1. Create an External Network

If you haven’t created the external network yet:

```shell
docker network create kt-network
```

### 2. Start the Container

```shell
docker-compose up -d
```

### 3. Verify the Container

```shell
docker ps
```

You should see a container named `kt-postgresql-db` running.

### 4. Access the Container's PostgreSQL Shell

```shell
docker exec -it kt-postgresql-db psql -U myuser -d postgres
```

### 5. Create a User for the Database

```sql
-- create a database if you didn't create it yet
CREATE DATABASE "kt-jab-houy-db";

-- create a user with password
CREATE USER "kt-jab-houy-user" WITH ENCRYPTED PASSWORD 'password';

-- grant access the user to the database
GRANT ALL PRIVILEGES ON DATABASE "kt-jab-houy-db" TO "kt-jab-houy-user";
```

---

## Useful Commands

### Stop the Service

```shell
docker-compose down
```

### View Logs

```shell
docker-compose logs -f
```

### Access PostgreSQL Shell

```shell
docker exec -it kt-postgresql-db psql -U myuser -d postgres
```

### Remove Volumes (if you want a clean setup)

```shell
docker-compose down -v
```

---

## Notes

- The `.env` file is **ignored by Git** for security reasons.
- Update your `.env` file credentials carefully before sharing or deploying.
- The external network allows other services (like backend apps) to connect to this PostgreSQL instance easily.
