# Mariadb & Monk

This repository contains Monk.io template to deploy mariadb system either locally or on cloud of your choice (AWS, GCP, Azure, Digital Ocean).


## Start

Set up Monk - https://docs.monk.io/docs/monk-in-10/

Start `monkd` and login.

```bash
monk login --email=<email> --password=<password>
```

## Clone Monk mariadb repository

In order to load templates and change configuration simply use below commands: 
```bash
git clone https://github.com/monk-io/monk-mariadb

# and change directory to the monk-mariadb/mariadb-ha-with-galera template folder
cd monk-mariadb/mariadb-ha-with-galera
```

## Configuration

You can add/remove configuration of the template.

The current variables can be found in `mariadb-ha/variables` section

```yaml
  variables:
    mariadb-image-tag: latest
    mariadb-root-user: root
    mariadb-root-password: 4wfoA7auxY
    mariadb-database-name: mariadb
    mariadb-user-name: mariadbuser
    mariadb-user-password: Cz9mGzmRtW
    mariadb-cluster-name: my_galera 
    mariadb-mariabackup-user: my_mariabackup_user 
    mariadb-mariabackup-password: cNQAW4GT
    mariadb-replication-user: my_replication_user
    mariadb-replication-password: FDkxXPB7
```

### Mariadb configuration files

You can find configuration file named custom.cnf in `/files` directory in repository and can edit before the running kit. Configuration file will be bound to the container while run monk-mariadb kit 


| Configuration File	 | Format Used | Directory in Container | Purpose 
|----------|-------------|------|---------|
| **custom.cnf** | cnf | `/opt/bitnami/mariadb/conf/my_custom.cnf` | Primary configuration file for Mariadb



##  Template variables

| Variable | Description | Type | Example |
|----------|-------------|------|---------|
| **mariadb-image-tag** | Mariab image version. | string | latest |
| **mariadb-root-password** | Mariab image version. | string | "4wfoA7auxY" |
| **mariadb-database-name** | Mariab image version. | string | latest |
| **mariadb-user-name** | Mariab database user name. | string | mariadbuser |
| **mariadb-user-password** | Mariab database user password. | string | "Cz9mGzmRtW" |



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
? Tags (split by whitespace) mariadbha
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
 │  ├─🧩 mariadb-ha/mariadb-bootstrap
 │  ├─🧩 mariadb-ha/monk-slave-2
 │  ├─🧩 mariadb-ha/mariadb-slave-common
 │  └─🧩 mariadb-ha/monk-slave-1
 ├─🔗 Process groups:
 │  └─🧩 mariadb-ha/stack
 └─⚙️ Entity instances:
    └─🧩 mariadb-ha/stack/metadata
✔ All templates loaded successfully

➜  monk list mariadb-ha

✔ Got the list
✔ Got the list
Type      Template                         Repository  Version  Tags
runnable  mariadb-ha/mariadb-bootstrap     local       -        -
runnable  mariadb-ha/mariadb-slave-common  local       -        -
runnable  mariadb-ha/monk-slave-1          local       -        -
runnable  mariadb-ha/monk-slave-2          local       -        -
group     mariadb-ha/stack                 local       -        self hosted, database


➜  monk run  mariadb-ha/stack

✔ Started local/mariadb/stack

```

## Logs & Shell

```bash
# show Mariadb master logs
➜  monk logs -l 1000 -f local/mariadb-ha/mariadb-bootstrap

# show Mariadb slave-1 logs
➜  monk logs -l 1000 -f local/mariadb-ha/monk-slave-1

# show Mariadb slave-2 logs
➜  monk logs -l 1000 -f local/mariadb-ha/monk-slave-2

```

## Stop, remove and clean up workloads and templates

```bash
➜ monk purge -x -a local/mariadb-ha/mariadb-bootstrap local/mariadb-ha/monk-slave-1  local/mariadb-ha/monk-slave-2  local/mariadb-ha/stack

✔ local/mariadb-ha/mariadb-bootstrap purged
✔ local/mariadb-ha/monk-slave-1 purged
✔ local/mariadb-ha/monk-slave-2 purged
✔ local/mariadb-ha/stack purged

```