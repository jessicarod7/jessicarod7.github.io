---
title: "How to encrypt an existing disk on Fedora without formatting"
date: 2025-05-16
last_modified_at: 2026-06-24
excerpt: "Make a backup first!"
---

_This is derived from [an existing answer](https://unix.stackexchange.com/a/710373/533888) that I posted on the Unix & 
Linux Stack Exchange. I'm bringing it to my personal blog for preservation purposes (and honestly, so I can refer back
to it when I inevitably need to set up a new laptop).
**[This post]({% post_url 2025-05-16-encrypt-existing-disk-fedora %}) by Jessica Rodriguez is licensed under [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/).**_

_I originally wrote this guide for Fedora 36, and have made a couple adjustments with better sources for Fedora 44._

## Assumptions

This assumes a default Fedora installation, with the following Btrfs-based partitions:

- Root partition (Btrfs subvolumes "root" [mounted at `/`] and "home" [mounted at `/home`])
- Boot partition (mounted at `/boot`)
- EFI partition (_UEFI systems only_, mounted at `/boot/efi`)

## Requirements

- A full-disk backup
- [cryptsetup](https://gitlab.com/cryptsetup/cryptsetup) (should be included, otherwise install with
  `dnf install cryptsetup`)
- At least 100 MiB of free space
- A rescue system that can unmount the root filesystem
  (ex. [Fedora live USB](https://docs.fedoraproject.org/en-US/quick-docs/creating-and-using-a-live-installation-image/))
- _NOTE:_ The encryption screen will use the keyboard layout defined in `/etc/vconsole.conf` (set with `localectl`). The
  layout cannot be changed at boot time.

## Instructions

1. Identify the root filesystem with `lsblk -f`. Store the UUID (format `XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX`) for
   later use.
2. Identify your current kernel version with `uname -r`, and save this value for later.
3. Reboot into the rescue system. Locate the root filesystem with `blkid --uuid <UUID>`, and run a check on the
   filesystem with `btrfs check <device>`
4. Mount the filesystem with `mount /dev/<device> /mnt`
5. Shrink the filesystem to make room for the LUKS header. At least 32 MiB is recommended, use
   `btrfs filesystem resize -32M /mnt`
6. Unmount the filesystem: `umount /mnt`
7. Encrypt the partition with `cryptsetup reencrypt --encrypt --reduce-device size 32M /dev/<device>`, providing a
   passphrase when prompted.
8. Identify the encrypted LUKS partition with `lsblk -f` (note that the UUID has changed). Save this LUKS partition UUID
   for later use.
9. Open the partition, providing your passphrase when prompted: `cryptsetup open /dev/<device> system`
10. Mount the mapped filesystem with `mount /dev/mapper/system /mnt`
11. Resize the filesystem to use all the space: `btrfs filesystem resize max /mnt`, then unmount the filesystem with
    `umount /mnt`
12. Mount the _**root subvolume**_ (the Linux filesystem root) with
    `mount -o subvol=root /dev/mapper/system /mnt`
13. Identify the devices for the boot and EFI partitions with `lsblk`. Mount the boot filesystem
    (`mount /dev/<boot device> /mnt/boot`), followed by the EFI filesystem for UEFI systems
    (`mount /dev/<EFI device> /mnt/boot/efi`), and then the EFI vars directory (`sudo mount -o bind /sys/firmware/efi/efivars /mnt/sys/firmware/efi/efivars`.
14. Bind-mount the pseudo filesystems `/dev`, `/dev/pts`, `/proc`, `/run`, and `/sys`, in the format of
    `mount -o bind /sys /mnt/sys`
15. Open a shell within the filesystem: `chroot /mnt /bin/bash`
16. Open `/etc/default/grub` with a text editor, and modify the kernel parameters to identify the LUKS partition, and
    temporarily disable SELinux enforcing. Add these parameters, then save the changes and close the file:
    ```
    GRUB_CMDLINE_LINUX="[other params] rd.luks.uuid=<LUKS partition UUID> enforcing=0"
    ```
17. Configure a relabelling of SELinux with `fixfiles -F onboot` (if the output isn't as expected, run `touch /.autorelabel` instead)
18. Regenerate the GRUB config: `grub2-mkconfig -o /boot/grub2/grub.cfg`
19. Regenerate initramfs to ensure cryptsetup is enabled:
    `dracut -f /boot/initramfs-<kernel version>.img <kernel version>`
20. Exit the chroot
21. Unmount all filesystems in reverse order. (For filesystems mounted with `-o bind`, the option `-l` can be used, but this may break the live session. It probably isn't needed.)
    Close the LUKS partition with `cryptsetup close system`
22. Reboot and log into the regular system. You'll be asked for your passphrase to decrypt the system during boot.
23. Open `/etc/default/grub` in a text editor, and reenable SELinux enforcing by removing `enforcing=0` from
    `GRUB_CMDLINE_LINUX`. Save and exit.
24. Relabel SELinux again with `fixfiles -F onboot`.
25. **Repeat step 18** to regenerate the GRUB config.
26. Reboot and log into the system.

This answer heavily derives from _maxschelpzig_'s [answer](https://unix.stackexchange.com/a/584275/533888) and
[the Arch wiki](https://wiki.archlinux.org/title/dm-crypt/Device_encryption#Encrypt_an_existing_unencrypted_file_system).
It also pulls from the [Fedora documentation on GRUB](https://docs.fedoraproject.org/en-US/quick-docs/grub2-bootloader/)
and _ceremcem's_ [answer](https://unix.stackexchange.com/a/558623/533888).