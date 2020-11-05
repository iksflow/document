# yum(Yellowdog Updater Modified)

## 1. yum은 어디에 사용할까?
RPM 기반의 패키지 매니저

## 2. yum 명령어
yum list의 경우 패키지를 조회하는데 사용한다.  
yum install, yum remove의 경우 프로그램을 제어하는 명령어이므로 root 권한을 필요로 한다.  
그러므로 yum 명령어를 입력할 때는 root 계정으로 사용하거나 sudo를 통해 입력해야 패키지 설치가 가능하다.

### 2.1 패키지 목록 조회하기
```sh
// 패키지 전체 목록
$ yum list 

// 설치된 패키지 목록
$ yum list installed

// 설치된 패키지 중 이름이 'sendmail'인 것이 있는지 확인
$ yum list installed | grep sendmail
```
### 2.2 패키지 설치
* yum install [패키지명1 패키지명2 ...]
```sh
// example
$ sudo yum install sendmail sendmail-cf
```
* yum -y install [패키지명1 패키지명2 ...]
yes/no 를 물어보는 경우 무조건 y로 응답한다.
```sh
// example
$ sudo yum install sendmail sendmail-cf
```

### 2.3 패키지 삭제
* yum remove [패키지명1 패키지명2 ...]
```sh
// example
$ sudo yum remove sendmail sendmail-cf
```


