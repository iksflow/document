
# 파티션 생성
fdisk - manipulate disk partition table

## fdisk -l 
지정한 디스크 파티 파티션 정보 출력
예제)
### fdisk -l /dev/sda1
/dev/sda1 장치의 파티션 정보를 출력한다

### fdisk -s /dev/sda1
/dev/sda1 장치의 파티션 크기를 출력한다. 단위는 block이다.

## fdisk /dev/sda1
/dev/sda1 장치의 파티션을 생성한다
n: 파티션 생성
d: 파티션 삭제
w: 변경 정보 저장
p: 파티션 정보 확인
t: 파티션 속성 변경


# 파일시스템 생성
mkfs

mk2efs, mkfs.vfat, mkfs.xfs 등등 파일시스템 별로 생성하는 명령어가 있다.

파일 시스템이 생서된 장치의 UUID확인
blkid 장치명
예) /dev/sda1의 UUID를 확인한다
blkid /dev/sda1

# LVM (Logical Volume Manager)
리눅스의 저장 공간을 효율적으로 관리하기 위한 커널의 일부분.

물리적 볼륨 / PV (Physical Volume)
- 실제 디스크 장치를 분할한 파티션된 상태를의미한다.
- PV는 일정한 크기의 PE들로 구성된다.

물리적 확장 / PE (Physical Extent)
- PV를 구성하는 일정한 크기의 Block.
- 보통 1PE는 4MB에 해당한다.
- PE와 LE는 1:1로 대응한다.

볼륨 그룹 / VG (Volume Group)
- PV들이 모여서 생성되는 단위이다. (모든걸 합친 거대한 지점토 덩어리의 느낌이다)
- 사용자는 VG를 원하는대로 쪼개서 LV로 만들게 된다.

논리적 볼륨 / LV (Logical Volume)
- 사용자가 최종적으로 사용하는 단위로, VG에서 필요한 크기로 할당받아 LV를 생성한다.

간단한 실습을 진행해 보자!

lsblk : 블록 스토리지 확인

# 파티션 생성
디스크의 파티션을 생성한다.
fdisk [장치명]

확장 파티션 생성
e.g.
```
root@iksflow-desktop:/# fdisk /dev/sda

Welcome to fdisk (util-linux 2.37.2).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.
Command (m for help): 

# 파티션 목록을 출력한다
Command (m for help): i
No partition is defined yet!

# 파티션을 생성한다
Command (m for help): n
# 파티션 타입을 정한다. 부팅할 떄 사용하려면 p(주파티션) 아니라면 e(확장파티션)으로 선택한다.
Partition type
   p   primary (0 primary, 0 extended, 4 free)
   e   extended (container for logical partitions)
Select (default p): e
# 파티션 번호를 선택한다. 엔터치면 기본값으로 설정된다
Partition number (1-4, default 1):
# 파티션의 용량을 정하는 부분이다. first,last 범위에 따라 파티션의 용량이 결정된다
First sector (2048-976773167, default 2048):
Last sector, +/-sectors or +/-size{K,M,G,T,P} (2048-976773167, default 976773167):

Created a new partition 1 of type 'Extended' and of size 465.8 GiB.
Partition #1 contains a ext4 signature.

# 기존에 시그니처가 있다면 없앤다.
Do you want to remove the signature? [Y]es/[N]o: y

The signature will be removed by a write command.

# 파티션 목록을 출력한다
Command (m for help): i
Selected partition 1
         Device: /dev/sda1
          Start: 2048
            End: 976773167
        Sectors: 976771120
      Cylinders: 60802
           Size: 465.8G
             Id: 5
           Type: Extended
    Start-C/H/S: 0/32/33
      End-C/H/S: 385/80/63

Command (m for help): w
The partition table has been altered.
Calling ioctl() to re-read partition table.
Syncing disks.

```

논리 파티션 생성
```
Command (m for help): n

All space for primary partitions is in use.
Adding logical partition 5
First sector (4096-976773167, default 4096):
Last sector, +/-sectors or +/-size{K,M,G,T,P} (4096-976773167, default 976773167):

Created a new partition 5 of type 'Linux' and of size 465.8 GiB.

Command (m for help): w
The partition table has been altered.
Calling ioctl() to re-read partition table.
Syncing disks.

# fdisk -l로 논리파티션 /dev/sda5 가 생성되었는지 확인한다.
root@iksflow-desktop:/#fdisk -l
..생략..
Device     Boot Start       End   Sectors   Size Id Type
/dev/sda1        2048 976773167 976771120 465.8G  5 Extended
/dev/sda5        4096 976773167 976769072 465.8G 83 Linux 


# cat /proc/partitions 명령으로도 파티션 현황을 확인할 수 있다.
root@iksflow-desktop:/# cat /proc/partitions
major minor  #blocks  name

   7        0          4 loop0
   7        1      60456 loop1
   7        2      60408 loop2
   7        4     219128 loop4
   7        5     338532 loop5
   7        6     338560 loop6
   7        7      83212 loop7
 179        0   31166976 mmcblk0
 179        1     262144 mmcblk0p1
 179        2   30903791 mmcblk0p2
   8        0  488386584 sda
   8        1          1 sda1
   8        5  488384536 sda5
   7        8      93888 loop8
   7        9      37980 loop9
   7       10      45416 loop10
   7       11      45416 loop11
   7       12     218388 loop12
   7       13      42532 loop13
   7       14        272 loop14
   7       15        276 loop15
   7       16      44028 loop16

# 생성한 논리파티션을 ext4 파일시스템으로 포맷한다.
root@iksflow-desktop:/# mkfs.ext4 /dev/sda5
mke2fs 1.46.5 (30-Dec-2021)
Creating filesystem with 122096134 4k blocks and 30531584 inodes
Filesystem UUID: 1cc41032-6faa-42dd-bac5-88064f38f3c5
Superblock backups stored on blocks:
	32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208,
	4096000, 7962624, 11239424, 20480000, 23887872, 71663616, 78675968,
	102400000

Allocating group tables: done
Writing inode tables: done
Creating journal (262144 blocks): done
Writing superblocks and filesystem accounting information: done

# 포맷한 파티션을 마운트 한다. 마운트 경로(/home/iksflow/workspace)는 미리 생성되어있어야한다.
root@iksflow-desktop:/# mount -t ext4 /dev/sda5 /home/iksflow/workspace

# df명령어로 마운트 되었는지 확인한다. 
root@iksflow-desktop:/# df -h
Filesystem      Size  Used Avail Use% Mounted on
tmpfs           781M  3.7M  778M   1% /run
/dev/mmcblk0p2   29G   13G   16G  46% /
tmpfs           3.9G     0  3.9G   0% /dev/shm
tmpfs           5.0M  4.0K  5.0M   1% /run/lock
/dev/mmcblk0p1  253M  126M  128M  50% /boot/firmware
tmpfs           781M   76K  781M   1% /run/user/127
tmpfs           781M   68K  781M   1% /run/user/1000
/dev/sda5       458G   28K  435G   1% /home/iksflow/workspace
```
