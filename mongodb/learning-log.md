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

## Enable Access Control for MongoDB Server

With access control enabled, ensure you have a user with userAdmin or userAdminAnyDatabase role in the admin database. This user can administrate user and roles such as: create users, grant or revoke roles from users, and create or modify customs roles.
