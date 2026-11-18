lierc development environment
=============================

Runs the whole lierc stack locally: liercd, the logger (which also
sends notifications) and identd from
[lierc](https://github.com/lierc/lierc), the API (Go, with
the event stream) from [lierc-api](https://github.com/lierc/lierc-api),
Postgres, nsqd, the ergo IRC server and nginx serving
[lierc-web](https://github.com/lierc/lierc-web).

This is for development, not production. The production setup lives in
relaychat-infra.

Questions or ideas? Come talk in [#relaychat](https://relaychat.party/join/?network=relaychat&channel=relaychat).

Setup
-----

```
git submodule update --init
docker compose up -d
```

Then open http://localhost:8080/ and register an account. Set
`HTTP_PORT` to use a different port, eg `HTTP_PORT=8088 docker compose up -d`.

| URL                    | What                                           |
|------------------------|------------------------------------------------|
| http://localhost:8080  | the UI, with the API at `/api/`                |
| http://localhost:8025  | mailpit, which catches verification and notification email |
| http://localhost:4171  | nsqadmin                                       |
| localhost:5432         | Postgres (user `lierc`, password `changeme`)   |
| localhost:6667         | ergo, the IRC server new accounts are connected to |

lierc-api creates the schema when it starts, by applying its migrations
(`internal/db/migrations` in lierc-api), and applies new ones on later
starts. Run `docker compose down -v` to start over with an empty
database. A volume from before lierc-api had migrations has the tables
but no `schema_migrations`, and lierc-api stops on its first migration:
run `docker compose down -v` once to replace it.

Editing the UI
--------------

`lierc-web` is mounted into nginx, so changes show up on reload (nginx
sends `Cache-Control: no-cache`, so the browser checks each file). There
is no build step; the release build in lierc-web is only for deploys.

Building lierc or lierc-api from source
---------------------------------------

By default the services run the images published to ghcr.io. To build
from local checkouts instead (`../lierc` and `../lierc-api` by default,
or set `LIERC_SRC` / `LIERC_API_SRC`):

```
docker compose -f docker-compose.yml -f docker-compose.build.yml up -d --build
```
