# rcos-presto-docs

## Helpful Commands:

Start CLI after spinning up server:
`./presto-cli/target/presto-cli-0.292-SNAPSHOT-executable.jar --catalog memory --schema default`

Creating a table with id, name and eventdate with type Date:
`create table test_date ( id BIGINT, name VARCHAR, event_date DATE );`

Insert today’s date into the table:
`INSERT INTO default.test_date (id, name, event_date) VALUES (1, 'Today’s Event', current_date);`

Test if the date was inserted by filtering:
`SELECT * FROM test_date WHERE date(event_date) = DATE '1984-01-08';`

Show what’s going on in the backend when you insert:
`EXPLAIN SELECT * FROM test_date WHERE date(event_date) = DATE '1985-01-08';`

Setting up Default Schemas from local, since we dont have any connected

`presto:default> SHOW SCHEMAS FROM MEMORY;`

`presto:default> USE MEMORY.DEFAULT;`

Display Table
`presto:default> SHOW TABLES;`
