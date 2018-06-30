# btrfs backup cron toolkit

These scripts are explicitly developed for my personal NAS setup (running Ubuntu 18.04 LTS). Feel free to fork & adapt, but please to not expect me to change anything for your setup (I'm happy to help tho, also feedback is highly appreciated)

### `crontab`

Add this to `crontab`, both crons are independent, `daily` also runs `hourly` scripts (which is the reason why `hourly` only runs `1-23`)

```
# create snapshots: btrfs subvolume snapshot -r
13 1-23 * * * sudo /mnt/.bkp/cron/hourly > /dev/null

# create snapshots: btrfs subvolume snapshot -r
# cleanup snapshots: btrfs subvolume delete
# backup snapshots: btrfs send | btrfs receive (this disables netatalk)
# backup remote: rsync
# report
13 0 * * * sudo /mnt/.bkp/cron/daily > /dev/null
```
