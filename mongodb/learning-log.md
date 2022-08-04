# MongoDB learning log

Collections

Quick comparison to RDB: Collection là Table

## Built-in roles

Roles: `read`, `write`, `readWrite`, `dbAdmin`, `userAdmin`, `root`.

## Create user

```
db.createUser(
{
user: "admin",
pwd: "YourHardToGuessPassword",
roles: [
        { role: "root", db: "admin"
        }
    ]
}
)
```

## Update user

```
db.runCommand( {
    updateUser : "admin",
    roles : [ { role : "root", db : "admin" } ]
 } )

```

## Get user

```
db.getUser("admin")
```