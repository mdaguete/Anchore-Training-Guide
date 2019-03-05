## Upgrading Anchore Engine

The anchore-engine is distributed as a [Docker Image](https://hub.docker.com/r/anchore/anchore-engine), which is comprised of smaller micro-services that can be deployed in a single container or scaled out to handle load.

The latest version of the anchore-engine image will be tagged with both the latest tag and a version number. For example latest and v0.2.4.

To retrieve the version of a running anchore-engine the system status command can be run.

```
# anchore-cli system status
...
...
...

Engine DB Version: 0.0.6
Engine Code Version: 0.2.3
```

In this example the anchore-engine is version 0.2.3 and the database schema is version 0.0.6.  In cases where the database schema is changed between releases of the anchore-engine, the engine will upgrade the database schema at launch.

### Pre-upgrade Procedure

Prior to upgrading anchore-engine, we highly recommend performing a database backup/snapshot by stopping your anchore-engine installation, and backing up the anchore engine database in its entirely.  There is no automatic downgrade capability in anchore-engine, thus the only way to downgrade after an upgrade (whether it succeeds or fails) is to restore your database contents to a state from a prior version of anchore-engine, and explicitly run the compatible version of anchore-engine against the corresponding database contents. 

Whether or not you wish to have the ability to downgrade, we recommend backing up your anchore-engine database prior to upgrading the software as a best practice.

### Upgrade Procedure (example with docker-compose)

1. Stop all running instances of the Anchore Engine

`docker-compose down`

2. Modify your docker-compose.yaml file to reference the desired version of anchore-engine (or if set to 'latest', your can skip this step).

```YAML
...
...
services:
  anchore-engine:
    #image: docker.io/anchore/anchore-engine:v0.2.3
    image: docker.io/anchore/anchore-engine:v0.2.4
...
...
```

3. Download the new configured version of the anchore-engine

`# docker-compose pull`

4. Restart the Anchore Engine containers

`# docker-compose up -d`

To monitor the progress of your upgrade, you can watch the docker logs from your anchore-engine container, where you should see some initial output indicating whether or not an upgrade is needed or being performed, followed by the regular anchore-engine log output.

`# docker-compose logs -f anchore-engine`

Once completed, you can review the new state of your engine to verify the new version is running using the regular system status command.

```
# anchore-cli system status
...
...
...

Engine DB Version: 0.0.7
Engine Code Version: 0.2.4
```

### Advanced / Manual Upgrade Procedure

If for any reason the automated upgrade fails, or you would like to perform the upgrade of the anchore database manually, you can use the following (general) procedure.  This should only be done by advanced operators after backing up the anchore database, ensuring that the anchore database is up and running, and that all running anchore-engine components are stopped.

- Install the desired anchore-engine container manually
- Run the anchore-engine container but override the entrypoint to run an interactive shell instead of the default 'anchore-manager service start' entrypoint command
- Manually execute the database upgrade command, using the appropriate db_connect string 
```
# anchore-manager db --db-connect <db_connect_string> upgrade
[MainThread] [anchore_manager.cli.utils/connect_database()] [INFO] DB params: {"db_connect_args": {"timeout": 86400, "ssl": false}, "db_pool_size": 30, "db_pool_max_overflow": 100}
[MainThread] [anchore_manager.cli.utils/connect_database()] [INFO] DB connection configured: True
[MainThread] [anchore_manager.cli.utils/connect_database()] [INFO] DB attempting to connect...
[MainThread] [anchore_manager.cli.utils/connect_database()] [INFO] DB connected: True
...
...
```
- The output will indicate whether or not a DB upgrade is needed, prompt for confirmation if it is, and will display upgrade progress output before completing.
