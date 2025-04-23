# rcos-presto-docs

## Helpful Commands:

### Presto CLI Test Queries with Timestamp/Dates
- Start CLI after spinning up server:
`./presto-cli/target/presto-cli-0.292-SNAPSHOT-executable.jar --catalog memory --schema default`

- Creating a table with id, name and eventdate with type Date:
`CREATE TABLE test_ts ( id BIGINT, event_ts TIMESTAMP );`

- Insert today’s date into the table:
` insert into test_ts (id, event_ts) Values (1, TIMESTAMP '2025-04-09 10:12:34'),(2, TIMESTAMP '2025-04-09 23:59:59'), (3, TIMESTAMP '2025-04-10 00:00:00');
`

- Test if the date was inserted by filtering:
`SELECT COUNT(*) FROM test_ts WHERE event_ts >= TIMESTAMP '2025-04-09 00:00:00' AND event_ts < TIMESTAMP '2025-04-10 00:00:00';`

- Setting up Default Schemas from local, since we dont have any connected

- `presto:default> SHOW SCHEMAS FROM MEMORY;`

- `presto:default> USE MEMORY.DEFAULT;`

- Display Table
`presto:default> SHOW TABLES;`
