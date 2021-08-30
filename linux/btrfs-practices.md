# (Personal) Best practices of brtfs

## Create subvolume for `/tmp`, `/var/log`...

- Take a snapshot of `@` for `/tmp` calling it `@tmp`.

	  # btrfs subvolume snapshot / /.snapshots/@tmp

- Then delete everything in the `@tmp` subvolume, but `@tmp/tmp`.

	  # cd /.snapshots/@tmp
	  # rm -rf *

- Then move the content inside the `@tmp/tmp/*` to `@tmp/`, then remove `@tmp/tmp`. Because there was nothing at all, so we are good til now. If it's `/var/log` or something like that, you needa be more careful.

- Then modify the `/etc/fstab` to new mount point.

	  # /dev/mapper/arch-root LABEL=root
	  UUID=cc8b4f93-7135-4e6a-b0dc-ab2778afcb31	/         	btrfs     	rw,relatime,ssd,space_cache,subvolid=5,subvol=/	0 0

	  # @tmp subvolume
	  UUID=cc8b4f93-7135-4e6a-b0dc-ab2778afcb31	/tmp         	btrfs     	rw,relatime,ssd,space_cache,subvol=.snapshots/@tmp 0 0

	  # There are other lines in this file for other mount points.

- Reboot.
- Finally, delete the original content of `@/tmp` by mounting the `@` to `/mnt` then purge `/mnt/tmp`.

      # mkdir -p /mnt/rootbtr
      # mount -t btrfs -o subvol=/ /dev/mapper/arch-root /mnt/rootbtr
	  # cd /mnt/rootbtr/var/log
	  # rm -rf *
	  