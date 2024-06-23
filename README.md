# BIK-ADU



## Cviceni 1


### 7.1 

    vi /etc/passwd
    user1:x:2001:2001::/home/user1:/bin/bash
    user2:x:2002:2002::/home/user2:/bin/bash

    vi /etc/shadow
    user1:!!:19732:0:99999:7:::
    user2:!!:19732:0:99999:7:::
    
    passwd user1
    passwd user2

    vi /etc/group
    adu1:x:101:user2


    vi /etc/passwd
    user1:x:2001:101::/home/user1:/bin/bash

    
    mkdir /home/user1
    mkdir /home/user2

    chown user1 /home/user1
    chown user2 /home/user2


### 7.2

    useradd -u 2003 -m -d /home/user3 user3
    useradd -u 2004 -m -d /home/user4 user4
    groupadd -g 102 adu2
    usermod -g adu2 user3
    usermod -G adu2 user4

### 8

    authconfig --passalgo=sha256 --update

### 9

    vi /etc/login.defs 
    PASS_MIN_DAYS=7

    vi /etc/pam.d/system-auth
    password sufficient pam_unix.so use_authtok md5 shadow remember=13

    touch /etc/security/opasswd

    vi /etc/security/pwquality.conf 

## Cviceni 2


### 1

    echo "$1:x:$2:101::/home/$1:/bin/bash" >> /etc/passwd
    echo "$1:!!:19732:0:99999:7:::" >> /etc/shadow
    mkdir /home/$1
    cp ~/.bashrc /home/$1
    cp ~/.profile /home/$1
    cp ~/.bashrc /etc/skel/
    cp ~/.profile /etc/skel/
    chown -R $1 /home/$1
    chmod 755 /home/$1

### 4

    vi /etc/sudoers
    visudo
    $1 ALL=(ALL) NOPASSWD: /bin/chown


### 5

    cp /etc/shadow /home/user1/shadow_copy
    chown root:root /home/user1/shadow_copy
    chmod 600 /home/user1/shadow_copy
    su user1
    cat /home/user1/shadow_copy
    su -

    setfacl -m u:user1:r-- /home/user1/shadow_copy
    getfacl /home/user1/shadow_copy

    su user1

    cat /home/user1/shadow_copy

## Cviceni 3 Solaris

### 0
    mkdir /export/home/user1
    useradd -m -d /export/home/user1 -s /bin/pfksh user1
    passwd user1 # !1Thakurova
    mkdir /export/home/user2
    useradd -m -d /export/home/user2 -s /bin/pfksh user2
    passwd user2 # !1Thakurova
    useradd -m -d /export/home/user3 -s /bin/pfksh user3
    passwd user3 # !1Thakurova

### 1

    cp /etc/user_attr /export/home/
    vi /etc/user_attr # root::::type=role student::::roles=root 
    reboot # vyzkousej root prihlaseni
    ssh root@localhost # SSH mi nefunguje na Virtualboxu


### 2

    echo "read_any_file:R0:::" >> /etc/security/prof_attr.d/local-entries
    echo "read_any_file:solaris:cmd:R0::/usr/bin/less:euid=0" >> /etc/security/exec_attr.d/local-entries
    usermod -P read_any_file -s /usr/bin/pfbash user3
    su - user3
    profiles
    roles
    more /etc/shadow
    less /etc/shadow

### 3

    roleadd -m  -P read_any_file -s /usr/bin/pfbash reader
    passwd reader # !1Thakurova
    usermod -R  reader user2
    su - user2
    id
    roles
    less /etc/shadow
    cat /etc/shadow
    su - reader
    roles
    id
    less /etc/shadow
    cat /etc/shadow

### 7

    man ppriv
    su - student 
    ppriv -v $$
    su - root
    ppriv -s I+file_dac_read 1578
    su - student 
    ppriv -v $$
    cat /etc/shadow

## Cviceni 4 Linux CentOS

