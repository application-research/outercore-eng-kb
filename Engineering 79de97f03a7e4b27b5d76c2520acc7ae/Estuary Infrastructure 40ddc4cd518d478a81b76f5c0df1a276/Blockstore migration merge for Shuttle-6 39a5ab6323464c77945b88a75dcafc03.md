# Blockstore migration / merge for Shuttle-6

# Overview

We need to merge the two blockstores on shuttle-6 using Elija’s scripts.

## Steps

### log on to shuttle-6

### go to `/mnt/blockstore`

```html
root@shuttle-6-sg-s3-xlarge-x86-01:/mnt/blockstore# ls -lrt
total 716
drwsrwsrwx 11127 estuary estuary 536576 Sep  2 23:13 flatfs_bu
drwsrwsrwx     6 root    root       109 Sep  2 23:13 estuary_bu
drwsrwsrwx     2 estuary estuary    313 Oct 16 13:21 tmpdir
drwxr-sr-x     7 root    estuary    126 Oct 17 16:01 estuary
drwxr-sr-x     8 root    estuary    259 Oct 17 16:06 bsutil
drwxr-sr-x     3 root    estuary     67 Oct 17 16:06 flatfs
drwxr-sr-x     3 root    estuary    112 Oct 17 16:15 migrate-lmdb-flatfs
drwxr-sr-x     2 root    estuary     50 Oct 18 05:18 flatfs_bu_for_1
```

1. the current shuttle uses `estuary` and `flatfs`
2. there was an old blockstore called `flatfs_bu` and `estuary_bu`
3. we need to merge the `estuary` and `estuary_bu` , AND `flatfs` and `flatfs_bu`
4. I tried doing it first with `flatfs` but I got some errors. See `migrate-lmdb-flatfs` and `flatfs_bu_for_1`
5. Feel free to start from scratch. I haven’t migrated nor merged anything yet.
6. To test, you need restart shuttle-6 (this will stop shuttle-6). Let me know if you need to this, we need to disable shuttle-6 on the viewer first before restarting.
    1. `systemctl stop estuary-shuttle.service`
    2. the service above points to the `/mnt/blockstore/estuary` and `/mnt/blockstore/flatfs`
    3. use `mv` to rename the folders
    

Elijah’s shuttle-6 process

1. created enough space for any blockstore that has to be migrated + the sum of all the blockstore to be merged
2. ran `migrate-lmdb-flatfs` on `flatfs` (which was in LMDB format despite its name)
3. w.i.p.