---
created: 2026-05-09 05:25
tags:
  - note
  - tip
  - guide
  - dev/server
  - dev/hosting
  - dev/database
aliases:
---
By default, [[PostgreSQL]] is extremely permissive to `PUBLIC` user.
This is not great for public services.
To harden the server, you should revoke all permissions from the `PUBLIC` user by running the following:

```postgresql
REVOKE ALL ON DATABASE template1 FROM PUBLIC;
REVOKE ALL ON DATABASE postgres FROM PUBLIC;
```

> [!NOTE]
> Make sure to replace `postgres` with the actual default db name your server uses.
> `template1` refers to the template db used when creating all the new dbs.
> If you previously created dbs, you must run the same command for them.