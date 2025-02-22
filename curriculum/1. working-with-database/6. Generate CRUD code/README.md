# Generate CRUD Golang code from SQL
## Two ways to interact with SQL

1. DATABASE / SQL
- Very fast & straighforward
- Manual mapping SQL fields to variables
- Easy to make mistakes, not caught until runtime

2. GORM:
- CRUD functions already implemented, very short production code
- Must learn to write queries using gorm's function
- Run slowly on high load

3. SQLX
- Quite fast & easy to use
- Fields mapping via query text & struct tags
- Failure won't occur until runtime

4. SQLC
- Very fast & easy to use
- Automatic code generation
- Catch SQL query errors before generating codes
- Full support Postgres. MySQL is experimental