### 2

    cd /dev
    ls -l /dev/zero #cisla oddelena carkou major minor
    sudo mknod only_zeroes c 1 5
    dd if=only_zeroes bs=1k  count=32k of=/var/tmp/32MBzeroes
    od -cx /var/tmp/32MBzeroes


### 3

    3/ Create and attach 2nd virtual disk to CentOS VirtualBox virtual computers

    Shut down CentOS (if running).
    Choose CentOS in VirtualBox, Click na Settings and choose Storage.
    Click on Controller: SATA, then + (Add hard disk) and then Create Disk Image.
    Choose VMDK and Fixed Size. Use about 500MB a let it be created (Create).
    Confirm, start CentOs.
    lsblk
    ls -l /dev

### 4 


    fdisk /dev/sdb
    fdisk> n
    Partition type: p
    Partifiton number: 1
    First sector: default
    Last sector: +125M
    fdisk> n
    Partition type: p
    Partifiton number: 2
    First sector: default
    Last sector: +125M
    fdisk> n
    Partition type: p
    Partifiton number: 3
    First sector: default
    Last sector: +125M
    fdisk> n
    Partition type: p
    Partifiton number: 4
    First sector: default
    Last sector: +125M

    fdisk> w


### 5

    lsblk
    fdisk /dev/sda
    fdisk> n
    Partition type: p
    Partifiton number: 3
    First sector: default
    Last sector: +500M
    fdisk> n
    Partition type: p
    Partifiton number: 4
    First sector: default
    Last sector: +500M
    fdisk> w

    partprobe



### 6 


    sudo fdisk -l /dev/sdb
    sudo dd if=/dev/sdb of=jahodik.mbr bs=512 count=1
    xxd jahodik.mbr


## Cviceni 4 Solaris

### 2

    cd /dev
    ls -lL /dev/zero #cisla oddelena carkou major minor
    sudo mknod only_zeroes c 118 12
    dd if=only_zeroes bs=1k  count=32k of=/var/tmp/32MBzeroes
    od -cx /var/tmp/32MBzeroes


### 3

    3/ Create and attach 2nd virtual disk to CentOS VirtualBox virtual computers

    Shut down CentOS (if running).
    Choose CentOS in VirtualBox, Click na Settings and choose Storage.
    Click on Controller: SATA, then + (Add hard disk) and then Create Disk Image.
    Choose VMDK and Fixed Size. Use about 500MB a let it be created (Create).
    Confirm, start CentOs.
    ls -l /dev

### 4 

    format
    fdisk /dev/rdsk/c1t2d0p0

    fdisk> 1
    Select the partition type to create:
    1=SOLARIS2   2=UNIX      3=PCIXOS     4=Other        5=DOS12
    6=DOS16      7=DOSEXT    8=DOSBIG     9=DOS16LBA     A=x86 Boot
    B=Diagnostic C=FAT32     D=FAT32LBA   E=DOSEXTLBA    F=EFI (Protective)
    G=EFI_SYS    0=Exit? 1
    
    Specify the percentage of disk to use for this partition
    (or type "c" to specify the size in cylinders). 20
    
    Should this become the active partition? If yes, it will be 
    activated each time the computer is reset or turned on.
    Please type "y" or "n". n
 
    fdisk> 1
    Select the partition type to create:
    1=SOLARIS2   2=UNIX      3=PCIXOS     4=Other        5=DOS12
    6=DOS16      7=DOSEXT    8=DOSBIG     9=DOS16LBA     A=x86 Boot
    B=Diagnostic C=FAT32     D=FAT32LBA   E=DOSEXTLBA    F=EFI (Protective)
    G=EFI_SYS    0=Exit? 1
    
    Specify the percentage of disk to use for this partition
    (or type "c" to specify the size in cylinders). 20
    
    Should this become the active partition? If yes, it will be 
    activated each time the computer is reset or turned on.
    Please type "y" or "n". n

    fdisk> 1
    Select the partition type to create:
    1=SOLARIS2   2=UNIX      3=PCIXOS     4=Other        5=DOS12
    6=DOS16      7=DOSEXT    8=DOSBIG     9=DOS16LBA     A=x86 Boot
    B=Diagnostic C=FAT32     D=FAT32LBA   E=DOSEXTLBA    F=EFI (Protective)
    G=EFI_SYS    0=Exit? 1
    
    Specify the percentage of disk to use for this partition
    (or type "c" to specify the size in cylinders). 20
    
    Should this become the active partition? If yes, it will be 
    activated each time the computer is reset or turned on.
    Please type "y" or "n". n

    fdisk> 1
    Select the partition type to create:
    1=SOLARIS2   2=UNIX      3=PCIXOS     4=Other        5=DOS12
    6=DOS16      7=DOSEXT    8=DOSBIG     9=DOS16LBA     A=x86 Boot
    B=Diagnostic C=FAT32     D=FAT32LBA   E=DOSEXTLBA    F=EFI (Protective)
    G=EFI_SYS    0=Exit? 1
    
    Specify the percentage of disk to use for this partition
    (or type "c" to specify the size in cylinders). 20
    
    Should this become the active partition? If yes, it will be 
    activated each time the computer is reset or turned on.
    Please type "y" or "n". n


    fdisk> 6

    fdisk /dev/rdsk/c1t2d0p0



