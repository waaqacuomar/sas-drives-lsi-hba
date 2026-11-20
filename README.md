# Adding Used Data-Center Drives with an LSI HBA

Got two used 2TB enterprise SAS drives working in a consumer PC running Proxmox.

## Hardware
- LSI 9300-8i HBA (IT mode) in a spare PCIe slot
- SFF-8643 to SFF-8482 breakout cable (SAS drives won't plug into normal SATA cables)
- 2x Seagate 2TB SAS drives pulled from data centers

## Troubleshooting
- Seller shipped the wrong cables; got a $20 refund and sourced the right breakout
- Drives spun up but never appeared in `lsblk`
- Confirmed the card loaded with `lspci` and `dmesg`, then checked each port's link with `negotiated_linkrate` (all Unknown)
- Tested a known-good SSD on the same cable to isolate the fault
- Moved the card to another node, where the drives showed up; the real cause was a cable that wasn't fully seated

## Result
- Both drives detected and set up as two separate ZFS pools in the Proxmox UI: one for storage, one for backups
- Used permanent `/dev/disk/by-id` names instead of sdX names, which can change between boots

## Screenshots
![ZFS storage in Proxmox](images/proxmox-storage.png)
<!-- add: ![lsblk output](images/lsblk.png) -->
