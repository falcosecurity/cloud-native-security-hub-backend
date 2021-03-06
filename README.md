# Cloud Native Security Hub

![last commit](https://flat.badgen.net/github/last-commit/falcosecurity/cloud-native-security-hub-backend?icon=github) ![licence](https://flat.badgen.net/github/license/falcosecurity/cloud-native-security-hub-backend)

Cloud Native Security Hub is a platform for discovering and sharing rules and
configurations for cloud native security tools.

This repository contains the HTTP API and backend code that runs the
https://securityhub.dev site

## Usage

This code requires a recent golang version (1.13) and it uses modules to handle
the dependencies.

### Configuration

This project requires a PostgreSQL 11 server running. And you configure the app
to attack the database using the `DATABASE_URL` environment variable, which contains
the connection string for your server.

For example: `DATABASE_URL="postgres://username:password@127.0.0.1/db_name?sslmode=disable"`

### cmd/server

This is the HTTP API server and it will listen to requests on the `8080` port.

```
$ go run cmd/server/main.go
```

### cmd/dbimport

You need to setup a couple of environment variables previously to import any
data in the database:

* `RESOURCES_PATH`: Path to securityhub/resources directory
* `VENDOR_PATH`: Path to securityhub/vendors directory

These directories can be found in the [Cloud Native Security Hub Resources repository](https://github.com/falcosecurity/cloud-native-security-hub).

Then with the `DATABASE_URL` set, execute:

```
$ go run cmd/dbimport/main.go
```

And voila!

## Contributing

Contributors are welcome! You will need a quick package overview to understand
some design decisions:

* `pkg/usecases`: You will find the entry points in the `pkg/usecases` directory.
  One action per file, modeled like a command.
* `pkg/resource` and `pkg/vendor`: This is the domain code for security resources
  and vendors. You will find the repositories, entities and value objects.
* `test`: All our code is test driven, in this directory we have some fixtures
  to avoid repeating test data in the test code.
* `web`: The web is just a delivery mechanism, it is separated from the backend code
  and can be used as a library if you need to. Is responsible to JSON
  marshalling and HTTP communications.
* `db`: Contains the migration files for the database. For every change
  in the schema, you will need to create the corresponding migration file.