### 5 no Solaris only Centos


    df -h                   # find the name of the physical disk and the name of partition(s)
    lsblk /dev/nvme0n1
    fdisk /dev/nvme0n1      # create two new partitions (partitions p3 and p4 on this disk)
                            # use sub-commands "m, p (print), n (new, -> p (primary partition)),
                            # p (print)".... finally "w" (write)
                            # check for new devices in /dev/  (/dev/nvme0n1p3, p4)

    partprobe               # inform OS kernel about new partitions created (alternative is to reboot the system)


### 6 

    sudo fdisk -l /dev/rdsk/c1t2d0p0
    sudo dd if=/dev/sdb of=jahodik.mbr bs=512 count=1
    xxd jahodik.mbr


## Cviceni 5

### 1


    mkfs -t ext4 /dev/sda3
    mkdir /new1
    mount /dev/sda3 /new1 #pokud nefunguje zkusit vytvorit novou dir /new4
    mkfs -t ext4 /dev/sda4
    mkdir /new2
    mount /dev/sda4 /new2
    df -Th
    lsblk -o+FSTYPE /dev/sda3
    man fstab   # LINUX
    vi /etc/fstab   # add lines for /new1 and /new2 directories
    reboot


### 2
    
    
    pvcreate /dev/sda3 /dev/sda4      
    pvdisplay; pvs; pgscan
    vgcreate VG-BIE-ADU /dev/sda3 /dev/sda4   # create a new volume group
    vgdisplay; vgs; vgscan       # display the status of new volume group
    lvcreate -L 200m VG-BIE-ADU
    lvcreate -L 300m VG-BIE-ADU
    lvdisplay; lvs; lvscan

    mkfs -t ext4 /dev/VG-BIE-ADU/lvol0
    mkfs -t ext4 /dev/VG-BIE-ADU/lvol1
    mkdir /new1 /new2
    mount /dev/VG-BIE-ADU/lvol0 /new1
    mount /dev/VG-BIE-ADU/lvol1 /new2
    cp /etc/passwd /new1
    df -Th
    lsblk -o+FSTYPE /dev/VG-BIE-ADU/lvol0
    blkid 
    vi /etc/fstab # add lines for /new1 and /new2 directories
    reboot



    df -Th
    vgdisplay
    lvdisplay
    lvextend -L +50M /dev/VG-BIE-ADU/lvol0  # lvol0 is extended *by* 50MB
    lvreduce -L -50M --resizefs /dev/VG-BIE-ADU/lvol0  # lvol0 is reduced *by* 50MB
    df -h

    df -Th
    lvcreate --snapshot /dev/VG-BIE-ADU/lvol0 -L 200M -n snap1


    lvdisplay; lvs; lvscan
    vgdisplay
    vdisplay; vgs; vgscan
    umount /new1
    umount /new2
    lvchange -an /dev/VG-BIE-ADU/lvol0
    lvchange -an /dev/VG-BIE-ADU/lvol1
    lvremove /dev/VG-BIE-ADU/lvol0
    lvremove /dev/VG-BIE-ADU/lvol1
    vgremove VG-BIE-ADU
    pvremove /dev/nvme0n1p3 /dev/nvme0n1p4
    vgdisplay; vgs; vgscan
    lvdisplay; lvs; lvscan

