## How to write docker-compose file and control service start-up orders

```bash
version: '3.9'
services:
  postgres:
    image: postgres:17-alpine
    environment:
      - POSTGRES_USER=root
      - POSTGRES_PASSWORD=secret
      - POSTGRES_DB=simple_bank
  
  api:
    build:
      context: .
      dockerfile: dockerfile
    ports:
      - "8080:8080"
    environment:
      - DB_SOURCE=postgresql://root:secret@postgres:5432/simple_bank?sslmode=disable
```

```bash
docker compose up
```


### Udpate the Dockerfile

```bash
# Build stage
FROM golang:1.23-alpine AS builder
WORKDIR /app
COPY . .
RUN go build -o main main.go
RUN apk add curl
RUN curl -L https://github.com/golang-migrate/migrate/releases/download/v4.18.2/migrate.linux-amd64.tar.gz | tar xvz

# Run stage
FROM alpine
WORKDIR /app
COPY --from=builder /app/main .
COPY --from=builder /app/migrate ./migrate
COPY app.env .
COPY db/migration ./migration

EXPOSE 8080
CMD ["/app/main"]
```

### Run compose up and down
- To 