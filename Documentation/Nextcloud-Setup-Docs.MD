# Nextcloud Setup Doc

## Nextcloud setup
1. clone the Unraid-Docker-Compose for keybase git to /mnt/user/appdata/nextcloud/
2. cd to /mnt/user/appdata/nextcloud/
3. execute `docker-compose up -d ` 



## Nextcloud backup for data
1. add script to Unraid via Custom Scripts Plugin
2. 
```bash
#!/bin/bash
# Output to a logfile
#exec &> /mnt/user/Backups/Logs/"nextcloud-$(date '+%Y-%m-%d').txt"
echo "Enabling nextcloud maintenance mode..."
docker exec nextcloud occ maintenance:mode --on
echo "Starting Nextcloud export..."
rsync -Aavx /mnt/user/appdata/nextcloud/ /mnt/user/Backups/temp/nextcloud/nextcloud-dirbkp_$(date '+%Y-%m-%d')/
echo "Export complete"
echo "Compressing backup..."
# Compress backed up folder
tar -zcf /mnt/user/Backups/nextcloud/"nextcloud-$(date '+%Y-%m-%d').tar.gz" /mnt/user/Backups/temp/nextcloud/nextcloud-dirbkp_* 
echo "Nextcloud backup successfully compressed to /mnt/user/Backups/nextcloud/"
# Remove uncompressed backup data
rm -rf /mnt/user/Backups/temp/nextcloud/*
echo "Removing backups older than 14 days..."
find ./mnt/user/Backups/nextcloud -mtime +14 -type f -delete
find ./mnt/user/Backups/Logs -mtime +14 -type f -delete
echo "Disabling nextcloud maintenance mode..."
docker exec nextcloud occ maintenance:mode --off
echo "Complete"

echo "Nextcloud backup completed successfully."
```
3. Make sure all directories in script are valid.
4. backup to externally mounted portable drive using Duplicati
5. Back to offsite location such as B2 etc.

## Backup Nextcloud DB
Coming SoonTM