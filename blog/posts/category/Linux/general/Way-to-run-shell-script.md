# 셸 스크립트를 실행하는 방법

셸 스크립트(.sh) 를 실행하는 방법에는 크게 2가지가 있다.  
현재 프로세스에서 실행하는 방법과 새 프로세스로 실행하는방법이다.
* 예제 스크립트 작성하기
```sh
$ vi exit.sh

-- exit.sh파일 내용

#!/bin/bash
exit
```
예제로 작성한 스크립트는 실행 시 실행 주체인 터미널을 종료한다.


* 현재 프로세스에서 실행
``` sh
$ source exit.sh
$ . exit.sh
```
현재 프로세스에서 셸을 실행하는 방법은 `source exit.sh` 또는 `. exit.sh` 처럼 사용하면 된다.  
현재 프로세스에서 실행하기 때문에 실행 시 터미널이 종료된다.


* 새 프로세스에서 실행
```sh
$ chmod +x exit.sh
$ ./exit.sh
```
별도의 프로세스를 통해 실행하려면 권한이 필요하다.  
`chmod +x exit.sh` 명령어를 통해 실행권한을 부여하고 `./exit.sh`를 통해 실행했다.  
앞서 살펴본 예제와는 다르게 터미널이 종료되지 않는다.  
별도의 프로세스에서 실행했기 때문이다.
