# SQLredis
intend to implement a proxy to redis cluster which supports SQL query from clients

# design
## Common Constraint
use "_" as all key prefix to distinct from other keys
## Table
### Table list
example:

key | type | value
--- | --- | ---
_tables| list | `tableA, tableB` 

### Table schema
prefix: "tb:", we maintain a table schema for easier migration to other SQL 

example:

key | type | field | value
--- | --- | --- | ---
_tb:product:type| hash | `id, name, sales, created_at, updated_at` | `BIGINT(20), VARCHAR(64), BIGINT(20), daytime, timestamp`
_tb:product:index| hash | `id, name, sales, created_at, updated_at` | `pk, uk, ix, ix, ix`
_tb:product:default| hash | `id, name` | `auto_incr, "", 0, now(), now()`
_tb:product:auto_incr:id| string | - | 1 
_tb:product:auto_update:updated_at| string | - | now() 

## Index
prefix:

- primary_key: "${tablename}:pk:${index}"
- index:  "${tablename}:ix:${index}"
- unique_index: "${tablename}:uk:${index}"

example:

key | type | field | value
--- | --- | --- | ---
_product:pk:id| zset | `1,2,3,4,5` (its id) | `1,2,3,4,5` (its id)
_product:ix:name| hash | `nameA, nameB` | `1, 2` (its id)
_product:ix:sales| hash | `100, 200` | `1, 2` (its id)

## Record

example:
key | type | field | value
--- | --- | --- | ---
_product:1 | hash | `id, name, sales, created_at, updated_at` | `1, nameA, 100, xxx, xxx`

## Plan to support later
- foreign key and reference constraint
- transaction

# TODOs
- parse SQL
- support basic DDL and simple CRUD
- support mysql client
- support being used on client side

