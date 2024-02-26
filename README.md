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
    chown user1 /home/user1


### 7.2

    useradd -u 2003 -m -d /home/user3 user3
    useradd -u 2004 -m -d /home/user4 user4
    groupadd -g 102 adu2
    usermod -ag adu2 user3
    usermod -aG adu2 user4

### 8

    authconfig --passalgo=sha256 --update

### 9

    vi /etc/login.defs 
    PASS_MIN_DAYS=7

    vi /etc/pam.d/system-auth
    password sufficient pam_unix.so use_authtok md5 shadow remember=13

    [ ! -f /etc/security/opasswd ] && touch /etc/security/opasswd

    vi /etc/security/pwquality.conf 

## Cviceni 2


### 1

    echo "$1:x:$2:101::/home/$1:/bin/bash" >> /etc/passwd
    echo "$1:!!:19732:0:99999:7:::" >> /etc/shadow
    mkdir /home/$1
    cp ~/.bashrc /home/$1
    cp ~/.profile /home/$1
    chown -R $1 /home/$1
    chmod $1 /home/$1

### 4

    vi /etc/sudoers
    visudo
    $1 ALL=(ALL) NOPASSWD: /bin/chown


### 5