### 3

    mkfs -t ext4 /dev/sda3
    mkdir /new1
    mount /dev/sda3 /new1
    cp /etc/passwd /new1
    umount /new1
    dd if=/dev/zero of=/dev/sda3 bs=8192 seek=1 count=1
    dd if=/dev/zero of=/dev/sda3 bs=8192 seek=32 count=1
    mount /new1
    
    fsck -b 32 /dev/sda3 #nefunguje
    fsck -p /dev/sda3



    mkfs -t ext4 /dev/sda4
    mkdir /new5/test
    echo "Content of f1" >/new5/test/f1
    echo "Content of f2" >/new5/test/f2
    ls -id /new5
    ls -i /new5
    umount /new5


    man clri
    clri    # na centOS nefunguje 
    fsck /dev/sda4
    mount /dev/sda4 /new5
    ls /new5/lost+found
    ls -lRi /new5

### 4

### A.0 preparation

    fdisk /dev/sda

    partprobe

    pvcreate /dev/sda3 /dev/sda4      
    pvdisplay; pvs; pgscan
    vgcreate VG0 /dev/sda3 /dev/sda4 
    lvcreate -L 100m VG-BIE-ADU 
    lvcreate -L 100m VG-BIE-ADU 
    lvcreate -L 100m VG-BIE-ADU 
    lvcreate -L 100m VG-BIE-ADU 
    lvcreate -L 100m VG-BIE-ADU 
    lvcreate -L 100m VG-BIE-ADU 
    lvcreate -L 100m VG-BIE-ADU 



### A.1
    lvdisplay VG0     # display logical volumes in volume group VG0
    mdadm -C /dev/md0 -l1 -n2 /dev/VG0/lvol0 /dev/VG0/lvol1
    mdadm -D /dev/md0
    mkfs -t ext3 /dev/md0
    mkdir /fs1
    mount /dev/md0 /fs1
    df -h

    mdadm --manage /dev/md0 --add-spare /dev/VG0/lvol2
    mdadm --manage /dev/md0 --fail /dev/VG0/lvol0

    mdadm -D /dev/md0
    mdadm --manage /dev/md0 --remove /dev/VG0/lvol0
    mdadm /dev/md0 -a /dev/VG0/lvol2
    mdadm -D /dev/md0


    umount /fs1
    mdadm --stop /dev/md0
    mdadm -Ds

### A.2

    mdadm -C -v /dev/md0 -l5 -n3 /dev/VG0/lvol3 /dev/VG0/lvol4 /dev/VG0/lvol5
    mdadm -D /dev/md0
    mkfs -t ext3 /dev/md0
    mount /dev/md0 /fs1
    df -h
    mdadm /dev/md0 -a /dev/VG0/lvol6
    mdadm --manage /dev/md0 --fail /dev/VG0/lvol4
    mdadm -D /dev/md0
    mdadm --manage /dev/md0 --remove /dev/VG0/lvol4
    mdadm -D /dev/md0

    umount /fs1
    mdadm --stop /dev/md0
    mdadm -Ds


