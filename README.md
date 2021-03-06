# mds-provider-services

Services for working with [MDS `provider`][provider] data, built as runnable Docker containers.

These services are implemented via [`mds-provider`](https://github.com/CityofSantaMonica/mds-provider),
a general-purpose Python library for working with MDS Provider data.

## Batteries Included

The services are organized around specific functions. More detailed explanation can be found in service `README` files.

| service | description |
| --------- | ----------- |
| [`analytics`](analytics/) | Perform analysis on `provider` data |
| [`client`](client/) | [pgAdmin4][pgadmin] web client |
| [`db`](db/) | Work with a `provider` database |
| [`fake`](fake/) | Generate fake `provider` data for testing and development |
| [`ingest`](ingest/) | Ingest `provider` data from different sources |
| [`server`](server/) | Local [postgres][postgres] database server |

## Getting Started

Requires both [Docker][docker] and [Docker Compose][compose].

Commands below should be run from the root of this repository, where the `docker-compose.yml` file lives.

### 0. Create a `docker-compose.yml` file

Copy the `dev` file and edit as necessary. Compose automatically uses this file for service definitions and configuration.

You shouldn't have to make too many (if any) changes; see the next step for environment variable configuration.

```bash
cp docker-compose.dev.yml docker-compose.yml
```

Alternatively, use the `dev` file as-is by prepending a switch to `docker-compose` commands, e.g.:

```bash
docker-compose -f docker-compose.dev.yml CMD [OPTIONS] SERVICE [OPTIONS]
```

### 1. Create an `.env` file

Copy the sample and edit as necessary. Compose automatically sources this environment file for `docker-compose` commands.

```bash
cp .env.sample .env
```

Modify this file with your own settings, but the defaults should be good enough to get going.

### 2. Initialize the database

Build and start the necessary containers to load and explore a Postgres database.

```bash
bin/initdb.sh
```

Now you can browse to `http://localhost:PGADMIN_HOST_PORT` and login with the `PGADMIN_DEFAULT` credentials.

Attach to the server `POSTGRES_HOSTNAME`, database `MDS_DB`, with the `MDS` credentials.

### 3. Run individual service jobs

Generally, an individual service `SERVICE` can be run with a command like:

```bash
docker-compose run --rm SERVICE [OPTIONS]
```

See the `README` file in each service folder for more details.

### 4. Start a Jupyter Notebook server

`analytics`, `fake` and `ingest` all come with Jupyter Notebook servers that can be run locally:

```bash
bin/notebook.sh SERVICE
```

Now browse to `http://localhost:NB_HOST_PORT` and append the `/?token=<token>` param shown in the Notebook container startup output.

Note your `NB_HOST_PORT` may be different than the default shown in the container output (`8888`).

Also note that all of the services make use of the *same* `NB_HOST_PORT` environment variable, and so they cannot be run at the same time!

Modify `docker-compose.yml` if you need to use different ports to run Notebook servers on multiple services simultaneously.

[compose]: https://docs.docker.com/compose/overview/
[docker]: https://www.docker.com/
[pgadmin]: https://www.pgadmin.org/
[postgres]: https://www.postgresql.org/
[provider]: https://github.com/CityOfLosAngeles/mobility-data-specification/tree/master/provider
