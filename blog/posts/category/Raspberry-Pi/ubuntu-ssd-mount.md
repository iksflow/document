# Ubuntu ssd 마운트

## 1. 디스크 이름 확인
라즈베리파이에 SSD를 연결한 다음 sudo fdisk -l 명령어를 입력합니다.  
실행 결과에서 가장 아래로 내려보면 방금 추가한 SSD의 장치명인 `/dev/sda1`을 확인할 수 있습니다.  
참고로 제가 연결한 장치는 `Samsung T7` 이라는 500GB 외장 SSD입니다.  
```
iksflow@iksflow-desktop:~$ sudo fdisk -l
(생략)
Disk /dev/sda: 465.76 GiB, 500107862016 bytes, 976773168 sectors
Disk model: PSSD T7
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 33553920 bytes
Disklabel type: dos
Disk identifier: 0x655e5554

Device     Boot Start       End   Sectors   Size Id Type
/dev/sda1        2048 976770112 976768065 465.8G  7 HPFS/NTFS/exFAT
```

## 2. 디스크 정보 확인
추가한 장치의 파일 시스템을 확인합니다.
blkid 명령어로 장치의 파일 시스템정보 및 UUID 등 속성 확인 
lsblk 명령어로 마운트 경로 확인
```
iksflow@iksflow-desktop:~$ sudo blkid
/dev/mmcblk0p1: LABEL_FATBOOT="system-boot" LABEL="system-boot" UUID="D7A9-3EE6" BLOCK_SIZE="512" TYPE="vfat" PARTUUID="2da91795-01"
/dev/mmcblk0p2: LABEL="writable" UUID="09799e9f-8009-4b0c-84bc-9761d73d9670" BLOCK_SIZE="4096" TYPE="ext4" PARTUUID="2da91795-02"
/dev/loop1: TYPE="squashfs"
/dev/loop6: TYPE="squashfs"
/dev/loop4: TYPE="squashfs"
/dev/loop2: TYPE="squashfs"
/dev/loop0: TYPE="squashfs"
/dev/loop7: TYPE="squashfs"
/dev/loop5: TYPE="squashfs"
/dev/loop3: TYPE="squashfs"
/dev/sda1: LABEL="T7" UUID="A40C-76DB" BLOCK_SIZE="512" TYPE="exfat" PARTUUID="655e5554-01"
iksflow@iksflow-desktop:~$ sudo lsblk
NAME        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
loop0         7:0    0  57.4M  1 loop /snap/core20/1408
loop1         7:1    0     4K  1 loop /snap/bare/5
loop2         7:2    0 147.5M  1 loop /snap/firefox/1233
loop3         7:3    0 228.4M  1 loop /snap/gnome-3-38-2004/100
loop4         7:4    0  81.3M  1 loop /snap/gtk-common-themes/1534
loop5         7:5    0  44.3M  1 loop /snap/snap-store/576
loop6         7:6    0  37.8M  1 loop /snap/snapd/15183
loop7         7:7    0   272K  1 loop /snap/snapd-desktop-integration/11
sda           8:0    0 465.8G  0 disk
└─sda1        8:1    0 465.8G  0 part
mmcblk0     179:0    0  29.7G  0 disk
├─mmcblk0p1 179:1    0   256M  0 part /boot/firmware
└─mmcblk0p2 179:2    0  29.5G  0 part /
```


## 3. 마운트
위에서 확인한 정보를 fstab으로 마운트.
재부팅 시에도 자동 적용된다.
입력 후 재부팅

### 3.1 마운트 디렉터리 생성
마운트할 디렉터리를 생성한다.
나는 Workspace라는 이름의 디렉터리를 생성했다.
```
iksflow@iksflow-desktop:~$ sudo mkdir ~/Workspace
```

```
iksflow@iksflow-desktop:~$ sudo vi /etc/fstab

[내용]
LABEL=writable  /       ext4    discard,x-systemd.growfs        0       1
LABEL=system-boot       /boot/firmware  vfat    defaults        0       1
PARTUUID="655e5554-01"  /home/iksflow/Workspace exfat   defaults,noatime        0       0

iksflow@iksflow-desktop:~$ sudo shutdown -r now
Connection to 192.168.0.11 closed by remote host.
Connection to 192.168.0.11 closed.
```

```
iksflow@iksflow-desktop:~$ sudo lsblk
NAME        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
loop0         7:0    0     4K  1 loop /snap/bare/5
loop1         7:1    0  57.4M  1 loop /snap/core20/1408
loop2         7:2    0 147.5M  1 loop /snap/firefox/1233
loop3         7:3    0 228.4M  1 loop /snap/gnome-3-38-2004/100
loop4         7:4    0  81.3M  1 loop /snap/gtk-common-themes/1534
loop5         7:5    0  44.3M  1 loop /snap/snap-store/576
loop6         7:6    0  37.8M  1 loop /snap/snapd/15183
loop7         7:7    0   272K  1 loop /snap/snapd-desktop-integration/11
sda           8:0    0 465.8G  0 disk
└─sda1        8:1    0 465.8G  0 part /home/iksflow/Workspace
mmcblk0     179:0    0  29.7G  0 disk
├─mmcblk0p1 179:1    0   256M  0 part /boot/firmware
└─mmcblk0p2 179:2    0  29.5G  0 part /
```

4. 권한
fatal: could not create work tree dir

RUN printf "deb http://ports.ubuntu.com/ubuntu-ports/ jessie main\ndeb-src http://ports.ubuntu.com/ubuntu-ports/ jessie main\ndeb http://ports.ubuntu.com/ubuntu-ports/ jessie/updates main\ndeb-src http://ports.ubuntu.com/ubuntu-ports/ jessie/updates main" > /etc/apt/sources.list

RUN printf "deb http://archive.debian.org/debian/ jessie main\ndeb-src http://archive.debian.org/debian/ jessie main\ndeb http://security.debian.org jessie/updates main\ndeb-src http://security.debian.org jessie/updates main" > /etc/apt/sources.list



