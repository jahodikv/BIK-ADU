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

## Cviceni 3

### 1

    mkdir /export/home/user1
    useradd -m -d /export/home/user1 -s /bin/pfksh user1
    passwd user1
    mkdir /export/home/user2
    useradd -m -d /export/home/user2 -s /bin/pfksh user2
    passwd user2
    tar cvf backup.tar /etc/user_attr
    vi /etc/user_attr
    ssh root@localhost

### 2

    echo "super_reader::::" >> /etc/security/prof_attr
    echo "super_reader:suser:cmd:::/usr/bin/more:euid=0" >> /etc/security/exec_attr
    usermod -P super_reader user1

### 3

    roleadd -m -d /export/home/ctenar -P super_reader -s /bin/pfksh ctenar
    passwd ctenar
    usermod -R ctenar user1

### 6

    man ppriv

## Cviceni 4 Linux

### 2

    cd /dev/zero
    ls -l /dev/zero
    sudo mknod only_zeroes c 1 5
    dd if=only_zeroes bs=1k  count=32k of=/var/tmp/32MBzeroes
    od -cx /var/tmp/32MBzeroes


### 3

    3/ Create and attach 2nd virtual disk to Solaris VirtualBox virtual computers

    Shut down Solaris (if running).
    Choose Solaris in VirtualBox, Click na Settings and choose Storage.
    Click on Controller: SATA, then + (Add hard disk) and then Create Disk Image.
    Choose VMDK and Fixed Size. Use about 500MB a let it be created (Create).
    Confirm, start Solaris.
    ls -l /dev

### 4 

    fdisk /dev/rdsk/c1t2d0p0

    fdisk> 1
    Do you wish to create a partition on /dev/rdsk/c1t2d0p0? (Y/N): Y
    Enter partition number (1-4): 1
    Enter starting cylinder (default 0): <Press Enter>
    Enter partition size in % (1-100%) or kilobytes (xxK) or megabytes (xxM) or gigabytes (xxG): 20%


    fdisk> 1
    Do you wish to create a partition on /dev/rdsk/c1t2d0p0? (Y/N): Y
    Enter partition number (1-4): 2
    Enter starting cylinder (default <next available>): <Press Enter>
    Enter partition size in % (1-100%) or kilobytes (xxK) or megabytes (xxM) or gigabytes (xxG): 20%

    fdisk> 1
    Do you wish to create a partition on /dev/rdsk/c1t2d0p0? (Y/N): Y
    Enter partition number (1-4): 3
    Enter starting cylinder (default <next available>): <Press Enter>
    Enter partition size in % (1-100%) or kilobytes (xxK) or megabytes (xxM) or gigabytes (xxG): 20%

    fdisk> 1
    Do you wish to create a partition on /dev/rdsk/c1t2d0p0? (Y/N): Y
    Enter partition number (1-4): 4
    Enter starting cylinder (default <next available>): <Press Enter>
    Enter partition size in % (1-100%) or kilobytes (xxK) or megabytes (xxM) or gigabytes (xxG): 20%

    fdisk> 6

    fdisk /dev/rdsk/c1t2d0p0



### 5

    df -h                   # find the name of the physical disk and the name of partition(s)
    lsblk /dev/nvme0n1
    fdisk /dev/nvme0n1      # create two new partitions (partitions p3 and p4 on this disk)
                            # use sub-commands "m, p (print), n (new, -> p (primary partition)),
                            # p (print)".... finally "w" (write)
                            # check for new devices in /dev/  (/dev/nvme0n1p3, p4)

    partprobe               # inform OS kernel about new partitions created (alternative is to reboot the system)


### 6 

    # List MBR content
    sudo fdisk -l /dev/sdX

    # Dump MBR content into a file
    sudo dd if=/dev/sdX of=your_surname.mbr bs=512 count=1

    # Display hexadecimal content of the file
    xxd your_surname.mbr


## Cviceni 4 Solaris


## Cviceni 5

### 1

    df -h                   # find the name of the physical disk and the name of partition(s)
    fdisk /dev/nvme0n1      # create two new partitions (partitions p3 and p4 on this disk)
                            # use sub-commands "m, p (print), n (new, -> p (primary partition)),
                            # p (print)".... finally "w" (write)
                            # check for new devices in /dev/  (/dev/nvme0n1p3, p4)

    partprobe               # inform OS kernel about new partitions created (alternative is to reboot the system)

    mkfs -t ext4 /dev/nvme0n1p3
    mkdir /new1
    mount /dev/nvme0p3 /new1
    mkfs -t ext4 /dev/nvme0n1p4
    mkdir /new2
    mount /dev/nvme0p4 /new2
    df -Th
    lsblk -o+FSTYPE /dev/nvme0n1p3
    man fstab   # LINUX
    vi /etc/fstab   # add lines for /new1 and /new2 directories
    touch /new1/f1
    fssnap –o bs=/var/tmp /new1
    mkdir /new1snap
    mount -o ro /dev/fssnap/0 /new1snap
    ls -l /new1snap