### B
    sudo fdisk /dev/nvme0n1
    n
    p
    3

    +500M

    n
    e
    4

    +800M

    n
    l
    5

    +100M

    t
    5
    8e

    n
    l
    6

    +100M

    t
    6
    8e

    n
    l
    7

    +100M

    t
    7
    8e

    n
    l
    8

    +100M

    t
    8
    8e

    n
    l
    9

    +100M

    t
    9
    8e

    n
    l
    10

    +100M

    t
    10
    8e

    n
    l
    11

    +100M

    t
    11
    fd

    n
    l
    12

    +100M

    t
    12
    fd

    w
### B.1


    mdadm -C /dev/md0 -l1 -n2 /dev/sda5 /dev/sda6
    mdadm -D /dev/md0
    mkfs -t ext3 /dev/md0
    mkdir /fs1
    mount /dev/md0 /fs1
    df -h
    mdadm --manage /dev/md0 --fail /dev/sda6
    mdadm -D /dev/md0
    mdadm --manage /dev/md0 --remove /dev/sda6
    mdadm -D /dev/md0
    mdadm /dev/md0 -a /dev/sda11
    mdadm -D /dev/md0

    umount /fs1
    mdadm --stop /dev/md0
    mdadm -Ds

### B.2

    mdadm -C /dev/md0 -l1 -n2 /dev/sda5 /dev/sda6
    mdadm -C /dev/md1 -l1 -n2 /dev/sda7 /dev/sda8
    mdadm -C /dev/md2 -l1 -n2 /dev/sda9 /dev/sda10
    pvcreate /dev/md0 /dev/md1 /dev/md2
    vgcreate vg0 /dev/md0 /dev/md1 /dev/md2
    lvcreate -L 100M vg0
    mkfs -t ext4 /dev/vg0/lvol0
    mount /dev/vg0/lvol0 /fs1
    df -hT

    umount /fs1
    lvdestroy /dev/vg0/lvol0
    lvremove /dev/vg0/lvol0
    vgremove /dev/vg0
    mdadm --stop /dev/md0
    mdadm --stop /dev/md1
    mdadm --stop /dev/md2
    mdadm -Ds

    mdadm -C /dev/md0 -l5 -n3 /dev/sda5 /dev/sda6 /dev/sda7
    mdadm -C /dev/md1 -l5 -n3 /dev/sda8 /dev/sda9 /dev/sda10
    pvcreate /dev/md0
    pvcreate /dev/md1
    vgcreate vg0 /dev/md0 /dev/md1
    lvcreate -L 100m vg0 

## Cviceni 6 Solaris


