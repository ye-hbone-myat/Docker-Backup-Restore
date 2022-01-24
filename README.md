# Docker-Backup-Restore

Docker volumes backup and restore with docker command.

Docker Container Volume Backup for Nginx Proxy Manager Setup
----------

```console
docker run --rm --volumes-from npm-app-1 -v $(pwd):/backup busybox tar cvf /backup/npm_backup.tar /etc/letsencrypt /data
```
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
