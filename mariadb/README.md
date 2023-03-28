# Mariadb & Monk

This repository contains Monk.io template to deploy mariadb system either locally or on cloud of your choice (AWS, GCP, Azure, Digital Ocean).

## Start

Set up Monk - [https://docs.monk.io/docs/monk-in-10/](https://docs.monk.io/docs/monk-in-10/)

Start `monkd` and login.

```bash
monk login --email=<email> --password=<password>
```

## Clone Monk mariadb repository

In order to load templates and change configuration simply use below commands:

```bash
git clone https://github.com/monk-io/monk-mariadb

# and change directory to the monk-mariadb/mariadb  template folder
cd monk-mariadb/mariadb
```

## Configuration

You can add/remove configuration of the template.

The current variables can be found in `mariadb/variables` section

```yaml
  variables:
    mariadb-image-tag: "latest"
    mariadb-root-password: "4wfoA7auxY"
    mariadb-database-name: "mariadb"
    mariadb-user-name: "mariadbuser"
    mariadb-user-password: "Cz9mGzmRtW"
```

## Template variables

| Variable                  | Description                    | Type   | Example      |
| ------------------------- | ------------------------------ | ------ | ------------ |
| **mariadb-image-tag**     | Mariab image version.          | string | latest       |
| **mariadb-root-password** | Mariab image version.          | string | "4wfoA7auxY" |
| **mariadb-database-name** | Mariab image version.          | string | latest       |
| **mariadb-user-name**     | Mariab database user name.     | string | mariadbuser  |
| **mariadb-user-password** | Mariab database user password. | string | "Cz9mGzmRtW" |

## Local Deployment

| First clone the repository simply run below command after launching `monkd`: |
| :--------------------------------------------------------------------------: |

```bash
➜  monk load MANIFEST

✨ Loaded:
 ├─🔩 Runnables:
 │  └─🧩 mariadb/mariadb
 ├─🔗 Process groups:
 │  └─🧩 mariadb/stack
 └─⚙️ Entity instances:
    └─🧩 mariadb/mariadb/metadata
✔ All templates loaded successfully

➜  monk list mariadb

✔ Got the list
Type      Template         Repository  Version  Tags
runnable  mariadb/mariadb  local       -        self hosted, database
group     mariadb/stack    local       -        -


➜  monk run  mariadb/stack

✔ Started local/mariadb/stack

```

This will start the entire mariadb/stack

## Cloud Deployment

To deploy the above system to your cloud provider, create a new Monk cluster and provision your instances.

```bash
➜  monk cluster new
? New cluster name mariadb
✔ Cluster created
Your cluster has been created successfully.

➜  monk cluster provider add -p gcp -f <path/to/your-key.json>
✔ Provider added successfully

➜  monk cluster grow -p gcp
? Cloud provider gcp
? Name of the new instance my-instance
? Tags (split by whitespace) mariadb
? Region europe-central2
? Zone europe-central2-a
? Instance type e2-medium
? Number of instances (or press ENTER to use default = 1) 3
? Default disk type for gcp is HDD (pd-standard). Would you like to change it? No
? Disk size (or press ENTER to use default = 250 GBs) 50
✔ Start creation of new instance(s) on gcp... DONE
✔ Creating node: my-instance-1 DONE
✔ Initializing node: my-instance-1 DONE
✔ Creating node: my-instance-2 DONE
✔ Initializing node: my-instance-2 DONE
✔ Creating node: my-instance-3 DONE
✔ Initializing node: my-instance-3 DONE
✔ Connecting: my-instance-1 DONE
✔ Syncing peer: my-instance-1 DONE
✔ Connecting: my-instance-2 DONE
✔ Connecting: my-instance-3 DONE
✔ Syncing peer: my-instance-2 DONE
✔ Syncing peer: my-instance-3 DONE
✔ Syncing nodes DONE
✔ Cluster grown successfully
```

Once cluster is ready execute the same command as for local and select your cluster (the option will appear automatically).

```bash
➜  monk load MANIFEST

✨ Loaded:
 ├─🔩 Runnables:
 │  └─🧩 mariadb/mariadb
 ├─🔗 Process groups:
 │  └─🧩 mariadb/stack
 └─⚙️ Entity instances:
    └─🧩 mariadb/mariadb/metadata
✔ All templates loaded successfully

➜  monk list mariadb

✔ Got the list
✔ Got the list
Type      Template         Repository  Version  Tags
runnable  mariadb/mariadb  local       -        self hosted, database
group     mariadb/stack    local       -        -

➜  monk run mariadb/stack

✔ Started local/mariadb/stack

```

## Logs & Shell

```bash
# show Mariadb logs
➜  monk logs -l 1000 -f local/mariadb/mariadb

```

## Stop, remove and clean up workloads and templates

```bash
➜ monk purge -x -a local/mariadb/mariadb local/mariadb/stack

✔ local/mariadb/mariadb purged
✔ local/mariadb/stack purged

```
