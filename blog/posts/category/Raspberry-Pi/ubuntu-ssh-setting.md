# Mac에서 라즈베리파이4 원격 제어하기


개발용 서버를 구축하기위해 라즈베리파이4(8GB)에 Ubuntu 22.04.LTS를 설치했습니다.  
초기 OS를 설치할 때는 별다른 방법이 없어서 모니터와 키보드를 연결해야합니다.  
하지만 매번 모니터랑 키보드를 연결하는건 불편합니다.  
모니터와 키보드 없이도 원격으로 Mac에서 라즈베리파이를 사용할 수 있도록 필요한 패키지를 설치했습니다.  

## 1. openssh-server 설치하기
apt명령어로 `openssh-server` 패키지를 설치하고 ssh접속을 허용하는 환경을 만들어줍니다.  
먼저 `sudo apt update` 명령어로 설치 가능한 패키지 목록을 업데이트합니다.  
다음으로 `sudo apt install openssh-server`를 입력해서 패키지를 설치합니다.  
마지막으로 `systemctl status ssh`를 입력해 설치후 ssh 서비스가 active(running)상태로 잘 동작하는지 확인합니다.  
정상적으로 설치와 실행을 마무리했다면 이제 ssh로 연결이 가능한 상태가 된 것입니다.  

참고로 apt는 Advanced Packaging Tools의 약자로 Devian계열 리눅스에서 사용하는 패키지 매니저입니다.  
Ubuntu는 Devian계열이므로 apt를 사용합니다.  
Redhat계열의 리눅스인 CentOS의 yum, MacOS의 homebrew와 같은 역할을 하는 도구입니다.  


```sh
iksflow@iksflow-desktop:~$ sudo apt update
iksflow@iksflow-desktop:~$ sudo apt install openssh-server
iksflow@iksflow-desktop:~$ systemctl status ssh
● ssh.service - OpenBSD Secure Shell server
     Loaded: loaded (/lib/systemd/system/ssh.service; enabled; vendor preset: enabled)
     Active: active (running) since Thu 2022-06-30 18:08:56 KST; 8min ago
       Docs: man:sshd(8)
             man:sshd_config(5)
   Main PID: 3933 (sshd)
      Tasks: 1 (limit: 8944)
     Memory: 3.8M
        CPU: 502ms
     CGroup: /system.slice/ssh.service
             └─3933 "sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups"
```

## 2. IP주소 확인
다음으로 연결할 IP주소를 확인합니다.  
`ip addr` 명령어를 입력하면 여러 결과들이 출력되는데 이중에서 `eth0`의 inet 부분 주소를 확인합니다.  
제 라즈베리파이4는 공유기에 연결한 상태인데 `192.168.0.11` 이라는 IP주소를 가지고 있습니다.  
참고로 이 IP주소는 사설IP이기 때문에 같은 네트워크에 속해있는 컴퓨터만 접속이 가능합니다.  
라즈베리파이에 접속하려는 맥북은 같은 공유기를 사용하고 있습니다. 다르게 말하면 같은 네트워크 안에 있다고 얘기할 수 있습니다.  
만약 외부에서도 접속할 수 있게하려면 포트 포워딩이라는 작업이 필요합니다.  
포트 포워딩으로 외부에서도 접속하게 만드는 방법은 다음번에 포스팅하도록 하겠습니다.  

```sh
iksflow@iksflow-desktop:~$ ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether dc:a6:32:b1:52:0f brd ff:ff:ff:ff:ff:ff
    inet 192.168.0.11/24 brd 192.168.0.255 scope global dynamic noprefixroute eth0
       valid_lft 5189sec preferred_lft 5189sec
    inet6 fe80::c0a3:fbf1:3a63:ecf4/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
3: wlan0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc fq_codel state DOWN group default qlen 1000
    link/ether dc:a6:32:b1:52:10 brd ff:ff:ff:ff:ff:ff
```


## 3. Mac에서 라즈베리파이에 ssh로 접속하기
이제 위에서 확인한 IP주소로 접속해봅니다.
맥북에서 `ssh 유저@IP주소` 형식에 맞게 입력하면 ssh로 해당 IP주소를 가진 컴퓨터에 연결을 시도합니다.  
올바른 패스워드를 입력하면 터미널을 사용할 수 있습니다.


```sh
iksflow@sungui-MacBookPro ~ % ssh iksflow@192.168.0.11
iksflow@192.168.0.11's password:
Welcome to Ubuntu 22.04 LTS (GNU/Linux 5.15.0-1005-raspi aarch64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

204 updates can be applied immediately.
75 of these updates are standard security updates.
To see these additional updates run: apt list --upgradable

Last login: Thu Jun 30 18:13:40 2022 from 192.168.0.2
iksflow@iksflow-desktop:~$
```