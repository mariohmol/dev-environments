
# Heroku 


## Postgres

[Seeing "FATAL: no pg_hba.conf entry" errors in Postgres](https://help.heroku.com/DR0TTWWD/seeing-fatal-no-pg_hba-conf-entry-errors-in-postgres)

In your connection using nodejs for example:
```js
 ssl: {
    rejectUnauthorized: false
  }
```

Config in the heroku environment variable:
```
PGSSLMODE=no-verify