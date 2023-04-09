# Ubuntu 도커 설치

1. 옛날 버전 삭제하기
```
iksflow@iksflow-desktop:~$ sudo apt-get remove docker docker-engine docker.io containerd runc
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
E: Unable to locate package docker-engine
```

2. 레포지토리 설정

2.1 패키지 설치하기
```
iksflow@iksflow-desktop:~$ sudo apt-get update
Hit:1 http://ports.ubuntu.com/ubuntu-ports jammy InRelease
Get:2 http://ports.ubuntu.com/ubuntu-ports jammy-updates InRelease [109 kB]
Get:3 http://ports.ubuntu.com/ubuntu-ports jammy-backports InRelease [99.8 kB]
Get:4 http://ports.ubuntu.com/ubuntu-ports jammy-security InRelease [110 kB]
Get:5 http://ports.ubuntu.com/ubuntu-ports jammy-updates/main arm64 Packages [322 kB]
Get:6 http://ports.ubuntu.com/ubuntu-ports jammy-updates/main Translation-en [80.8 kB]
Get:7 http://ports.ubuntu.com/ubuntu-ports jammy-updates/main arm64 DEP-11 Metadata [90.8 kB]
Get:8 http://ports.ubuntu.com/ubuntu-ports jammy-updates/main arm64 c-n-f Metadata [5,920 B]
Get:9 http://ports.ubuntu.com/ubuntu-ports jammy-updates/restricted Translation-en [32.8 kB]
Get:10 http://ports.ubuntu.com/ubuntu-ports jammy-updates/universe arm64 DEP-11 Metadata [94.5 kB]
Get:11 http://ports.ubuntu.com/ubuntu-ports jammy-backports/universe arm64 DEP-11 Metadata [12.5 kB]
Get:12 http://ports.ubuntu.com/ubuntu-ports jammy-security/main arm64 DEP-11 Metadata [11.4 kB]
Fetched 970 kB in 4s (260 kB/s)
Reading package lists... Done

iksflow@iksflow-desktop:~$ sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
ca-certificates is already the newest version (20211016).
ca-certificates set to manually installed.
gnupg is already the newest version (2.2.27-3ubuntu2).
gnupg set to manually installed.
lsb-release is already the newest version (11.1.0ubuntu4).
lsb-release set to manually installed.
(생략)
```

2.2 도커 공식 GPG key 추가
```
iksflow@iksflow-desktop:~$ sudo mkdir -p /etc/apt/keyrings
iksflow@iksflow-desktop:~$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

2.3 레포지토리 설정
```
iksflow@iksflow-desktop:~$ echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```


3. 도커 엔진 설치

3.1 업데이트
apt update를 입력하면 앞서 수행한 작업으로 docker 관련된 경로가 새롭게 추가된 것을 확인할 수 있다.
```
iksflow@iksflow-desktop:~$ sudo apt-get update
Get:1 https://download.docker.com/linux/ubuntu jammy InRelease [48.9 kB]
Get:2 https://download.docker.com/linux/ubuntu jammy/stable arm64 Packages [5,880 B]
Hit:3 http://ports.ubuntu.com/ubuntu-ports jammy InRelease
Hit:4 http://ports.ubuntu.com/ubuntu-ports jammy-updates InRelease
Hit:5 http://ports.ubuntu.com/ubuntu-ports jammy-backports InRelease
Hit:6 http://ports.ubuntu.com/ubuntu-ports jammy-security InRelease
Fetched 54.7 kB in 3s (20.8 kB/s)
Reading package lists... Done
```

3.2 도커 엔진 설치
```
iksflow@iksflow-desktop:~$ sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following packages were automatically installed and are no longer required:
  dctrl-tools dmeventd dmraid dpkg-repack efibootmgr gir1.2-timezonemap-1.0 gir1.2-xkl-1.0 grub-common grub-efi-arm64 grub-efi-arm64-bin grub-efi-arm64-signed
  grub2-common kpartx kpartx-boot libdebian-installer4 libdevmapper-event1.02.1 libdmraid1.0.0.rc16 libdpkg-perl libfile-fcntllock-perl liblvm2cmd2.03
  libqt5designer5 libqt5help5 libqt5positioning5 libqt5sensors5 libqt5test5 libqt5webchannel5 libqt5webkit5 libtimezonemap-data libtimezonemap1 lvm2 os-prober
  python3-dbus.mainloop.pyqt5 python3-icu python3-pam python3-pyqt5 python3-pyqt5.qtsvg python3-pyqt5.qtwebkit python3-pyqt5.sip rdate thin-provisioning-tools
