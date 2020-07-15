
### 1. 셸이란?
셸(Shell)은 사용자가 커널을 사용할 수 있도록 하기위한 통로이자 인터페이스다.  
사용자가 커널에 직접 명령을 내리는것은 어렵기 때문에 셸을 통해 파일관리, 프로세스 관리, 배치 프로세싱, 성능 모니터링, 환경 설정 등 커널의 기능을 사용할 수 있도록 도와준다.

### 2. 셸의 유형
크게 본셸 계열과 C셸 계열로 나뉜다.  
bash셸은 Windows, Mac에서도 사용되는 셸이며, 리눅스의 기본 셸도 bash셸로 설정되어 있다.
  
본셸 
* sh(bourne shell)
* ksh(korn shell)
* bash(bourne again shell)

C셸
* csh(C shell)
* tcsh(TC shell)

### 3. 셸 명령어
* 현재 셸 확인하기
``` shell
$ echo $SHELL
/bin/bash
```
* 시스템이 지원하는 셸 목록 확인하기
``` shell
1. chsh 명령어를 통한 쉘 목록 확인
$ chsh -l
/bin/sh
/bin/bash
/usr/bin/sh
/usr/bin/bash

또는

2. 셸 목록이 저장된 /etc/shells 파일을 직접 열어 확인
$ cat /etc/shells
/bin/sh
/bin/bash
/usr/bin/sh
/usr/bin/bash
```
* 셸 변경하기
``` shell
bash 셸에서 sh로 변경하기
$ cat /etc/passwd | grep iksflow(계정명)
iksflow:x:1000:1000:iksflow:/home/iksflow:/bin/bash

$ chsh -s /bin/sh

$ cat /etc/passwd | grep iksflow(계정명)
iksflow:x:1000:1000:iksflow:/home/iksflow:/bin/sh

```