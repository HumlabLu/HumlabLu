
# External backup

Before going live, you want to set up an external backup. Here is one way to do it.

The dockerfile points the db folder to `db\live`. In another folder, `db\backup` you can stow away live data locally. Your external backup system (slower) does then not have to read live data, but always read from the `db\backup` folder.

1. Create a script `zip_and_clean.sh` to copy from the live folder into  `<yourfolder>/db/backup`,  eg like this, with rolling removal on 10 days:
```
sudo tar -zcvf "/home/<yourfolder>/db/backup/db-$(date +"%Y-%m-%d %H%M%S").tar.gz" /<yourfolder>/db/live/

find '<yourfolder>/db/backup/' -name 'db-*.tar.gz' -mtime +10 -type f -delete
```
3. Create a cron job to run the script, eg like this, every morning at 00:30 AM.
```
30 0 * * * /home/<yourfolder>/db/backup/zip_and_clean.sh >  /home/<yourfolder>/db/backup/cron.log
```
4. Point your external backup system to `<yourfolder>/db/backup` 
5. Schedule your (nightly) external backup to 30 min after your cron job.
