# cron based backup toolkit for btrfs filesystem

These scripts are explicitly made for my personal NAS setup, also most of them are kinda hacky - use with care :)

Feel free to fork & adapt, but please to not expect me to change anything for your setup (I'm happy to help tho, also feedback is highly appreciated)

## Our Setup

We are running two (almost) identical servers plus one small NAS:

- "Datenteich" at our place (running Ubuntu 18.04 LTS / i3-8100 / 32GB / H370)
- "Speichersee" at my parents place (running Ubuntu 18.04 LTS / G4400 / 16GB / H110)
- "Aussenbecken" as external offsite backup (running Synology DSM 6 / DS214+)

Both Ubuntu NAS have two data disks (24/7 (`/mnt/data.disk`) + Backup (`/mnt/backups.disk`) and Aussenbecken is running two disks in RAID1 mode.

We use [Resilio](https://www.resilio.com/individuals/) to sync important and/or actively used data between our machines (mainly MacBooks, but also Windows systems and iPhones) and store everything else directly on the NAS systems, they also act as "Cloud" for Resilio (which is P2P).

This toolkit helps backing up the data so that we can follow the 3-2-1 rule (its _way_ more in our case…).

## Data Flow

1.  Our machines sync data between each other and all three servers using Resilio
    1.  Snapshot is created every our on the same disk and stored with Time Machine like strategy
    2.  Snapshot history gets synced to backup disk every night on "Datenteich"
2.  Data is stored in "NAS" folder on "Datenteich" and "Speichersee"
    1.  Snapshot is created every our on the same disk and stored with Time Machine like strategy
    2.  Snapshot history gets synced to backup disk every night on "Datenteich" and "Speichersee"
    3.  Most recent Snapshot content is rsynced to the other servers backup disk ("Datenteich" -> "Speichersee" & "Speichersee" -> Datenteich")
        1.  deleted files are stored in dedicated backup folder for 30 days.
    4.  Most recent Snapshot content is rsynced to "Aussenbecken"
        1.  deleted files are stored in dedicated backup folder for 30 days.
3.  MacBooks backing up to "home servers" backup disk ("Datenteich" or "Speichersee")

Call me paraniod, thats fine 🤷‍♂️ Also 🍪 for everyone spotting the weak point (there is one).

### `crontab`

Both crons are independent, `daily` also runs `hourly` scripts

```
# create snapshots: btrfs subvolume snapshot -r
13 * * * * sudo /mnt/.bkp/cron/hourly > /dev/null

# create snapshots: btrfs subvolume snapshot -r
# cleanup snapshots: btrfs subvolume delete
# backup snapshots: btrfs send | btrfs receive (this disables netatalk)
# backup remote: rsync
# report
13 0 * * * sudo /mnt/.bkp/cron/daily > /dev/null
```
