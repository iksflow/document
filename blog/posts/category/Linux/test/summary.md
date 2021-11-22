# chage 사용자 정보 변경
-E: 만료일 지정
-m: 패스워드 변경 최소 일수
-M: 패스워드 변경 후 유효일수

# 디스크 쿼터
edquota, quota

# 프로세스 우선순위 변경
nice, renice

nice: 기존 값에서 조정수치만큼 변경. 프로세스 이름을 인자로 줌
renice: 프로레스 ID를 인자로 줌

# 패키지명 찾기
which, whereis

# 프린터 관련 명령어
lp, lpr
인쇄 매수 입력
lp -n
lpr -#



# crontab: 정기적인 작업의 자동화
cron 서비스는 자동 반복실행을 지원하는 데몬이다.
cron은 crontab(/var/spool/cron/blahblah)의 데이터를 읽어 명령을 수행한다.
```sh
# 작업 추가하기
cron -e
# * * * * * 명령  
# 분 시 일 월 초 요일(일요일 = 0)

cron -e */10 * * * * touch /root/et

# 작업 변경하기
cron -e -u 유저명

# 작업 등록 후 cron 서비스 재시작
service crond restart
```

# rpm 패키지 관리
-q : 질문 시작시 쓰는 명령어
-qR : 패키지가 의존내용 출력
-qlp : 패키지가 설치될 파일이나 디렉터리 확인
-qc : 패키지 환경설정파일 정보만 출력

# 모듈 관련 작업 및 커널 컴파일
/lib/modules/커널버전/modules.dep 모듈간 의존성이 기록된 파일명
uname: system 정보를 출력하는 명령어
uname -r: 커널정보를 출력

make mrproper: 커널 설정 정보 초기화
depmod: 모듈간 의존성 관리. modules.dep 파일을 생성함
make modules: 선택한 모듈을 생성하는 명령
make modules_install: 생성한 모듈을 설치

# 시스템 장치 확인
**장치정보가 담긴 파일**  
/proc/cpuinfo : cpu 정보가 담긴 파일
/proc/meminfo : 메모리 정보
/proc/mdstat : RAID정보 확인
/proc/version : 사용중인 커널 버전정보

# 시스템 로그설정
`man logger`로 관련 정보를 확인할 수 있음
/var/log 하위 디렉터리쪽에 정보가 쌓임

로그설정파일은 `/etc/syslog.conf`또는 `/etc/rsyslog.conf`이다.
로그 상황 설정파일은 선택자필드와 액션필드로 나뉜다.
선택자필드는 다시 메시지종류(facility 또는 서비스)와 우선순위(priority)로 나뉜다.
액션필드는 
로그 설정은 `facility.priority` 그리고 `액션`으로 한 라인을 사용해 정의한다.
예를 들자면 `mail.* /var/log/maillog`는 메일 관련 서비스로그를 `/var/log/maillog`에 저장하겠다는 의미

*.emerg, *.panic: 서비스(facility)가 최고수준(priority)의 위험상황인 경우 메시지를 보냄
```sh
# /etc/rsyslog.conf
*.emerg root, ihduser
or
*.emerg :omusrmsg:root,ihduser
```

mail.=error

# 사용자 접속 로그 확인
last, lastlog, lastb로 확인

last는 /var/log/wtmp 파일을 참조
lastlog는 /etc/lastlog 파일을 참조
lastb는 /var/log/btmp 파일을 참조

last는 전체적
lastlog는 가장 최근
lastb 는 실패기록 조회할 때 쓴다.

# ssh 접속
ssh-keygen: ssh 키 만들기
ssh -p

# 디스크 백업
dd: 파일 복사 명령어이다.
사용 예
dd if=/dev/sda1 of=/dev/sdb1 bs=4096 (or 4k)

# 아파치 웹 서버
서버네임
서버루트
도큐먼트루트
userdir
server tokens
keep alive
# 아파치 웹 사용자 인증

htpasswd -c /etc/password ihduser: ihduser 라는 사용자 등록. 저장파일은 /etc/password 경로에

/etc/httpd/conf/httpd.conf: 아파치 설정 파일
.htaccess: 

# 삼바 서버 설정
명령어: smbclient
설정파일: /etc/samba/smb.conf

설정하기
```sh
vi /etc/samba/smb.conf
# 공유폴더
[www]
#코멘트
comment = 
valid users = 
```

smbclient -L 192.168.5.13: -L은 주소 입력



# 메일서버 설정
sendmail 패키지 설치 필요

/etc/mail 경로에 설치됨

하나의 메일서버에 여러 도메인이 있는경우 같은아이디가 존재할 수 있음.
이때 분기를 해줘야하는데 필요한 파일이 /etc/mail/virtusertable 이다.

```sh
# /etc/mail/virtusertable
$ vi /etc/mail/virtusertable
ceo@ihd.or.kr   ihduser
ceo@kait.or.kr  kaituser

# 적용하기
$ makemap hash /etc/mail/virtusertable < /etc/mail/virtusertable
```
## 특정 계정으로 들어오는 메일을 다른 계정을 전송되게 하기
/etc/aliases

## DNS서버환경설정
필요 패키지: bind
설정 파일: /etc/named.conf
명령어: acl
사용 예
acl "ihd" { 192.168.}
질의 가능한 호스트 설정
allow-query { 192.168.5.0/24; 192.168.12.22; }
존 파일 내용 복사 대상 제한
allow-transfer
forward

## 방화벽 규칙 설정
iptables