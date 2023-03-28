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

# and change directory to the monk-mariadb/mariadb-ha template folder
cd monk-mariadb/mariadb-ha
```

## Configuration

You can add/remove configuration of the template.

The current variables can be found in `mariadb-ha/variables` section

```yaml
  variables:
    mariadb_database_repl_user:
      value: "repl"
      type: string
    mariadb_database_repl_password:
      value: "RhgDPXPEnAgJ5q5g"
      type: string
    mariadb_database_monitor_user:
      value: "monitor"
      type: string
    mariadb_database_monitor_password:
      value: "RhgDPXPEnAgJ5q5g"
      type: string
    mariadb_database_user:
      value: "db_user"
      type: string
    mariadb_database_root_password:
      value: "RhgDPXPEnAgJ5q5g"
      type: string
    mariadb_database_name:
      value: "monk"
      type: string
    mariadb_master_database_port: 3306
    mariadb_slave1_database_port: 3307
    mariadb_slave2_database_port: 3308
    mariadb_database_password: "RhgDPXPEnAgxxXYZ"
    mariadb_image_tag:
      value: "10.10"
      type: string
    mariadb_database_data_volume_path:
      value: <- `${moncc-volume-path}/mariadb`
      type: string
    mariadb_database_data_volume_size: 60
```

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
 │  ├─🧩 monk-mariadb-master-slave/slave2
 │  ├─🧩 monk-mariadb-master-slave/configure
 │  ├─🧩 monk-mariadb-master-slave/master
 │  └─🧩 monk-mariadb-master-slave/slave1
 ├─🔗 Process groups:
 │  └─🧩 monk-mariadb-master-slave/stack
 └─⚙️ Entity instances:
    └─🧩 monk-mariadb-master-slave/stack/metadata
✔ All templates loaded successfully

➜  monk list monk-mariadb-master-slave

✔ Got the list
Type      Template                             Repository  Version  Tags
runnable  monk-mariadb-master-slave/configure  local       -        -
runnable  monk-mariadb-master-slave/master     local       -        -
runnable  monk-mariadb-master-slave/slave1     local       -        -
runnable  monk-mariadb-master-slave/slave2     local       -        -
group     monk-mariadb-master-slave/stack      local       -        self hosted, database



➜  monk run monk-mariadb-master-slave/stack

✔ Started local/mariadb/stack

```

## Logs & Shell

```bash
# show Mariadb master logs
➜  monk logs -l 1000 -f local/monk-mariadb-master-slave/master

# show Mariadb slave1 logs
➜  monk logs -l 1000 -f local/monk-mariadb-master-slave/slave1

# show Mariadb slave2 logs
➜  monk logs -l 1000 -f local/monk-mariadb-master-slave/slave2

```

## Stop, remove and clean up workloads and templates

```bash
➜ monk purge monk-mariadb-master-slave/stack 

```