Use 'sudo apt autoremove' to remove them.
The following additional packages will be installed:
  docker-ce-rootless-extras libslirp0 pigz slirp4netns
  (생략)
```

4. docker 설치 확인
도커 설치를 정상적으로 마치고나서 명령을 실행하면 hello-world라는 도커 이미지를 다운받고 실행한다.
```
iksflow@iksflow-desktop:~$ sudo docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
7050e35b49f5: Pull complete
Digest: sha256:13e367d31ae85359f42d637adf6da428f76d75dc9afeb3c21faea0d976f5c651
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (arm64v8)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

5. docker 명령어 sudo 없이 사용하기
sudo 없이 docker 명령어를 사용하면 `Got permission denied while trying to connect to the Docker daemon~~` 라는 권한 오류가 발생한다.  
매번 sudo 입력하기 귀찮으니 로그인 사용자에게 권한을 부여하자.



5.1 권한 부여
sudo usermod -aG docker $USER 를 입력해 현재 로그인한 사용자에게 docker 사용 권한을 부여한다.  
하지만 권한을 부여하고 바로 사용하려고 하면 오류가 발생한다.  
```
iksflow@iksflow-desktop:~$ sudo usermod -aG docker $USER
iksflow@iksflow-desktop:~$ docker ps -a
Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get "http://%2Fvar%2Frun%2Fdocker.sock/v1.24/containers/json?all=1": dial unix /var/run/docker.sock: connect: permission denied
```

5.2 로그아웃 후 다시 확인
로그아웃 후 다시 접속해서 docker ps -a 명령을 입력하면 sudo 없이도 정상적으로 실행된다.
```
iksflow@iksflow-desktop:~$ logout
Connection to 192.168.0.11 closed.
iksflow@sungui-MacBookPro ~ % ssh iksflow@192.168.0.11
iksflow@192.168.0.11's password:
Welcome to Ubuntu 22.04 LTS (GNU/Linux 5.15.0-1005-raspi aarch64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

203 updates can be applied immediately.
74 of these updates are standard security updates.
To see these additional updates run: apt list --upgradable

Last login: Thu Jun 30 18:18:20 2022 from 192.168.0.2
iksflow@iksflow-desktop:~$ docker ps -a
CONTAINER ID   IMAGE         COMMAND    CREATED         STATUS                     PORTS     NAMES
624c125d5e6f   hello-world   "/hello"   6 minutes ago   Exited (0) 6 minutes ago             practical_antonelli
```

6. 실행중인 hello-world 컨테이너 종료
조금 전 docker ps -a를 실행했다면 컨테이너를 확인했을 것이다.
앞서 도커 설치를 확인하기 위해 hello-world 이미지를 실행해서 컨테이너가 생성된 상태이다.
해당 컨테이너를 종료하자.
docker rm 컨테이너 ID 형식으로 입력하면 컨테이너가 종료된다.
```
iksflow@iksflow-desktop:~$ docker ps -a
CONTAINER ID   IMAGE         COMMAND    CREATED         STATUS                     PORTS     NAMES
624c125d5e6f   hello-world   "/hello"   6 minutes ago   Exited (0) 6 minutes ago             practical_antonelli
iksflow@iksflow-desktop:~$ docker rm 624c125d5e6f
624c125d5e6f
iksflow@iksflow-desktop:~$ docker ps -a
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```