### 0 preparation

    Run VirtualBox, but do not start virtual Solaris machine
    Create and attach one more virtual disk
    (SOLARIS/settings/storage/add_hard_disk/create_new_disk/VMDK/fixed_size - name: Solaris-disk002, size approx. 2GB)


    format
    fdisk /dev/rdsk/c1t3d0p0

    fdisk> 1
    Select the partition type to create:
    1=SOLARIS2   2=UNIX      3=PCIXOS     4=Other        5=DOS12
    6=DOS16      7=DOSEXT    8=DOSBIG     9=DOS16LBA     A=x86 Boot
    B=Diagnostic C=FAT32     D=FAT32LBA   E=DOSEXTLBA    F=EFI (Protective)
    G=EFI_SYS    0=Exit? 1
    
    Specify the percentage of disk to use for this partition
    (or type "c" to specify the size in cylinders). 20
    
    Should this become the active partition? If yes, it will be 
    activated each time the computer is reset or turned on.
    Please type "y" or "n". n

    fdisk> 1
    Select the partition type to create:
    1=SOLARIS2   2=UNIX      3=PCIXOS     4=Other        5=DOS12
    6=DOS16      7=DOSEXT    8=DOSBIG     9=DOS16LBA     A=x86 Boot
    B=Diagnostic C=FAT32     D=FAT32LBA   E=DOSEXTLBA    F=EFI (Protective)
    G=EFI_SYS    0=Exit? 1
    
    Specify the percentage of disk to use for this partition
    (or type "c" to specify the size in cylinders). 20
    
    Should this become the active partition? If yes, it will be 
    activated each time the computer is reset or turned on.
    Please type "y" or "n". n

    fdisk> 1
    Select the partition type to create:
    1=SOLARIS2   2=UNIX      3=PCIXOS     4=Other        5=DOS12
    6=DOS16      7=DOSEXT    8=DOSBIG     9=DOS16LBA     A=x86 Boot
    B=Diagnostic C=FAT32     D=FAT32LBA   E=DOSEXTLBA    F=EFI (Protective)
    G=EFI_SYS    0=Exit? 1
    
    Specify the percentage of disk to use for this partition
    (or type "c" to specify the size in cylinders). 20
    
    Should this become the active partition? If yes, it will be 
    activated each time the computer is reset or turned on.
    Please type "y" or "n". n

    fdisk> 1
    Select the partition type to create:
    1=SOLARIS2   2=UNIX      3=PCIXOS     4=Other        5=DOS12
    6=DOS16      7=DOSEXT    8=DOSBIG     9=DOS16LBA     A=x86 Boot
    B=Diagnostic C=FAT32     D=FAT32LBA   E=DOSEXTLBA    F=EFI (Protective)
    G=EFI_SYS    0=Exit? 1
    
    Specify the percentage of disk to use for this partition
    (or type "c" to specify the size in cylinders). 20
    
    Should this become the active partition? If yes, it will be 
    activated each time the computer is reset or turned on.
    Please type "y" or "n". n

    fdisk> 6

    fdisk /dev/rdsk/c1t3d0p0
    mkdir /var/tmp/disks
    for i in 1 2 3 4 5 6 7 8 9 10
    do
        mkfile 200m /var/tmp/disks/disk$i
    done

### 1

    zpool create pool1  /dev/dsk/c1t3d0p1 /dev/dsk/c1t3d0p2 # use block devices
    zpool create pool2  /var/tmp/disks/disk1 /var/tmp/disks/disk2
    df -h | grep pool
    zpool list
    zpool status
    zpool add -f pool1 /var/tmp/disk3  # adding file to a pool from partitions (option *-f* is needed)

    zpool status
    zpool add -f pool2 /dev/dsk/c1t3d0p3 # adding a partition to a pool created from files (option *-f* is needed)

    zpool status
    zpool history
    zpool destroy pool1 pool2


### 2
    
    zpool create pool1 mirror /dev/dsk/c1t3d0p1 /dev/dsk/c1t3d0p2
    zpool create pool2 raidz /var/tmp/disks/disk1 /var/tmp/disks/disk2 /var/tmp/disks/disk3
    zpool add -f pool2 raidz /var/tmp/disks/disk4 /var/tmp/disks/disk5
    zpool status
    zpool destroy pool1
    zpool create -f pool1 /var/tmp/disks/disk6 mirror /var/tmp/disks/disk7 /var/tmp/disks/disk8
    zpool add -f pool1 raidz /var/tmp/disks/disk9 /var/tmp/disks/disk10
    zpool status pool1 

### 3
    
    zpool get all pool1
    zfs create pool1/fs1
    zfs create pool1/fs1/fs3
    zfs get all pool1/fs1/fs3
    zfs set quota=50mb pool1/fs1
    zfs create pool1/fs2
    zfs set mountpoint=/fs2 pool1/fs2
    df -k
    zfs list

### 4

    zpool destroy pool2
    zpool create pool2 mirror /dev/dsk/c1t3d0p1 /dev/dsk/c1t3d0p2
    zpool detach pool2 /dev/dsk/c1t3d0p2
    df -h; zpool status
    zpool add -f pool2 /dev/dsk/c1t3d0p2
    df -h; zpool status
    zpool attach pool2 /dev/dsk/c1t3d0p1 /dev/dsk/c1t3d0p3
    zpool attach pool2 /dev/dsk/c1t3d0p2 /dev/dsk/c1t3d0p4
    df -h; zpool status

