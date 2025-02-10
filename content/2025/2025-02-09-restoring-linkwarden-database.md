---
title: Restoring Linkwarden Database
tags:
  - tools
  - devops
  - docker
  - databases
---

I had an issue with [Linkwarden](https://github.com/linkwarden/linkwarden) recently. It's a fabulous piece of software but somehow managed to lose its own password to its Postgres database. How it accomplished this I have no idea, but as a result when you would launch the [docker compose](https://github.com/frozenfoxx/docker-bricksandblocks/blob/main/compose/linkwarden.yml) file I use it would send P1000 errors, complaining it could not connect to the database. To fix this, I did the following.
- Ensure the container running the database is functional
- Input the following:
```shell
docker exec -it linkwarden-postgres /bin/bash
cd /var/lib/postgresql/data
pg_dump --data-only --inserts -U linkwarden linkwarden > linkwarden-backup.sql
```
- Exit the container, shut it down, copy the backup to a safe location elsewhere (and keep it handy)
- Wipe the directory for linkwarden (`rm -rf /path/to/linkwarden-postgres/*`)
- Restart the container stack (`docker compose -f linkwarden.yml up`)
- After it's initialized the database, execute the following:
```shell
cp [safe location of linkwarden-backup.sql] /path/to/linkwarden-postgres/
docker exec -it linkwarden-postgres /bin/bash
cd /var/lib/postgresql/data
psql -U linkwarden linkwarden < linkwarden-backup.sql
```

At this point you've restored the database, sans DB user, and you should be able to access Linkwarden again without issue. I would recommend taking a backup at this point just in case.