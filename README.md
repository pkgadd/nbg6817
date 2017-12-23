# nbg6817
tools and infos about the ZyXEL NBG6817

## Files
* nbg6817-dualboot: small tool for getting and setting the dualboot flag on the ZyXEL NBG6817, this can be used on LEDE and the ZyXEL OEM firmware
* dualflag-0x01-mmcblk0p8.bin: dump of the dualflag mtd, configured for 0x01/ /dev/mmcblk0p8
* dualflag-0xFF-mmcblk0p5.bin: dump of the dualflag mtd, configured for 0xFF/ /dev/mmcblk0p5

## Partitioning

### 4 MiB SPI-NOR (Macronix MX25U3235F)

#### ZyXEL OEM firmware V1.00(ABCS.5)C0:

    # cat /proc/mtd 
    dev:    size   erasesize  name
    mtd0: 000c0000 00010000 "SBL"
    mtd1: 00040000 00010000 "TZ"
    mtd2: 00040000 00010000 "RPM"
    mtd3: 00080000 00010000 "u-boot"
    mtd4: 00010000 00010000 "env"
    mtd5: 00010000 00010000 "ART"
    mtd6: 00010000 00010000 "dualflag"
    mtd7: 00210000 00010000 "reserved"

#### LEDE

    # cat /proc/mtd 
    dev:    size   erasesize  name
    mtd0: 00020000 00010000 "0:SBL1"
    mtd1: 00020000 00010000 "0:MIBIB"
    mtd2: 00020000 00010000 "0:SBL2"
    mtd3: 00040000 00010000 "0:SBL3"
    mtd4: 00010000 00010000 "0:DDRCONFIG"
    mtd5: 00010000 00010000 "0:SSD"
    mtd6: 00040000 00010000 "0:TZ"
    mtd7: 00040000 00010000 "0:RPM"
    mtd8: 00080000 00010000 "0:APPSBL"
    mtd9: 00010000 00010000 "0:APPSBLENV"
    mtd10: 00010000 00010000 "0:ART"
    mtd11: 00010000 00010000 "0:DUAL_FLAG"
    mtd12: 00210000 00010000 "0:RESERVED"

### 4 GiB internal eMMC (Kingston S10004)

    # gdisk -l /dev/mmcblk0
    GPT fdisk (gdisk) version 1.0.3
    
    Partition table scan:
      MBR: protective
      BSD: not present
      APM: not present
      GPT: present
    
    Found valid GPT with protective MBR; using GPT.
    Disk /dev/mmcblk0: 7471104 sectors, 3.6 GiB
    Sector size (logical): 512 bytes
    Disk identifier (GUID): XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX
    Partition table holds up to 12 entries
    Main partition table begins at sector 2 and ends at sector 4
    First usable sector is 34, last usable sector is 7471070
    Partitions will be aligned on 2-sector boundaries
    Total free space is 1 sectors (512 bytes)
    
    Number  Start (sector)    End (sector)  Size       Code  Name
       1              34            8225   4.0 MiB     FFFF  rootfs_data
       2            8226           16417   4.0 MiB     FFFF  romd
       3           16418           18465   1024.0 KiB  FFFF  header
       4           18466           26657   4.0 MiB     FFFF  kernel
       5           26658          157729   64.0 MiB    FFFF  rootfs
       6          157730          159777   1024.0 KiB  FFFF  header_1
       7          159778          167969   4.0 MiB     FFFF  kernel_1
       8          167970          299041   64.0 MiB    FFFF  rootfs_1
       9          299042          823329   256.0 MiB   FFFF  bu1
      10          823330         7471069   3.2 GiB     FFFF  bu2
