# Docker-Backup-Restore

Docker volumes backup and restore with docker command.

Docker Container Volume Backup for Nginx Proxy Manager Setup
----------

```console
docker run --rm --volumes-from npm-app-1 -v $(pwd):/backup busybox tar cvf /backup/npm_backup.tar /etc/letsencrypt /data
```
You will get backup tar file in current directory save to somewhere safe.

Explanation
-------
1. --rm ==> Remove the running temp busybox container after runnging.
2. --volumes-from ==> The docker volume of the running containers
3. npm-app-1 ==> The docker container id which you want to backup.
4. $(pwd) ==> The current directory
5. busybox ==> The tempory container to archive the container data
6. tar ==> The archiving tool.
7. cvg ==> compressing option for tar.
8. npm_backup.tar ==> Backup file name(You can change whatever you like)
9. /etc/letsencrypt /data ==> Directories which need to be backup from the container. You can check docker compose file for those directories for volume.(Note: This will be change according to your docker compose setup)

Docker Container Volume Restore for Nginx Proxy Manager Setup
-----
1. Extract the backup file.
```console 
tar -xvf npm_backup.tar
```
2. Change mount point in docker compose file to backup folder directory
```console
 volumes:
      - ./data:/data
      - ./etc/letsencrypt:/etc/letsencrypt
```
3. Run the docker-compose setup.
```console
docker compose up -d
```

Docker Container Volume Backup for Pgadmin Setup
-------
```console
docker run --rm --volumes-from pgadmin-postgres-1 -v $(pwd):/backup busybox tar cvf /backup/db-data_backup.tar /var/lib/postgresql/data && docker run --rm --volumes-from pgadmin-pgadmin-1 -v $(pwd):/backup busybox tar cvf /backup/pgadmin_backup.tar /var/lib/pgadmin
```
You will get backup tar files in current directory and save to somewhere safe.


Docker Container Volume Restore for Pgadmin Setup
-----
1. Extract the backup file.
```console 
tar xvf db-data_backup.tar && tar xvf pgadmin_backup.tar
```
2. Change mount point in docker compose file to backup folder directory.

For postgre data:

```console
volumes:
            - ./var/lib/postgresql/data:/var/lib/postgresql/data
```

For pgadmin data:

```console
volumes:
            - ./var/lib/pgadmin:/var/lib/pgadmin
```

3. Run the docker-compose setup.
```console
docker compose up -d
```
