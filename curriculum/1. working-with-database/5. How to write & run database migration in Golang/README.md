## Start to migrate

```bash
migrate create -ext sql -dir db/migration -seq init_schema
```

## Migrate Up

```txt
old db -> migration files (1.up.sql, 2.up.sql and 3.up.sql) -> new db
```

## Mirate Down

```txt
old db <- migration files (1.up.sql, 2.up.sql and 3.up.sql) <- new db
```

## Migrate path

```bash
migrate -path db/migration -database "postgresql://root:secret@localhost:5432/simple_bank?sslmode=disable" -verbose up
```

- For example:

```bash
(base) tieuma@Tieus-MacBook-Pro simplebank % migrate -path db/migration -database "postgresql://root:secret@localhost:54997/simple_bank?sslmode=disable" -verbose up
2025/02/19 21:29:09 Start buffering 1/u init_schema
2025/02/19 21:29:09 Read and execute 1/u init_schema
2025/02/19 21:29:09 Finished 1/u init_schema (read 3.058625ms, ran 3.193834ms)
2025/02/19 21:29:09 Finished after 9.031458ms
2025/02/19 21:29:09 Closing source and database
```