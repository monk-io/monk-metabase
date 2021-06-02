# Metabase & Monk

This repository contains Monk.io template to deploy Metabase system either locally or on cloud of your choice (AWS, GCP, Azure, Digital Ocean).

## Start

Setup Monk - https://docs.monk.io/monk-in-10/

Start `monkd` and login.

```bash
monk login --email=<email> --password=<password>
```

## Local Deployment

This template is available directly from Monkhub.io therefore if you want a quick deploy simply run below command after launching `monkd`:
```bash
➜ monk list metabase
✔ Got the list
Type      Template                     Repository  Version  Tags
runnable  metabase/metabase            local       -        -
runnable  metabase/postgres            local       12       dataops, database
group     metabase/stack               local       latest   -

➜  monk run metabase/stack

✔ Started metabase/stack

🔩 templates/local/metabase/stack
 └─🧊 Peer QmPtVZCdz2nD8j9sUTUaXZ2L2Dsox2EfNj7DuXiRES72rf
    ├─📦 templates-local-metabase-metabase-metabase
    │  ├─🧩 metabase/metabase:latest
    │  └─🔌 open localhost:3000 -> 3000
    └─📦 templates-local-metabase-postgres-postgres
       ├─🧩 postgres:12
       ├─💾 /Users/karol/.monk/volumes/db_data -> /var/lib/postgresql/data
       └─🔌 open localhost:5432 -> 5432

💡 You can inspect and manage your above stack with these commands:
	monk logs (-f) metabase/stack - Inspect logs
	monk shell     metabase/stack - Connect to the container's shell
	monk do        metabase/stack/action_name - Run defined action (if exists)
💡 Check monk help for more!
```

## Cloud Deployment
To deploy the above system to your cloud provider, create a new Monk cluster and provision your instances.
```bash
➜  monk cluster new
? New cluster name metabase-deployment
✔ Cluster created
Your cluster has been created successfully.

➜  monk cluster provider add
? Cloud provider gcp
? GOOGLE_APPLICATION_CREDENTIALS is set. Try load it? Yes
✔ Provider added successfully

➜  monk cluster grow -p gcp
? Name for the new instance metabase-instance
? Tags (split by whitespace) metabase
? Instance region (gcp) europe-west2
? Instance zone (gcp) europe-west2-c
? Instance type (gcp) e2-standard-2
? Disk Size (GBs) 30
? Number of instances (or press ENTER to use default = 1) 2
✔ Creating a new instance(s) on gcp...
✔ Syncing nodes DONE
✔ Cluster grown successfully
```
Once cluster is ready execute the same command as for local just specify the tag
```bash
➜  monk run -t metabase metabase/stack
```

## Configuration
You can add/remove or override current configuration of the template or create a brand new one which could inherit the components that you require.

The current variables can be found in `metabase/stack/variables` section
```bash
  variables:
    defines: variables
    postgres-db-user: metabase
    postgres-db-password: "DB_PASS"
    postgres-db-port: 5432
    postgres-db-name: metabase
    metabase-port: 3000
```
To override the settings or add new ones for your own template you can inherit it in this way:
```bash
namespace: my-metabase

metabase-prod:
  defines: process-group
  inherits: metabase/stack
  variables:
    postgres-db-user: metabase-prod
    postgres-db-password: secure-postgres-pass
```

### Logs & Shell
```bash
➜  monk logs -f metabase/metabase

➜  monk shell metabase/metabase
```
