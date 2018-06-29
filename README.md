```
# create snapshots: btrfs subvolume snapshot -r
7 * * * * sudo /mnt/.bkp/cron/snapshot-create

# cleanup snapshots: btrfs subvolume delete
13 * * * * sudo /mnt/.bkp/cron/snapshot-cleanup

# backup snapshots: btrfs send | btrfs receive (this disables netatalk)
# backup remote: rsync
23 0 * * * sudo /mnt/.bkp/cron/backups

# report
42 5 * * * sudo /mnt/.bkp/cron/report
```
