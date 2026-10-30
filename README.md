lierc development environment
=============================

Runs the whole lierc stack locally: liercd, the logger, notifier and
identd from [lierc](https://github.com/lierc/lierc), the API and Go
stream server from [lierc-api](https://github.com/lierc/lierc-api),
Postgres, nsqd, the ergo IRC server and nginx serving
[lierc-minimal-ui](https://github.com/lierc/lierc-minimal-ui).

This is for development, not production. The production setup lives in
relaychat-infra.

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

The schema in `lierc-postgres/lierc.sql` is loaded only when the
`postgres` volume is first created. Run `docker compose down -v` to
start over with an empty database.

Editing the UI
--------------

`lierc-minimal-ui` is mounted into nginx, so changes show up on reload.
There is no build step.

Building lierc or lierc-api from source
---------------------------------------

By default the services run the images published to ghcr.io. To build
from local checkouts instead (`../lierc` and `../lierc-api` by default,
or set `LIERC_SRC` / `LIERC_API_SRC`):

```
docker compose -f docker-compose.yml -f docker-compose.build.yml up -d --build
```
