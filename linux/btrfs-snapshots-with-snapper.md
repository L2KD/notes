## Btrfs snapshot

Show thông tin subvolume /

    # btrfs subvolume show /

    /
    Name: 			<FS_TREE>
    UUID: 			026fb3ba-d0ee-49c0-89c8-aa610f012a6e
    Parent UUID: 		-
    Received UUID: 		-
    Creation time: 		2019-11-14 15:37:26 +0700
    Subvolume ID: 		5
    Generation: 		476482
    Gen at creation: 	0
    Parent ID: 		0
    Top level ID: 		0
    Flags: 			-
    Snapshot(s):

Tạo mới 1 snapshot bằng vanilla btrfs subvolume

    # mkdir /.snapshots
    # btrfs subvolume snapshot / /.snapshots/20201012

    Create a snapshot of '/' in '/.snapshots/20201012'

    # btrfs subvolume show /
    /
    Name: 			<FS_TREE>
    UUID: 			026fb3ba-d0ee-49c0-89c8-aa610f012a6e
    Parent UUID: 		-
    Received UUID: 		-
    Creation time: 		2019-11-14 15:37:26 +0700
    Subvolume ID: 		5
    Generation: 		476467
    Gen at creation: 	0
    Parent ID: 		0
    Top level ID: 		0
    Flags: 			-
    Snapshot(s):
    			.snapshots/2020101

Khi này các file bên trong `/.snapshots` được thể hiện như một cây thư mục tương tự với `/`.

Để xóa 1 snapshot

    # btrfs subvolume delete /.snapshots/20201012
    Delete subvolume (no-commit): '/.snapshots/20201012'

## Snapper

https://wiki.archlinux.org/index.php/Snapper

Là một tool để quản lý các snapshots với LVM và Btrfs. Tool này được viết cho opensuse. Nên doc của nó ở đây: https://documentation.suse.com/sles/12-SP4/html/SLES-all/cha-snapper.html

Arch: Cài nó vào bằng `pacman`.

Lý do dùng: Vì nó có 1 số cấu hình thuận tiện hơn cho việc backup. Ví dụ như:

- Automatic timeline snapshots - tự động backup mỗi giờ, lưu lại 10 hourly backups. Mỗi ngày sẽ tự clean up. Giữ lại 10 daily, 10 monthly and 10 yearly. Có thể tắt nó bằng config.
- Bỏ qua 1 số directories để tránh việc phiền hà khi backup: `/var/log`, tránh việc mất log khi system crash + rollback, `/tmp`, `/boot`, `/home`...

Xóa snapshot 1 và 1101 
    
    snapper -c root delete 1 1101

Xóa một snapshot từ 1 tới 1101

    snapper -c root delete 1-1101

## Plugins

Hook snapshot khi system thực hiện các pacman transactions.

- `snap-pac`. This is a set of pacman hooks and script that causes snapper to auto‐matically take a pre and post snapshot before and after pacman transactions, similar to how YaST does with OpenSuse. This provides a simple way to undo changes to a system after a pacman transaction.

        Install it
        # cp /etc/snap-pac/root.conf.example /etc/snap-pac/root.conf

  Result after pacman transaction:

        # snapper -c root list
           # | Type   | Pre # | Date                            | User | Cleanup  | Description     | Userdata
        -----+--------+-------+---------------------------------+------+----------+-----------------+---------
        170  | pre    |       | Mon 19 Oct 2020 11:06:12 AM +07 | root | number   | pacman -S conky |
        171  | post   |   170 | Mon 19 Oct 2020 11:06:13 AM +07 | root | number   | conky           |

- Backup to external drive. Not tested yet.
