# 계정에 sudo 권한 부여하기

## 1. root로 login 하기
사용중인 계정에 sudo 권한을 부여하려면 /etc/sudoers 파일을 편집해서 사용하려는 계정을 추가해야한다.  
하지만 이 파일의 읽기, 쓰기권한은 root에게 있기때문에 먼저 root 계정으로 로그인 해야한다.
``` shell
su - 
// 또는
su root
```

## 2. /etc/sudoers 파일 수정하기
/etc/sudoers 파일의 내용에 권한을 부여하고자하는 계정을 추가할 것이다.  
먼저 아래 명령어를 통해 해당 파일을 연다.  
``` shell
vi /etc/sudoers
```
'Allow root to run any commands anywhere' 라고 적혀있는 부분 아래에 계정을 추가해야한다.  
'/' 키를 누르면 vi 편집기에서 검색을 할 수 있다.  
'/Allow root to run' 이라고 검색하면 금방 찾을 수 있다.  
'a' 또는 'i' 키를 눌러 편집모드로 변경한 다음 아래와 같이 계정을 추가해준다.

``` shell
## Allow root to run any commands anywhere
root      ALL=(ALL)      ALL
iksflow   ALL=(ALL)      ALL
```

수정을 완료했으면 ESC 키를 눌러 편집모드를 종료한다.  
그리고 ':wq!' 를 입력해서 저장 후 파일을 닫는다.  

