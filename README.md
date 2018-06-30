# btrfs backup cron toolkit

These scripts are explicitly made for my personal NAS setup.

Feel free to fork & adapt, but please to not expect me to change anything for your setup (I'm happy to help tho, also feedback is highly appreciated)

## Our Setup

We are running two (almost) identical Ubuntu 18.04 LTS systems:

- "Datenteich" is at my place
- "Speichersee" is at my parents place.

Both NAS have two data drives:

- 24/7 drive (`/mnt/data.disk`) and
- backup disk (`/mnt/backup.disk`).

We use [Resilio](https://www.resilio.com/individuals/) to sync important and/or actively used data between our machines (mainly MacBooks, but also Windows systems and iPhones) and store everything else directly on the NAS systems.

This toolkit helps backing up the data so that we can follow the 3-2-1 rule. Most data is actually stored on 5+ systems :)

## Todos

- Add "Aussenbecken" to the setup

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
