# MongoDB learning log

Collections

Quick comparison to RDB: Collection là Table

## Create user

Roles: `read`, `write`, `readWrite`, `dbAdmin`, `userAdmin`, `root`.

```
db.createUser(
{
user: "admin",
pwd: "YourHardToGuessPassword",
roles: [ { role: "root", db: "admin" } ]
}
)
```