```
# create snapshots: btrfs subvolume snapshot -r
7 * * * * sudo /mnt/.bkp/cron/hourly > /dev/null

# cleanup snapshots: btrfs subvolume delete
# backup snapshots: btrfs send | btrfs receive (this disables netatalk)
# backup remote: rsync
# report
13 0 * * * sudo /mnt/.bkp/cron/daily > /dev/null
```