## Cviceni 7

    #For some reason am not able to run the lab on my VMs however I can run it differt way. At least they can ping each other.

    #Virtual host adapter dhcp
    #Host only network - Virtual host adapter on both VMs

    #On solaris enable dhcp 
    ipadm create-addr -T dhcp net0/dhcp
    ipadm show-addr

    # now they can ping between each other

## Cviceni 8

    #I am not able to do the lab because I do not have the lab 7 done correctly.




## Cviceni 9

### 0

    groupadd lab9
    useradd -m -g lab9 lab9user1
    passwd lab9user1 #thakurova22
    useradd -m -g lab9 lab9user2
    passwd lab9user1 #thakurova22
    useradd -m -g lab9 lab9user3
    passwd lab9user1 #thakurova22

### 2

    crontab -l > crontab_backup.txt
    crontab -e # * * * * * echo "1 minute passed" > /dev/tty6
    echo "sudo reboot" | at 16:15
    atq
    atrm 1
    vi /etc/cron.deny #lab9user1
    vi /etc/at.deny #lab9user1
    su - lab9user1
    crontab -e


### 3

    mkdir /newroot    # create a directory for new root
    cd /newroot
    mkdir bin lib lib64

    ldd /bin/ls   # list dynamic dependencies (shared libraries) for executables to be used in /newroot
    ldd /bin/date
    ldd /bin/bash
    ldd /bin/who
    ldd /bin/ps
3
    cd /usr/lib
    find . | cpio -pVdum /newroot/lib # copy all libraries from */lib* with all atributes (owner, access rights, times stamps, ... )

    cd /usr/lib64
    find . | cpio -pVdum /newroot/lib64 # copy all libraries from */lib64* with all atributes (owner, access rights, times stamps, ... )

    chroot /newroot   # change root for */newroot*
    ls
    who
    date
    cd /etc         # why this does not work?

    ctrl+d

### 4

    ulimit -a
    :(){ :|:& };:

    vi /etc/security/limits.conf # *    hard    nproc   500
    reboot
    su student
    ulimit -a

    # ulimit -S -u 500     
    
    :(){ :|:& };:
    ctrl+c

## Cviceni 10

### 1.2

    ps -ef | grep sshd
    kill -9 508
    svcs -a | grep ssh
    tail -f `svcs -L network/ssh` # run in a different terminal window
    svcadm disable network/ssh
    vi /etc/ssh/sshd_config #Port 22222
    svcadm enable network/ssh
    svcadm restart network/ssh
    netstat -an | grep 22222

### 1.3

    ls -li /etc/init.d
    ls -li /etc/rc2.ds
    cd /etc/init..d
    cp acct jahodvik
    vi jahodvik  

        exec >> /var/log/zemanek.log 2>&1

        case "$1" in
        start)
            exec >/dev/pts/2
            echo "Service nfs/client is being started."
            banner "START"
            banner `date +%R`
            sleep 3
            # Start NFS client service
            svcadm enable -s svc:/network/nfs/client:default
            ;;
        stop)
            exec >/dev/pts/2
            echo "Service nfs/client is being stopped."
            banner "STOP"
            banner `date +%R`
            sleep 3
            # Stop NFS client service
            svcadm disable -s svc:/network/nfs/client:default
            ;;
        *)
            echo "Usage: $0 {start|stop}"
            exit 1
            ;;
        esac

        exit 0


    ln /etc/init.d/jahodvik /etc/rc2.d/S99jahodvik
    ln /etc/init.d/jahodvik /etc/rc0.d/K05jahodvik

### 1.4

    touch /var/log/jahodvik.log
    init 6
    svcs -a | grep jahodvik
    reboot # obejde init procesy a komunikuje primo s krenelem a nespousti tak /etc/rc
    svcs -a | grep jahodvik
