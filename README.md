Setup
=====

Run:

```
docker-compose build
docker-compose up -d
```

Editing the user interface/javsacript
=====================================

Edit files in lierc-basicui. The `basicui` container will rebuild
and update the necessary files. You can watch the logs with:

```
docker-compose logs -f basicui
```
