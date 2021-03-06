---
layout: default
title: Relax-and-Recover Release Notes
---

## Release Notes for Relax-and-Recover version 2.1

This document contains the release notes for the open source project
Relax-and-Recover.

[Relax-and-Recover website](http://relax-and-recover.org/)

[Github project](https://github.com/rear/)


This document is distributed with the following license: "Creative Commons
Attribution-NoDerivs 3.0 Unported (CC BY-ND 3.0)". To read the license deed
go to [http://creativecommons.org/licenses/by-nd/3.0/](http://creativecommons.org/licenses/by-nd/3.0/)


## Overview
Relax-and-Recover is a GNU/Linux system administrator tool used to
create disaster recovery images which makes bare metal restore easier.
System administrators use Relax-and-Recover as part of disaster recovery
policy which does not replace in any way a good backup policy.


### Product Features
The following features are supported on the most recent releases of
Relax-and-Recover.  Anything labeled as (*New*) was added as the
most recent release. New functionality for previous releases can be
seen in the next chapter that details each release.

The most recent release of Relax-and-Recover is supported on most GNU/Linux
based systems with kernel 2.6 or higher. It provides the following
functionality:

* Hot maintenance capability. A rescue image can be made online while
  the system is running.

* Command line interface. Relax-and-Recover does not require a graphical
  interface to run, nor in creation mode, nor in rescue mode (console
  is enough).

* Support included for most common file systems, such as ext2, ext3, ext4
  and reiserfs. Other filesystems like jfs, xfs and btrfs are also
  implemented, but are less tested. _(Feedback is appreciated)_

* Selected Hardware RAID and (eg. HP SmartArray) and mirroring solutions (eg.
  DRBD) are supported.

* NVME and mmcblk disks are supported (*New*)

* LVM root volumes are supported.

* Multipath support for SAN storage

* UEFI support (including UEFI USB booting

* Integrates with _internal_ backup solutions such as:

   - GNU tar (BACKUP=NETFS, BACKUP_PROG=tar)
   - GNU tar (BACKUP=NETFS, BACKUP_PROG=tar, BACKUP_TYPE=incremental, FULLBACKUPDAY="Mon") for using incremental backups with a weekly full backup. Be aware, old tar archives will not be removed automatically! (*New*)
   - GNU tar (BACKUP=NETFS, BACKUP_PROG=tar, BACKUP_TYPE=differential, FULLBACKUPDAY="Mon") for using differential backups with a weekly full backup. Be aware, old tar archives will not be removed automatically! (*Updated*)
   - GNU tar with openssl encryption (BACKUP=NETFS, BACKUP_PROG=tar, BACKUP_PROG_CRYPT_ENABLED=1)
   - rsync on local devices (BACKUP=NETFS, BACKUP_PROG=rsync), such USB and local disks
   - rsync over the network (BACKUP=RSYNC, BACKUP_PROG=rsync)
   - Multiple backup methods ([read the documentation](https://github.com/rear/rear/blob/master/doc/user-guide/11-multiple-backups.adoc)) (*New*)
   - Windows partitions via BACKUP=BLOCKCLONE (*New*) See [the documention about BLOCKCLONE](https://github.com/rear/rear/blob/master/doc/user-guide/12-BLOCKCLONE.adoc)
   - BACKUP=ZYPPER is SLES12 only (*Experimental*)

* Integrates with _external_ backup solutions such as:

  - Tivoli Storage Manager (BACKUP=TSM)
  - HP Data Protector (BACKUP=DP)
  - Symantec NetBacakup (BACKUP=NBU)
  - Galaxy 5, 6, and 7 (BACKUP=GALAXY)
  - Galaxy 10 [Commvault Simpana] (BACKUP=GALAXY10)
  - Bacula (BACKUP=BACULA)
  - Bareos (BACKUP=BAREOS) (A fork of Bacula)
  - Rsync Backup Made Easy (BACKUP=RBME)
  - Duplicity/Duply (BACKUP=DUPLICITY)
  - EMC Networker, also known as Legato (BACKUP=NSR)
  - SEP Sesam (BACKUP=SESAM)
  - FDR/Upstream (BACKUP=FDRUPSTREAM)
  - Novastor NovaBACKUP DC (BACKUP=NBKDC)
  - Borg Backup (BACKUP=BORG)

* Integrates with [Disaster Recovery Linux Manager (DRLM)](http://drlm.org)

* Udev support (except for some really ancient udev versions) which is
  the base for many new and important features:

  - kernel drivers for network cards and storage adapters are loaded via udev
  - deal with network persistent names in udev rules
  - firmware loading
  - persistent storage device names (though Relax-and-Recover does nothing with this)

* System reconfiguration

  - enable recovery on hardware, that is not the same as the original system
  - network and storage drivers are adjusted
  - map hard disks if they do not match (e.g. hda -> sda)
  - remap network MAC addresses
  - use another IP address, or using dhcp via templates or from kernel command line
  - rebuild the initial ramdisk if needed (for new storage drivers)
  - migration to SAN storage (*Experimental*)

* Support backup software: Bacula, both locally attached tapes (with
  bextract) and network-based backups. Also, in combination with OBDR tapes.

* Create OBDR tapes with method `mkbackup` and put the backup onto the tape
  to have a single-tape bootable recovery solution

* Label the OBDR tape with the method `format` to avoid accidental
  overwrites with OBDR

* Create bootable disk (eSATA, USB ...) media with the backup included:

    BACKUP_URL=usb:///dev/device

    Together with `OUTPUT=USB` we have now a complete solution on hard disks
    (booting of it and restoring data).

* DHCP client support (IPv4 and IPv6). Dhcp client activation
  can be forced via the variable *USE_DHCLIENT=yes* (define in _/etc/rear/local.conf_).
  It is also possible to force DHCP at boot time with kernel option `dhcp`

* `USE_STATIC_NETWORKING=y`, will cause statically configured network settings to be applied even when `USE_DHCLIENT` is in effect

* Save layout and compare layouts for easy automation of making
  Relax-and-Recover snapshots (checklayout option)

* The *layout* workflow is the default workflow instead of the
  deprecated *dr* workflow. The *dr* workflow kept all important
  system information into a directory structure where the new *layout*
  workflow use files to keep the information centralized. The *dr* workflow
  has been removed in ReaR v1.14!

* External USB booting now uses extlinux instead of syslinux, and
  therefore, the USB disk must first be formatted with an ext2, ext3, ext4
  or btrfs based file system

* cron job to check changes in disk layout and trigger `rear mkrescue` if required

* VLAN tagging support

* Add timestamp of ReaR run with rc code to the syslog or messages file; sending mail report is also possible

* The possibility to backup Windows partitions via new BACKUP type BLOCKCLONE (*New*)

*NOTE*: Features marked *Experimental* are prone to change with future releases.



## Relax-and-Recover Releases
The first release of Relax-and-Recover, version 1.0, was posted to the web
in July 2006. For each release, this chapter lists the new features and
defect fixes. Note that all releases are cumulative, and that all releases
of Relax-and-Recover are compatible with previous versions of
Relax-and-Recover, unless otherwise noted.

The references pointing to *fix #nr* or *issue #nr* refer to our [issues tracker](https://github.com/rear/rear/issues).

### Version 2.1 (June 2017)

* Support for Grub2 installation with software RAID1 on Linux on POWER (ppc64/ppc64le) (issue #1369)

* REBUILD_INITRAMFS variable was introduced. The new default.conf setting REBUILD_INITRAMFS="yes" rebuilds the initramfs/initrd during "rear recover" to be more on the safe side. With REBUILD_INITRAMFS="" the old behaviour can still be specified (issue #1321)

* ISO_RECOVER_MODE=unattended mode (issue #1351) - required for automated ReaR testing with OUTPUT=ISO

* MODULES variable supports now special values like 'all_modules', 'loaded_modules', 'no_modules' (issues #1202, #1355)

* Include systemd/network to preserve "Predictable Network Interface Names" (issue #1349)

* Various improvements regarding multipath (issues #1190, #1309, #1310, #1311, #1314, #1315, #1324, #1325, #1328, #1329, #1344, #1346)

* Show OUTPUT variables in `rear dump` (issue #1337)

* Added support for "grub PXE style" via PXE_CONFIG_GRUB_STYLE and PXE_TFTP_IP on non x86 platform (issue #1339)

* Try 'wipefs --force' and use 'dd' as fallback to better clean up disk partitions (issue #1327)

* Reorganized "finalize" scripts ordering and cleanup of the PPC bootloader installation (issue #1323)

* Avoid long default wait in 'dig' when DNS servers are not set (issue #1319)

* Fail-safe calculations in partitioning code (issues #1269, #1307)

* Improved support on ppc/ppc64/ppc64le architectures (issues #1178, #1311, #1313, #1322)

* Define hostname in both /etc/HOSTNAME and /etc/hostname in rescue image (for Arch) (issue #1316)

* Rename network interface when MAC not present in udev (issue #1312)

* Added support for 'nano' editor (in addition to 'vi') (issues #1298, #1306)

* mmcblk disk types are now supported (issues #1301, #1302)

* NETFS_RESTORE_CAPABILITIES variable was introduced to restore file capabilities in a proper way (issue #1283)

* Added required libs and files for 'curl' with HTTPs by default (issues #1267, #1279)

* More precise XFS file system creation during `rear recover` (issues #1208, #1213, #1276)

* DRLM management and security improvements (issue #1252)

* Improved BOOTLOADER support (issue #1242)

* DRLM support for multiple backups via multiple config files (issue #1229)

* FIRMWARE_FILES support to exclude firmware files in rescue image to reduce the size of image (issue #1216)

* Enable SELinux in the rescue image for tar internal backup method if BACKUP_SELINUX_DISABLE=0 (issue #1215)

* BOOT_OVER_SAN is now fully supported (issues #1190, #1309, #1314, #1315, #1325, #1329, #1344)

* NVME disks are now fully supported (issue #1191)

* Some initial basic support for new backup type ZYPPER was added (issues #1085, #1209)

* Finding UEFI boot loaders on non standard places (issues #1204, #1225, #1293)

* The USB UEFI partition size USB_UEFI_PART_SIZE for kernel image has been increased from 100 to 200 MB (issue #1205)

* REAR_INITRD_COMPRESSION variable was introduced to specify initrd compression (e.g. 'lzma' for PPC64) (issues #1142, #1218, #1290)

* New backup type BLOCKCLONE was added to backup non-Linux partitions (e.g. Windows NTFS partitions) (issues #1078, #1162, #1172, #1180)

* Bareos 16.2 is now supported (issue #1169)

* New USB_PARTITION_ALIGN_BLOCK_SIZE and USB_DEVICE_FILESYSTEM_PARAMS variables were added (issue #1217)

* Improved the USB backup selection menu during the recovery via USB (issue #1166)

* USB_SUFFIX variable was introduced to align backup on USB with backup on NFS (issues #1164, #1160, #1145)

* Forbid incremental backup to work on BACKUP_URL=usb:// (issue #1146)

* The USB_DEVICE_PARTED_LABEL=gpt setting is now honered while formatting the USB disk (issue #1153)

### Version 2.00 (January 2017)

(*Important Note*) ReaR 2.00 introduced the 3-digits scripts instead of the 2-digits script. This means all scripts must begin with 3 digits, e.g. 010-my-script.sh instead of 10-my-script. Therefore, if you wrote your own scripts make sure to renumber these. You could also use the *make validate* to check this.

* Bareos support: add missing directory /var/run/bareos in recovery system (issue #1148)

* Forbid BACKUP_URL=usb for BACKUP_TYPE=incremental/differential (issues #1141 and #1145)

* Improved and added new example configurations (issue #1068, #1058)

* Modified/Improved the exit code messages of ReaR (issues #1089, #1133)

* Fix documentation regarding OUTPUT_URL=null (issues #734, #1130)

* Better and fail safe progress messages while tar backup restore (issue #1116)

* Implement simulation mode with simulation with the workflows validate and shell (issue #1098)

* Update 11-multiple-backups.adoc : Multiple backups are in general not supported for
BACKUP_TYPE=incremental or BACKUP_TYPE=differential (issues #1074 and #1123)

* Using RUNTIME_LOGFILE in all scripts as needed (issue #1119)

* New Backup method was added - BORG (issues #1030, #1037, #1046, #1048, #1118)

* Multiple backups are now possible (issues #1088, #1102, #1096) - see the [documentation page](https://github.com/rear/rear/blob/master/doc/user-guide/11-multiple-backups.adoc) (*New*)

* Support partitioning and formatting huge USB devices (issue #1105)

* Skip remount async when systemd is used (issue #1097)

* Fixed and enhanced code for multiple ISOs (issue #1081)

* BACKUP_TYPE=incremental (*New*) and BACKUP_TYPE=differential were updated (issues #974, #1069)

* Added support for setting a UUID on XFS with enabled CRC (RHEL 7) (issue #1065)

* Fix for ISO not bootable for SLES11 ppc64 with root LVM (issue #1061)

* PXE booting enhancement with new style of uploading the boot files (issue #193)

* Renumbering the ReaR scripts from 2-digits to 3-digits (issue #1051)

* Improved boot loader detection (issue #1038)

### Version 1.19.0 (October  2016)

* Save bootloader info from POWER architecture and rebuild initrd after migration (issues #1029, #1031)

* Improved documentation and man page in general (issues #918, #930, #1004, #1007, #1008)

* New SLE12-SP2-btrfs-example.conf file because since SLES12-SP2 btrfs quota setup for snapper via "snapper setup-quota" is needed (issue #999)

* Simplified reboot halt poweroff and shutdown in the rescue/recovery system in case of systemd (issue #953)

* If TSM parameters contain a dot, the dot is replaced by an underscore in the TSM_SYS variable names (issues #985 and #986)

* Check if /dev/disk/by-label/RELAXRECOVER exist (issues #979 and #326)

* Added PRE_BACKUP_SCRIPT and POST_BACKUP_SCRIPT to be able to do custom tasks in the mkbackup/mkbackuponly workflows (issue #977)

* Make TMPDIR work in compliance with Unix standards (issue #969)

* USE_STATIC_NETWORKING now really overrides USE_DHCLIENT (issue #964)

* Make it safe against wrong btrfs subvolumes on SLES12 (issues #963, #966)

* Encrypted incremental backup cannot read the tar label (issue #952)

* Introduction of the NETWORKING_PREPARATION_COMMANDS variable to prepare network setup in the rescue/recovery system (issue #960)

* After migration fs_uuid for root partition was not changed in ELILO config file /etc/elilo.conf (issue #956)

* Clarified rear man page and default.conf file around BACKUP_URL=rsync: (issues #930 and #918)

* Make "rear recover" work with default btrfs on SLES12-SP2 (issue #944)

* Dropped GRUB_SUPERUSER and GRUB_RESCUE_PASSWORD to avoid that GRUB_RESCUE could change the behaviour of the GRUB2 bootloader in the currently running system in unexpected ways. With the new optional GRUB_RESCUE_USER setting GRUB_RESCUE works in compliance with the existing GRUB2 configuration (issues #938, #942)

* Bail out if not enough disk space for GRUB and GRUB2 rescue image (issue #913)

* Use BACKUP_PROG_COMPRESS_OPTIONS as an array so that one can use it to provide more complex values (issue #904)

* Add /usr/lib/syslinux/bios to the search path for mbr.bin (issue #908)

* Always load modules in /etc/modules (issue #905)

* Ask user for EFI partition size on USB disk (issue #849)

* Insure /etc/rear/mappings directory exists before doing a recovery (issue #861)

* First steps for rescue/recovery system update support via RECOVERY_UPDATE_URL (issue #841)

* NFS mount points are not recreated after a recover (issue #818)

* Correcting ReaR return code handling in auto recover mode (issue #893)

* Added NFSv4 support for security 'sys' only so far (issue #754)

* Changed the usage of 'rpcinfo -p' a bit to have the same outcome of different Linux flavours (issue #889)

* RSYNC: /boot/efi needs --relative rsync option (issue #871)

* New variables added for Bareos: BAREOS_RESTORE_JOB and BAREOS_FILESET

* Multipath partition not found in rhel7.2 (issue #875)

* Adding support for ppc64le PowerNV (non-virtualized aka Bare-Metal) (issue #863)

* First steps to support new ftpfs BACKUP_URL scheme (issue #845)

* Clean up 'url_host()' (issue #856)

* Fix that libaio (needed for multipath) could be missing in rescue/recovery system because libaio can be located in different directories (issue #852)

* Improved the Relax-and-Recover menu for GRUB2 (issues #844, #849, #850)

* Check for valid BACKUP_URL schemes (issue #842)

* USB UEFI boot support (issue #831)

* Mitigate the problem that btrfs subvolums are not restored by default via TSM (issue #833)

* Determine EFI virtual disk size automatically (issue #816)

* ebiso image size is too small if BACKUP=TSM (issue #811)

* Improving the logics around ebiso usage in UEFI mode (issue #801)

* Fix for wrong UUID in initrd for bootfs (issues #649 and #851)



### Version 1.18.0 (March 2016)

* Support was added for NVME SSD type of disk devices (issue #787)

* For LUKS added the password libraries (issue #679)

* Script 99_sysreqs.sh was added to save the minimal system requirements necessary for cloning
  a system in a remote DRP data center (issue #798)

* New 99_move_away_restored_files.sh to remove restored files after recover (issue #799)
  New array was introduced to make this - BACKUP_RESTORE_MOVE_AWAY_FILES=()

* Improved 40-start-udev-or-load-modules.sh script for better udevd handling (issue #766)

* Run ldconfig -X before dhclient gets started at boot time (issue #772)

* Remove the "-c3" option fron rsyslogd start-up (issue #773)

* Add example configuration for NetBackup Master/Media server

* Added backup capabilities; getcap and setcap are used to backup and restore (issue #771)

* Correct bash syntax so ReaR is compatible with bash v3 and v4 (issue #765, #767)

* Added support for new backup method Novastor NovaBACKUP DC (BACKUP=NBKDC) (issue #669)

* Code was improved to have network teaming support (issue #655)

* Example configuration to put backup and rescue image on same ISO image, eg. DVD (issue #430)

* Improved the ReaR documentation

* remove the noatime mount option for cifs mount (issue #752)

* Replace option 'grep -P' to 'grep -E' due to SELinux errors (issues #565, #737)

* Hidding the encryption key while doing the restore in the rear.log (issue #749)

* is_true function was to uniform the different ways of enable/disble variable settings (issue #625)

* Added and use sysctl.conf; rescue mode should honor these settings (issue #748)

* The BACKUP_PROG_COMPRESS variable was not used during incremental backup (issue #743)

* prevent any other workflow in REAR rescue mode then recover (issue #719)

* Exclude Oracle ASM device directory from Rescue System (issue #721)

* SaveBashFlagsAndOptions and RestoreBashFlagsAndOptions in usr/share/rear/lib/framework-functions.sh were added (issue #700)

* /mnt/local became a global variable TARGET_FS_ROOT (issue #708)

* Copy rear.log from recovery into /var/log/rear/recovery/ directory after a 'rear recover' (issue #706)

* wipefs will be used when available (issue #649)

* SAN related improvements with btrfs (issue #695)

* Support for shim.efi (UEFI booting) added (issue #702)

* Added support for elilo (used by SLES 11/12) (issue #583, #691, #692, #693)

* Added option --debugscripts to rear (help-workflow) (issue #688)

* Removed dosfslabel as required program for vfat UEFI boot partition (issue #694)

* Bareos team added BAREOS_FILESET and ISO_DEFAULT to default.conf (for automated DR tests executed by Bareos team; issues #686, #719)

* Fix getty/agetty with upstart (issue #685)

* New SLE11-SLE12-SAP-HANA-UEFI-example.conf (issue #683)

* usr/share/rear/conf/examples/SLE12-SP1-btrfs-example.conf added as an example configuration file

* Added support for the SUSE specific gpt_sync_mbr partitioning scheme (issue #544)

* Improved btrfs snapshot support with SLES 12 (issue #556)

* Unload scsi_debug driver in recovery mode for RHEL 7.1 (issue #626)

* Saved the current mount points and permissions; in order to improve and avoid missing mount points after recovery (issue #619)

* NSR servername not defined causing ReaR to hang (issue #637)

* Removed mingetty as a required package (issue #661)

* Adding -scrollprompt=no to dsmc query  in script verify/TSM/default/40_verify_tsm.sh (issue #667)

* Fixed a bug around USB_DEVICE and OUTPUT_URL mis-match (issue #579)

* grub support for ppc64 (issue #673)

* grub2 supported was added for ppc64 (issue #672)

* ppc64le support was added into the rear.spec (issue #665)

* Network code partially rewritten to improve teaming (issue #662)

* Changed default value of USE_CFG2HTML from 1 to empty (means do not run cfg2html by default) (issue #643)

* Move the 50_selinux_autorelabel.sh script to the default location so it gets picked up by all backup methods. This was required for RHEL 7 (issue #650)

* Check via NSR if the ISO image is not obsolete (issue #653)

* Added ebiso support within ReaR (required for UEFI booting with SLES 11 & 12 (issue #657)

* FDR/Upstream (BACKUP=FDRUPSTREAM) (*New*) (issue #659)

### Version 1.17.2 (August 2015)

* Several fixed required to the Debian packaging rules needed so it builds correctly on OBS

* Fixed the NTP startup script (issue #641)

* Fixed the vfat label issue (issue #647)

* Improved the DUPLICITY method with finding all required libraries and python scripts

* Added the `/run` directory to the list of recreating missing directories (issue #619)

* Fix issue with USB disk and rsync as internal backup program (issue #645)

* Fix rsync restore: --anchored invalid rsync option (issue #642)

* A new variable was introduced `NSR_POOLNAME` (issue #640)

* Replaced almost all temporary files from `/tmp/` to `$TMP_DIR/` (issue #607)
  Related to security recommendations for Fedora and RHEL:
  - https://bugzilla.redhat.com/show_bug.cgi?id=1239009 (f22)
  - https://bugzilla.redhat.com/show_bug.cgi?id=1238843 (rhel 7.2)

* Move nfs-client from depends to recommends in Debian control file (issue #633)

* In `packaging/rpm/rear.spec` replaced "BuildArch: noarch" with "ExclusiveArch: %ix86 x86_64 ppc ppc64" that should tell the user that rear is known to work only on %ix86 x86_64 ppc ppc64 and removed "Requires: yaboot" for ppc ppc64 because that is the default installed bootloader on ppc ppc64 (issues #629 and #631)

* Support the Oracle Linux 6 ksplice module (issue #605)

* In script 27_hpraid_layout.sh added the missing `HPSSACLI_BIN_INSTALLATION_DIR` variable to the `COPY_AS_IS` array (issue #630)

* Modified the packaging Makefile and rules for debian to fix the failing OBS Debian builds (issue #604)

* Syslinux version > 5.00 is now supported (ISO and USB output) - works on Debian 8, Ubuntu 15.04 (issue #624)

* Bail out when syslinux/modules are not found in lib/bootloader-functions.sh (issues #467 and #596)
  You could also define a variable `SYSLINUX_MODULES_DIR` if ReaR cannot find it automatically (should not be necessary)

* Support was added for SLES11 on PPC64 hardware (issues #616 and #628)

* Support was added for new hardware - PPC64LE - RHEL and Ubuntu (issue #627)

* FIX the hashed password (`SSH_ROOT_PASSWORD` variable) and added a missing library libfreeblpriv3 (issue #560)

* Insert a 3 seconds sleep after a volume group restauration to give udevd or systemd-udevd time to create needed devices (issue #608 and #617)

* Variable `MANUAL_INCLUDE=YES` has been introduced to work with array `BACKUP_PROG_INCLUDE` (issue #597)

* Add new variable `NSR_DEFAULT_POOL_NAME` (defaulting to `Default`) to use a different default pool name. Renamed the `RETENTION_TIME` variable to `NSR_RETENTION_TIME` (issue #640)

* ReaR website shows the user guide which is part of the ReaR software (linked to GitHub)

* new document 10-integrating-external-backup.adoc which explains the steps to take for a new backup integration into ReaR

* All AsciiDoc documentation changed extention from .txt to .adoc

### Version 1.17.1 (June 2015)

* Removed the plain password in the logs (and output) coming from BACKUP_PROG_CRYPT_KEY to avoid crib (issue #568)

* Mount vfat file system without special mount options seems to work much better then with these options in recovery mode, therefore,
  we do not use these anymore (especially for /boot/efi) (issue #576)

* Elilo support has been added for SLES (not fully tested yet) - issue #583

* Grub2 rescue menu has been added (enable this feature with `GRUB_RESCUE=y`) - issue #589

* splitted script 31_include_uefi_tools.sh in two pieces: 31_include_uefi_tools.sh: to grab the UEFI tools (as long as /boot/efi is mounted), and 32_include_uefi_env.sh: to dig deeper into the configuration when UEFI is active (related to issue #214)

  This is necessary to have to UEFI tools on SLES 11/12 where we cannot create an UEFI bootable ISO image. We must boot in BIOS mode, and need the UEFI tools to make the system bootable over UEFI.

* It is now possible to format an USB disk with a vfat and ext3 partition (for UEFI booting) - issue #593

    `rear -v format -- --efi /dev/<usbdevice>`

* Simplified the code for ext* fs and added StopIfError calls to prevent divide by zero during recovery (issue #598)

* Syslinux version >6 requires some special handling due to splitting up the package (Ubuntu 15.04) (issue #584)

* Debian 8 support added with ISO booting with latest syslinux release as well (issues #596, #599 and #600)

* Changed the behavior of SSH_ROOT_PASSWORD - now saved as MD5 hash password, but backwards compatibility is still respected (issue #560)

* For EMC NetWorker server/client we added some exclude items to COPY_AS_IS_EXCLUDE_NSR (issue #571)

* Removed the Warning message from main rear script as it was misleading (issues #563 and #564)

* output/ISO/Linux-i386/80_create_isofs.sh: make sure ISO_FILES[@] are copied to isofs directory (issue #569)


### Version 1.17.0 (March 2015)

* ReaR 1.17 introduced a new stage directory (`init`) which is called before any workflow. Can be used for extra initialization code, custom configurations and other stuff that should happen each time that ReaR is used (was added especially for DRLM as plugin for their DRLM variables) (issue #557)

* RHEL 5 could not mount NFS share as `portmap` daemon was not started yet. Reason was that script `verify/NETFS/default/08_start_required_daemons.sh` was executed after the mount test instead of before. (issue #547)

* RHEL 6 on PPC64 architecture has problems with *seclabel* mount option. There was a fix which cuts the *seclabel* mount option (as ReaR is by default not using SELinux) (issue #545)

* [Disaster Recovery Linux Manager (DRLM)](http://drlm.org/) is a new Open Source project to centrally manage Disaster Recovery for Linux. ReaR 1.17 adds support for DRLM with a new configuration option `DRLM_MANAGED=y` (issue #522)

* Adding /etc/crypto-policies/ when openssl is requested (`prep/NETFS/default/09_check_encrypted_backup.sh`) - required for https://bugzilla.redhat.com/show_bug.cgi?id=1179239 (issue #523)

* Introduced USING_UEFI_BOOTLOADER in default.conf file and modified all the involved scripts to follow the logic (issue #214)

* Archicture PPC64 has been enhanced (partition recognition issue #536, console #537)

* TSM restore filespace has been enhanced (issue #535)

* Correct the usage of set -e and StopIfError function (issue #534, #541)

* With BACKUP_TYPE=incremental disable variable NETFS_KEEP_OLD_BACKUP_COPY (mutual exclusive with this mode) (issue #143)

* Increased to start variable from 32768 to 2097152 in script 10_include_partition_code.sh (issue #492)

* Adding /run/resolvconf/resolv.conf (Ubuntu) to ReaR image when found (issue #520)

* When using /etc/rear/mappings/ip_addresses IPADDR/cidr must be translated into IPADDR and NETMASK (for RHEL 5 and 6) (issue #460)

* mdadm layout code was changed in RHEL 7; code to deal with it (issue #518)

* Adding missing driver xhci_hcd to GNU/Linux.conf (issue #519)

* Adding findmnt command to GNU/Linux.conf file; request from https://bugzilla.opensuse.org/show_bug.cgi?id=908854

* BTRFS code has been rewritten by J. Meixner/G. Dhaese to get SUSE rear116 fork back into our master tree of ReaR. BTRFS is now properly recreated on Fedora 20 and 21 and on SLES 12 and openSUSE 13 (issue #233, #497, #538)
See also https://bugzilla.opensuse.org/show_bug.cgi?id=908854

* Enhanced the systemd start-up of udev daemons which is different on Fedora 20 and 21 (issues #507, #531)

* BACKUP_PROG_OPTIONS="--anchored" became a default option in default.conf (issue #475)

* Fixed an issue with auto-detection of the client name in BAREOS (issue #542)

* Ubuntu 14.04 did not detect /dev/sda automatically - added upstart script 99-makedev.sh to create missing block devices like sda (issue #446)

* New script usr/share/rear/layout/save/GNU/Linux/34_false_blacklisted.sh added as work-around.
Sometimes we might see the HP Smart Storage Array disk listed as a multipath device. Most likely this device was not blacklisted in the blacklist section of the /etc/multipath.conf file (do not forget to rebuilt the initial ramdisk after this) e.g. falsempathdev=$( multipath -l | grep "HP,LOGICAL" | awk '{print $1}' )  # mpatha

* Added a systemd service script for SEP Sesam to start sesam client automatically (issue #498)

* Disk Layout Generation with HP SmartArray with more than one Logical Drive correction introduced by issue #208 (issue #455)

* OUTPUT_URL=null was added to avoid 2 ISO images on the local system (issue #501, #419)

* Fix drbd restore code so it can handle multiple volumes in single resource (issue #495)

* Function get_device_name was enhanced to properly translate /dev/vg/lv to /dev/mapper/vg-lv (issue #494)

* Fixed the "lvm wait yes" problem (issue #493)

* Device name conflict when grep disklayout file without proper space (issue #491)

* Several fixes with mdadm code (issue #480, #489, #490, #508, #539)

* Several fixes in the 10_include_partition_code.sh script (issue #487, #492)

* Being a bit more careful when deleting the outputfs directory when ReaR finishes (issue #465)

* Add a new config option USE_STATIC_NETWORKING (issue #488)

* Resolving library search with duply (issue #482)

* Improved the RPM package build for SLES, removed the lsb requirement (issue #479)

* Integrated the patch skip_sysconfig_kernel_if_not_exist.diff from SUSE (issue #477)

* Debian 7 reference to grub.conf was corrected (issue #473)

* Debian packing was improved (issue #474, #484)

* Commercial backup solution Galaxy 10 support added to ReaR (issue #471)
  Also, point in time recovery was implemented (issue #472)

* Fixed the error "unary operator expected with BACKUP_TYPE" (issue #422)

* lvm release 2.02.72 had already the knowledge of --norestorefile (issue #462)

* Add prefix $OUTPUT_PREFIX to all PXE related files (issue #464)

* 55-migrate-network-devices.sh: fixed type with /sys/class/net (issue #459)

* Removed Get_Start_Date function with Netbackup because (depending on backup strategy) it led to partial restore only (issue #456)

* 26_crypt_layout.sh: fixed issue when cypher contains `:` (issue #425)

* 55-migrate-network-devices.sh: rebuild the array instead of unset (issue #452)

* skel/default/etc/scripts/system-setup : change `uname -n` by $HOSTNAME to avoid short/long hostname problems (issue #439)

* Added a mkdir for $VAR_DIR/output if dir does not exist (issue #440)

* 95_check_missing_programs.sh to find REQUIRED_PROGS before proceeding (issue #418)

* Added a fix for autorizing and recognizong the boot flag in the partition (issue #443)

* udev showing error message during startup (issue #442)

* Prevent "ntpd -q" from waiting forever if, for example, no network is available, by killing it after 10 seconds (issue #438)

* Several improvements around duply and duplicity [Debian related] (issues #426, #478)

* 95_copy_result_files.sh uses now [*] instead of [@] with array (issue #431)

* New variable introduced for TSM in `default.conf` file called LANG_RECOVER (issue #424)

* Added Pre check script for removing older releases first during upgrade (RPM - issue #361)

* Prevent udev rule rewrites if systemd-udevd is running - issue #405

* Fix "Networker backup fails if pool name contains spaces" (issue #427 and #429)

* Fix the "More than 128 partitions is not supported" problem (issue #373 and #428) during recreating the partition layout

* A new configuration option, `USE_STATIC_NETWORKING=y`, will cause statically configured network settings to be applied even when `USE_DHCLIENT` is in effect (issue #447).

### Version 1.16.1 (June 2014)

* The validate rule for `xarg bash -n` changed into `xarg -n bash -n` so that ReaR is working correctly with older bash version 3 as well. Especially required for SLES 10, SLES 11 and EPEL 5 (issue #410).

* Adding an updated ReaR man page and the correct release notes to the 1.16.1 sub-release. In ReaR 1.16.0 these were forgotten.

* Code style improvements (issue #409).

* Improvements around btrfs file system recognition and re-creation (issues #408 and #415).

* Important fix for grub2 with openSUSE 13 (changed LOADER into LOADER_TYPE).

* Added example prep script to verify if extlinux is present (issue #250) - this type of script could be cloned for other important executables in various workflows.

### Version 1.16.0 (May 2014)

* Network script now deals correctly with VLAN tagging as well

* Added 2 new variables to `default.conf` for TSM: `TSM_ARCHIVE_MGMT_CLASS` which defines a backup class to use with TSM, and `TSM_RM_ISOFILE` which triggers (if set to y) to remove the original ISO files from local system once the ISO image has been transferred to TSM

* Guessing the bootloader and saving this in a file in the `$VAR_LIB/recovery/bootloader` (will be picke dup during recovery to assist us in selecting the proper bootloader [ grub, grub2, uefi ])

* Various corrections with `BACKYP=RSYNC` method (issue #200 and #366). In ReaR 1.16 the old and new style are still recognized, but we might drop the old style in later versions.

       OLD STYLE:
       BACKUP_URL=[USER@]HOST:PATH           # using ssh (no rsh)
       with rsync protocol PATH is a MODULE name defined in remote /etc/rsyncd.conf file
       BACKUP_URL=[USER@]HOST::PATH          # using rsync
       
       NEW STYLE:
       BACKUP_URL=rsync://[USER@]HOST[:PORT]/PATH    # using ssh
       BACKUP_URL=rsync://[USER@]HOST[:PORT]::/PATH  # using rsync

* Checking the bootloader files during savelayout section - see issue #234

* Backup method `BACKUP=DUPLICITY` has been fully tested and proven to work well (backup and recover). When using `duply` and automated recovery is possible. Several updates were made to make duplicity works as well with backup and recover

* Variable `SSH_ROOT_PASSWORD` (see `default.conf`) allows to force a password with the rescue image (issue #362)

* TSM version >=6.4 is now supported (issue #356), and point in time restore works better now (issue #358)

* Fix the order of `fs` lines in `checklayout.conf` file when mor ethen 9 partitions are in use (issue #352)

* Avoiding CD/DVD type devices to be added in the disklayout.conf file during savelayout (issue #257)

* Removed `mingetty` executable from `REQUIRED_PROGS` list as RHEL 7 does not ship it anymore. `agetty` is getting more and more the standard now and ReaR will use this one automatically when `mingetty` is absent

* New variable in `default.conf` file was defined `BACKUP_TYPE=` (empty is full or incremental; only works with the default `BACKUP_PROG=tar`)

* Avoid stale NFS hangs with `df` command (issue #350)

* For TSM a new variable was introduced in `default.conf` file: TSM_RESULT_SAVE (to disable the TSM saving; can be used when you saved the result via `$ISO_URL`, `$OUTPUT_URL`, `$ISO_DIR` or something else)

* `/etc/rear/local.conf` is now empty by default and `/etc/rear/site.conf` now contains `OUTPUT=ISO` setting. This way we are sure that during an upgrade of ReaR we do not loose our original user settings (local.conf tends te be overwritten).

* Fix for hpacucli where the order logicaldrive and array could be wrong (issue #208)

* New variable TMPDIR allows to overrule the default `/tmp` temporary directory (handy when building big ISO images)

* Added option `-iso-level 3` to `mkisofs` command so we can create ISO image bigger then 4 GB (issue #323)

* detecting 'out of space' errors while building huge ISO images

* Add SEP Sesam external backup integration - issues #324 and #325

* Add automatic recovery mode for Bareos - issue #311

* Add incremental backup type with GNU tar with weekly backups, define in _/etc/rear/local.conf_ the following (issue #294):

       BACKUP=NETFS
       BACKUP_TYPE=incremental
       FULLBACKUPDAY="Mon"

* Add support for Gentoo kernels - issue #312

### Version 1.15.0 (September 2013)

* Add EMC NetWorker (Legato) support for doing _external_ backup

* Add Bareos (Backup Archive REcovery Open Sourced) support for doing _external_ backup (bareos
  is a fork of the open source backup suite Bacula)

* Align the usage of KEEP_OLD_OUTPUT_COPY and NETFS_KEEP_OLD_BACKUP_COPY and make them behave sane.
  See also issue #192

* Add the --selinux option to be safe with SELinux context restoration (with GNU tar)

* Add an option to exclude modules on the rescue system

  default.conf: EXCLUDE_MODULES=()

* Add integrity check when BACKUP=NETFS and BACKUP_PROG=tar with the option BACKUP_INTEGRITY_CHECK

* Add an option to define a root password to allow SSH connection whithout a public/private key pair (#272)
  
  default.conf: SSH_ROOT_PASSWORD=

* Implement the "splitted backup to iso" feature.  To use that, BACKUP_URL has to use the iso://[subdirs] scheme but
  a different OUTPUT_URL has to be defined (issues #278 and #287). New parameter ISO_MAX_SIZE introduced.

* Fix SELinux autorelabelling (issue #270 and #274)

* Rear supports now more then 9 partitions (see issue #263)

* systemd support added for Fedora 19/20 and openSUSE 12.x/13.x

* Automatic Recover feature on USB devices (label AUTOMATIC RECOVER in rear recover menu)

* Add an option to copy all users and group. It is usefull in the case we want to restore ACL of all users. See default.conf:

  CLONE_ALL_USERS_GROUPS=n

* Example prep script to detect missing lftp (issue #247)

* Using POSIX output format in df.txt (issue #248)

* Recognize OracleServer as a Fedora derivate

* Allow basic network configuration from the command line

* Create directory $PXE_TFTP_LOCAL_PATH if it does not exist (issue #244)

* Support booting syslinux v5 (issue #238)

* Fix installing grub when /boot is inside the root filesystem (issue #210)

* Deal with BTRFS subvolumes correctly (issues #233 and #252)

* Recognize OracleServer as a Fedora derivate

* Added UEFI integration within ReaR (including booting ISO images)

* Improved dataprotector GUI interaction (issue #189)

* Secure backup was introduced (issue #196)
  
    Add to /etc/rear/local.conf:
    BACKUP_PROG_CRYPT_ENABLED=1
    BACKUP_PROG_CRYPT_KEY="secretkey"

* Added fixes recomended by RedHat Bugzilla report #882175

* OUTPUT_URL scheme approvement (issue #37)

* Netbackup improvement for xinetd (issue #180)

* LOGFILE variable can be overruled now (issue #56)

* Support for RBME restores over NFS

* Packaging support introduced for Arch Linux

* Added support for sshfs transport with BACKUP=NETFS (issue #171)

* Re-introduced /dev/disk/by-id migration which was lost since release 1.7.22 (issue #22)

* Updates required for systemd for fedora 18 and 19.

* Check if ROOT_FS filesystem was mounted with 'noexec' attribute (issue #150)

### Version 1.14.0 (September 2012)

* Added duply/duplicity with one new backup method duplicity (*Experimental*)

* Systemd supported on Fedora 17 and openSUSE 12.2 (issues #115, #126)

* Create correct yaboot dir on ppc (issue #109)

* Add new RAMDISK output method. This writes the kernel and the initramfs to the location given in OUTPUT_URL.

* Packaging - introduction of Makefile; cleanup dr workflow (issues #13, #49)

* Make ReaR working from checkout of git, w.o.w. path is relocatable (issue #53)

* Add bytes_per_inode information for ext* filesystem to layout.conf (issue #86)

* Fixing ebuild to be Gentoo compliant (issue #93)

* fix shutdown with upstart (issue #41)

* multiarch support library support (issue #82)

* Fix serial console on ubuntu 11.04 (issue #83)

* Several fixes in layout file (issue #85)

* Added fix for DAT320 tape drive (issue #35)

* Use generic grub code for all distributions (issue #77)

* list Xen paravirtualized disks in disklayout.conf (fixes #74 and SF3520992)

### Version 1.13.0 (April 2012)

* Support for multipathing was added

* Several improvements and bug fixes to the layout code (especially with parted backwards compatibility).

* Added support for ext4 file systems

* The OUTPUT=USB with BACKUP=NETFS and BACKUP_PROG=rsync was corrected to avoid duplicate work

* Fedora and RHEL will now rebuild the initial ramdisk if needed (on recovered system)

* Fix for SF#3475480: datacompression on tape

* Fix for SF#3481656: missing bacula-console executable for BACKUP=BACULA workflow

* Fix for SF#3479570: /etc/passwd contains `:x:` without /etc/shadow

* Added "migrate HWADDR after cloning" code

* Improved the systemd code (parts required by Relax-and-Recover only) for Fedora 16/17

* The DHCLIENT variables were moved from local.conf to rescue.conf. This is done automatically, so the end-user, shouldnot be aware of it.

* At boot time more kernel options are recognized such as `noip`, `dhcp`,
  `debug`. The `noip` will give you a rescue environment without any attempt to
  start networking. The `dhcp` variable will try to start dhclient on any
  network interface it finds activated. The `debug` variable (which is not new
  by the way) will give you the chance to debug the code of our
  Relax-and-Recover code.

* Relax-and-Recover works again on IA64 architecture (at least with RHEL 5.x). Remember, RHEL 6 is not ported to IA64.



### Version 1.12.0 (November 2011)

* Multiple copies of Relax-and-Recover backups (of the same or different
  systems) can be kept on a USB device (with `OUTPUT=USB`).

* (*NEW!*) `BACKUP=RSYNC` workflow using `rsync` executable. Both `ssh` and `rsync` methods are supported. E.g.

        BACKUP=RSYNC
        OUTPUT=ISO
        BACKUP_URL=rsync://username@hostname/path
        BACKUP_PROG=/usr/local/bin/rsync #(instead of the default rsync)


* Added better named `EXCLUDE_` variables, better control over what is restored:
  - `EXCLUDE_BACKUP` excludes components from backup
  - `EXCLUDE_RECREATE` excludes components from the recreate process
  - `EXCLUDE_RESTORE` excludes components from the restore process

* The *layout* workflow is now the default instead of the *dr* workflow.  Under _/var/lib/rear/layout_ all information of the system is kept in files.

* Arch Linux is now supported with Relax-and-Recover.

* The `labeltape` command has been superseded by the `format` command. This can be used with tapes and external (USB, eSATA) devices. Usage:

        rear format [/dev/st0|/dev/sdx]


* Replaced `NETFS_URL` and `ISO_URL` by `BACKUP_URL` and `OUTPUT_URL`. However, old references will still be recognized and used.

* Fedora 16 is supported including GRUB 2, and systemd as init replacement.

* Added the `BACKUP_URL=file:///PATH` with `BACKUP=NETFS` method (as described in _configuration-examples.txt_)

* Improved multipath functionality

* Optional automatic autofs exclusion

* (*NEW! EXPERIMENTAL!*) Basic btrfs file system backup and restore works. Advise is not to trust it (yet). At recreation of the btrfs file system the UUID number is automatically renamed in all configuration files (such as _/etc/fstab_ or _/boot/grub/menu.lst_).


### Version 1.11.0 (May 2011)

* The `mkobdr` command has been removed. OBDR-enabled tapes can now be created using the `mkrescue` command and by defining the proper variables in _/etc/rear/local.conf_:

        BACKUP=NETFS
        OUTPUT=OBDR
        BACKUP_URL=tape:///dev/nst0


* The site configuration file _/etc/rear/site.conf_ has been removed from the
  Relax-and-Recover package, but can still be used if end-users want. The purpose of this
  is to enable sites to distribute this file through RPM or DEB packages that
  do not have a file conflict with the Relax-and-Recover package. The distribution
  _/etc/rear/local.conf_ file contains only configuration examples as comments
  in order to not interfere with configurations in _site.conf_.

* The `rear` command is by default quiet, which means if you want the same
  behavior as in previous versions you need to add the verbose option (`-v`)
  with the `rear` command

* The *output* workflow now runs before the *mkbackup* workflow especially done
  to make OBDR tape creation possible with the *mkbackup* workflow as the ISO
  image must be written first on an OBDR aware tape. Please note that this is
  a fundamental change with regard to previous versions of Relax-and-Recover.
  While utmost care has been taken that there would be no adverse side effects
  of this change. We cannot test all possible usage scenarios.

* When using `OUTPUT=USB` then you have to make sure that the destination (USB)
  disk is formatted as an ext2, ext3, ext4 or btrfs file-system. Extlinux is
  now the only supported boot loader for bootable disks, syslinux is not
  supported any more.

* The Relax-and-Recover boot now shows a boot menu with options to choose from.
  The actual content of the menu depends on the available syslinux version and
  its modules (like menu.c32, hdt.c32, reboot.c32, poweroff.com).

* Relax-and-Recover does properly recognize IPv6 addresses and uses these if
  configured.

* NBU backup method now allows to restore to a point in time.

* Support Fedora 15 (using systemd to boot-up) and RHEL6 and Scientific Linux 6.

* Improved handling of HP SmartArray controllers.

* Significantly improved error handling, especially when failing on subshells.

* Autologin as root in the rescue media (for upstart and systemd based systems).

* `EXCLUDE_MOUNTPOINTS` should work correctly now (fixed typo).

* Support ext4 on RHEL5 and clones.

* Ignore known errors when using `EXTERNAL` backup method (set 
  `EXTERNAL_IGNORE_ERRORS` to an array of return codes to ignore).

* Use original filesystem mount options for recovery, support `attr` and `facl` tools.

* Support XEN paravirtualized systems (tested only on RHEL5 so far).

* Performance improvements (removed checksum calculation, PID-based locking).

* Relax-and-Recover work space is now created with a random part to prevent
  potential security exploits.

* Control exit tasks and subprocesses better. Kill subprocesses before exiting.

* Support adding Relax-and-Recover boot files to local GRUB environment
  (`GRUB_RESCUE`) and password protect rescue boot (`GRUB_RESCUE_PASSWORD`)
  to avoid accidential recovery.

  The default password is *REAR*.

* *(experimental!)* Transfer ISO image to remote URL (`ISO_URL`). Please note
  that this feature will be extended to cover all output methods. It has been
  renamed to `OUTPUT_URL`.

* Removed various warnings, e.g. about NETFS not being a professional backup
  method.

* Partial support for Arch Linux has been added, more testing required.

* (*NEW!*) shell workflow is now really usable.

* Make 32/64 bit handling much more robust, especially on systems having /lib32.

* NETFS backup and restore with rsync working now (`BACKUP_PROG=rsync`).

* Support udev on RHEL4.

* Development snapshot have now a version like 0.0.REV where REV is the SVN
  revision used to build the development snapshot.

* Greatly reduced log clutter (lvm warnings about leaked file descriptors,
  which is a bash bug, various irrelevant error and verbose output).

* checklayout can now also check arbitrary files (through an md5 checksum),
  extend the `CHECK_CONFIG_FILES` array to use this feature.

### Version 1.10.0 (February 2011)
An intermediate release only which fixed some hanging issues of version 1.9.0.
Also, a RPM upgrade was fixed by this release from 1.7.25 to 1.9.0, which
failed because of a wrongly CentOS symbolic link. See
[bugzilla#680664](https://bugzilla.redhat.com/show_bug.cgi?id=680664)


### Version 1.9.0 (February 2011)
With version 1.9.0 some new methods were added, such as:

* `rear mkobdr` : to create an OBDR recovery tape (obsolete since 1.11.0)

* `rear labeltape` : goes together with OBDR tapes. To avoid accidental
overwrites we force the creation of a label before `rear mkbackup` will
work. (obsolete since 1.11.0)

* `rear checklayout/savelayout` : a new method to save the disk layout
and check if a new `rear mkbackup` or `rear mkrescue` is required.

* New BACKUP methods were added, Bacula (`BACKUP=BACULA`) and bextract
(`BACKUP=BEXTRACT`), both are able to work in conjunction with
*output=TAPE*. See under the doc directory (or _/usr/share/doc/rear-1.9.0/_)
the _configuration-examples.txt_ text file for beginners instructions.

* `OUTPUT=USB` method has been extended with `BACKUP=NETFS` and
`NETFS_URL=usb:///dev/<device>` which makes it possible that the
complete archive is stored on the `/dev/<device>` and 
Relax-and-Recover will make the USB stick (or disk) bootable too.

* Udev support (except for some really ancient udev versions) which is the
base for many new and important features, like kernel drivers for network
cards and storage adapters are now loaded via udev, or deal with network
persistent names in udev rules, and firmware loading.

* DHCP client support (IPv4 and IPv6) has been added. Auto detection
is possible with new variable `USE_DHCLIENT=yes` (define in _local.conf_),
or one can hard-code your special DHCP client with the variables
`DHCLIENT_BIN` (for IPv4), and/or `DHCLIENT6_BIN` (for
IPv6).

Relax-and-Recover version 1.9.0 contain fixes for the following defects:

* Missing support for Scientific Linux, LinuxMint

* Sourceforge patch ID 2963804 - support for USBFS, but this patch has been
rewritten afterward to incorporate usb support into the NETFS backup method,
instead of having a separate USBFS backup method. Now, by using
`NETFS_URL=usb:///dev/<device>` and the NETFS backup method we achieve the
same result.

* Sourceforge bug 3153027 : support for RHEV virtio device files

* Novell bugzilla 581292 : cannot load NIC firmware because of missing udev
support.  Version 1.9 does have udev support, but firmware loading was
broken. The rule in 00-rear.rules has been changed.


### Version 1.7.26 (November 2010)
Relax-and-Recover version 1.7.26 fixed RedHat bugzilla defect 657174 : rescue
image freezes during the boot while executing init. This was due the new
upstart mechanism (replaced the sysv init procedure).


### Version 1.7.25 (June 2010)
Relax-and-Recover version 1.7.25 fixed RedHat bugzilla defect 600217 : Fedora
link missing in restore, pack and finalize sub-directories. This broke the
proper image building on several Fedora versions.


## System and Software Requirements
Relax-and-Recover works on GNU/Linux kernel with version 2.6 and higher.
For lower kernel versions Relax-and-Recover cannot be used, and for these
systems, [mkcdrec](http://mkcdrec.sourceforge.net/) is still a good
alternative.

As Relax-and-Recover has been solely written in the *bash* language we need
the bash shell which is standard available on all GNU/Linux based systems.
The default backup program Relax-and-Recover uses is GNU/tar which is also
standard available.

Relax-and-Recover is known to work well on x86 and x86_64 based architectures.
Relax-and-Recover has also been ported to ia64 and ppc architectures, but
these are less tested.  Use the '`rear validate`' command after every
successful DR test please and mail us the results.


### Choosing the best compression algorithm
The default backup program with Relax-and-Recover is (`BACKUP_PROG=tar`)
GNU tar and the default compression used with tar is `gzip`. However, is
using `gzip` the best choice? We have done some tests and published the
results. See
[Relax-and-Recover compression tests](http://www.it3.be/2013/09/16/NETFS-compression-tests/)

## Support
Relax-and-Recover (rear) is an Open Source project under GPL v3 license which means
it is free to use and modify. However, the creators of ReaR have spend many, many hours in 
development and support. We will only give *free of charge* support in our free time (and when work/home balance
allows it).

That does not mean we let our user basis in the cold as we do deliver support as a service (not free of charge).

## Supported Operating Systems
We try to keep our wiki page [Test Matrix rear 2.1](https://github.com/rear/rear/wiki/Test-Matrix-rear--2.1) up-to-date with feedback we receive from the community.

Rear-2.1 is supported on the following Linux based operating systems:

* Fedora 24 and 25
* RHEL 5, 6 and 7
* CentOS 5, 6 and 7
* ScientificLinux 6 and 7
* SLES 11 and 12
* openSUSE Leap and openSUSE Tumbleweed
* Debian 6, 7 and 8
* Ubuntu 12, 13, 14 and 15

Rear-2.1 dropped officially support for the following Linux based operating systems:

* Fedora <24
* RHEL 3 and 4
* SLES 9 and 10
* openSUSE <13
* Debian <6
* Ubuntu <12

If you require support for *unsupported* Linux Operating System you must acquire a *ReaR support contract*.

Requests to port *ReaR* to another Operating System (not Linux) can only be achieved with **serious** sponsoring.

## Supported Architectures
Rear-2.1 is supported on:

* Intel x86 type of processors
* AMD x86 type of processors

Rear-2.1 may or may not fully work on:

* Intel Itanium processors
* PPC processors
* PPC64 processors
* PPC64LE processors

Rear-2.1 does not support:

* ARM type of processors
* s390x type of processors

If you feel the need to get a fully functional ReaR working on one of the above mentioned type of processors please buy
consultancy from one of our official developers.

## Supported ReaR versions
ReaR has a long history (since 2006) and we cannot supported all released versions anymore. If you have a problem we urge you to install the latest stable ReaR version or the development version (available on github) before submitting an issue.

However, we do understand that it is not always possible to install on hundreds of systems the latest and greatest version so we are willing to support previous versions of ReaR if you buy a support contract. Why do we change our policy? We cannot handle the big support requests anymore and we must give paid projects priority, therefore, we urge our customers to buy a support contract for one or more systems. You buy time with our core developers...


## Known Problems and Workarounds

*Issue Description*: tar --test-label is not supported on Centos 5 who have tar version 1.15

* Workaround:

Read the comments in [issue #1014](https://github.com/rear/rear/issues/1014)


*Issue Description*: Debian 8 initramfs-tools: / on LVM gets mounted by initrd with kernel device name /dev/dm-X instead of /dev/mapper/XXX name

[Issue #613](https://github.com/rear/rear/issues/613) goes deeper into the problem with Debian 8 and LVM on /. The reporter created also a [Debian Bug report #791754](https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=791754) for it.

* Workaround:

See the fix mentioned in [Debian Bug report #791754](https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=791754)

*Issue Description*: BACKUP=NSR on RHEL 6 could break yum

[Issue #387](https://github.com/rear/rear/issues/387) describes a problem seen on RHEL 6 where when `rear` uses NSR and afterwards the link `/lib64/libexpat.so.1` has been changed.

* Workaround:

So far there is *no* workaround for this issue.

*Issue Description*: usage of an alternative configuration directory is different in mkbackup or recover mode

Using `rear -v -c /etc/rear/mydir mkbackup` works fine in production, but when you try (once booted from rescue image) `rear -v -c /etc/rear/mydir recover` it will fail.

* Workaround:

The configuration files are copied to `/etc/rear/` into the rescue image, so you simply need to type: `rear -v recover`
See issue #512

*Issue Description*: Is there a possibility to add btrfs subvolume to a rsync backup

* Workaround:

At present (release 1.18) there is no workaround in place. If you happen to know how this could be fixed then add your ideas to [issue #417](https://github.com/rear/rear/issues/417)

*Issue Description*: UEFI ISO booting does not work on openSUSE 12.x, or SLES 11/12

* Workaround:

At present (release 1.18.x and higher) `genisoimage` cannot produce ISO images that can boot via UEFI on an openSUSE distribution (and also SLES). However, use the [`ebiso`](http://download.opensuse.org/repositories/Archiving:/Backup:/Rear/SLE_11_SP3/x86_64/ebiso-0.2.3-1.x86_64.rpm) package instead to create UEFI ISO images on SLES.

*Issue Description*: TSM 7.1.0.0 cp writing dangling symlink libxmlutil-7.1.0.0.so on SLES SP3

* Workaround:

To fix the error (you might see):

    ERROR [LipCopyTo] Could not copy '/usr/lib64/../../opt/tivoli/tsm/client/api/bin64/libxmlutil-7.1.0.0.so' to '/tmp/rear.10455/rootfs/lib64'

See [issue 476](https://github.com/rear/rear/issues/476) for the resolution.


*Issue Description*: System reconfiguration still has some weaknesses.

* this has to be tested before relying on it, there are too many unknowns
involved so that we cannot guarantee anything in this area. It has been
developed mostly as a P2V tool to migrate HP servers to VMware Vms

* hard disks need to be at least of the same size and amount as in the
original system, ATM this is a simple 1:1 mapping of old to new disks,
there is no removal of RAID groups or merging of smaller disks onto a
bigger one or making stuff smaller.

* any use of _/dev/disk/by-path_ or _/dev/disk/by-id_ is untested and will
most likely not work. In some cases Relax-and-Recover will print a warning,
but we are not able to detect all cases. Typically this leads to unbootable
systems or bad _/etc/fstab_ files


*Issue Description*: If SELinux is not disabled during backup (variable
`BACKUP_SELINUX_DISABLE=` in _/etc/rear/local.conf_) then we might see
errors in the `rear-$(hostname).log` file such as:

    tar: var/cache/yum/i386/15/updates/packages: Cannot setfilecon: No such file or directory

* Workaround:

Make sure the `BACKUP_URL` destination understands extended attributes
(CIFS is out of the question and NFS is problematic). When using local
disks (or external USB devices) make sure the proper mount options are
given in the `BACKUP_OPTIONS` variable, e.g.:


    BACKUP_OPTIONS="rw,relatime,seclabel,user_xattr,acl,barrier=1,data=ordered"


(TIP) `BACKUP_SELINUX_DISABLE=1` variable has been introduced in the
_/usr/share/rear/conf/default.conf_ file to disable SELinux
while the backup is running (default setting).

*Issue Description*: Is incremental backup possible?

With our default settings (`BACKUP=NETFS` and `BACKUP_PROG=tar`) we support
incremental backups when we add the following extra variables:

    BACKUP_TYPE=incremental
    FULLBACKUPDAY="Mon"


*Issue Description*: ERROR: FindStorageDrivers called but STORAGE_DRIVERS is empty

Above error message might be seen after a fresh installation of the GNU/Linux kernel. Rear got confused between the running kernel version number and the actual fresh kernel available.

* Workaround:

Reboot your server before using ReaR again, which is a good practice anyway after upgrading the GNU/Linux kernel.