### 2
    
    lsblk /dev/nvme0n1
    pvcreate /dev/nvme0n1p3 /dev/nvme0n1p4      # initialize new physical volumes /dev/nvme0n1p3 and /dev/nvme0n1p4
    pvdisplay; pvs; pgscan
    vgcreate VG-BIE-ADU /dev/nvme0n1p3 /dev/nvme0n1p4   # create a new volume group
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

    df -Th
    vgdisplay
    lvdisplay
    lvextend -L +50M /dev/VG-BIE-ADU/lvol0  # lvol0 is extended *by* 50MB
    resize2fs /dev/VG-BIE-ADU/lvol0
    df -h

    df -Th
    lvcreate -s -L 200M -n snap1 VG-BIE-ADU
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

    newfs /dev/lofi/1
    mkdir /new1
    mount /dev/lofi/1 /new1
    cp /etc/passwd /new1
        umount /new1
    dd if=/dev/zero of=/dev/rlofi/1 bs=8192 seek=1 count=1
    dd if=/dev/zero of=/dev/rlofi/1 bs=8192 seek=32 count=1
    mount /new1
    fstyp /dev/rlofi/1
    fsck -o b=32 /dev/rlofi/1

    mkdir /new1/dir1
    echo "Content of f1" >/new/dir1/f1
    echo "Content of f2" >/new/dir1/f2
    ls -id /new1
    ls -i /new1
    umount /new1
    man clri
    clri ..............   # please not that this is a dangerous command
    mount .............   # this should not be possible
    fsck ..............
    mount .............
    ls /new/lost+found
    ls -lRi /new1

### 4

#### A.1
    lvdisplay VG0     # display logical volumes in volume group VG0
    mdadm -C /dev/md0 -l1 -n2 /dev/VG0/lv1 /dev/VG0/lv2
    mdadm -D /dev/md0
    mkfs -t ext3 /dev/md0
    mkdir /fs1
    mount /dev/md0 /fs1
    df -h
    mdadm --manage /dev/md0 --fail /dev/VG0/lv1
    mdadm --manage /dev/md0 --remove /dev/VG0/lv1
    mdadm /dev/md0 -a /dev/VG0/lv3
    mdadm -D /dev/md0


    umount /fs1
    mdadm --stop /dev/md0
    mdadm -Ds

#### A.1

    mdadm -C -v /dev/md0 -l5 -n3 /dev/VG0/lv4 /dev/VG0/lv5 /dev/VG0/lv6
    mdadm -D /dev/md0
    mkfs ....
    mount ......
    df -h
    mdadm /dev/md0 -a /dev/VG0/lv7
    mdadm --manage … --fail
    mdadm -D /dev/md0
    mdadm --manage … --remove
    mdadm -D /dev/md0

    umount /fs1
    mdadm --stop ....
    mdadm -Ds


#### B
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
#### B.1


    mdadm -C /dev/md0 -l1 -n2 /dev/nvme0n1p5 /dev/nvme0n1p6
    mdadm -D /dev/md0
    mkfs -t ext3 /dev/md0
    mkdir /fs1
    mount /dev/md0 /fs1
    df -h
    mdadm --manage /dev/md0 --fail /dev/nvme0n1p6
    mdadm -D /dev/md0
    mdadm --manage /dev/md0 --remove /dev/nvme0n1p6
    mdadm -D /dev/md0
    mdadm /dev/md0 -a /dev/nvme0n1p11
    mdadm -D /dev/md0

    umount /fs1
    mdadm --stop /dev/md0
    mdadm -Ds

#### B.2

    mdadm -C /dev/md0 -l1 -n2 /dev/nvme0n1p5 /dev/nvme0n1p6
    mdadm -C /dev/md1 -l1 -n2 /dev/nvme0n1p7 /dev/nvme0n1p8
    mdadm -C /dev/md2 -l1 -n2 /dev/nvme0n1p9 /dev/nvme0n1p10
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

    mdadm -C /dev/md0 -l5 -n3 /dev/nvme0n1p5 /dev/nvme0n1p6 /dev/nvme0n1p7
    mdadm -C /dev/md1 -l5 -n3 /dev/nvme0n1p8 /dev/nvme0n1p9 /dev/nvme0n1p10
    pvcreate /dev/md0
    pvcreate /dev/md1
    vgcreate vg0 /dev/md0 /dev/md1


    mdadm -a /dev/md0 -l5 -n3 /dev/nvme0n1p11

### 5

#### 0a

    df -h                   # find the name of the physical disk and the name of partition(s)
    fdisk /dev/nvme0n1      # create two new partitions (partitions p3 and p4 on this disk)
                            # use sub-commands "m, p (print), n (new, -> p (primary partition)),
                            # p (print)".... finally "w" (write)
                            # check for new devices in /dev/  (/dev/nvme0n1p3, p4)

    partprobe               # inform OS kernel about new partitions created (alternative is to reboot the system)
#### 1
    mkfs -t ext4 /dev/nvme0n1p3
    mkdir /new1
    mount /dev/nvme0p3 /new1
    mkfs -t ext4 /dev/nvme0n1p4
    mkdir /new2
    mount /dev/nvme0p4 /new2
    df -Th
    lsblk -o+FSTYPE /dev/nvme0n1p3
    man fstab   # LINUX
    vi /etc/fstab   # add lines for /new1 and /new2 directories

