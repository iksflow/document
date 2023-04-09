커널 
하드웨어 자원을 사용할 수 있게하는 주체
모듈 
별도로 보관했다가 필요한 경우 호출해서 사용하는 코드.
커널에서 하드웨어를 제어하는 코드 중에는 가끔씩 사용하는 코드들도 있다.
이런 코드를 커널에 포함시켜놓으면 커널의 크기가 커지기 분리해서 커널의 크기를 줄이는데 사용한다.

1. 클린 타겟
이전에 커널 컴파일을 수행한 경우 잔여 파일들이 영향을 줄 수 있으므로 환경을 정리하는 과정이 필요하다

make clean: 커널 환경설정을 제외한 대부분의 파일을 모두 제거한다.
make mrproper : 커널 환경설정을 포함하여 모든 파일을 모두 제거한다.
make distclean: mrproper + 백업 및 패치파일까지 전부 제거한다.

2. 커널 환경설정   
make config: 텍스트 기반 환경설정 도구
make menuconfig: 텍스트 메뉴기반에서 커널 컴파일 관련 옵션을 설정함
make nconfig: 좀 더 향상된 컬러메뉴
make xconfig: GUI환경에서 커널 컴파일 관련 옵션을 설정함
make gconfig: X윈도우 환경

모듈

정의
커널의 기능을 확장하기 위해 메모리에 동적으로 로드 가능한 커널 오브젝트 파일. 확장자는 .ko
시스템을 중단하지 않고 메모리에 동적으로 로드, 언로드가 가능해서 LKM(Loadable Kernel Module)이라고 부르기도 한다.

필요성
모듈이 없이 모든 기능이 커널에 포함되어 있다면 각 기능들의 사용여부와는 관계없이 항상 시스템 메모리를 차지하고 낭비가 발생할 것이다.
그리고 새로운 기능을 추가하려면 커널을 다시 컴파일 해서 교체하는 번거로움과 재시작을 해야한다.
모듈은 이런 불편한 과정 없이 커널은 그대로 두고 사용자가 필요한 경우에만 메모리에 로드했다가 사용이 끝나면 제거하는 식으로 동작하기 때문에 효율적이다. 또한 시스템을 중단할 필요가 없다.

특징
* 시스템 중단 없이 기능의 추가가 가능하다.
* 커널의 일부분으로 시스템의 제약을 받지 않고 기능을 수행할 수 있다. 떄문에 잘못된 동작이라면 시스템이 패닉상태에 빠질 수 있으므로 조심해야한다.
* /lib/modules/kernel-version/kernel에 위치한다. ls -al /lib/modules/$(uname -r) 로 쉽게 확인이 가능하다.

명령어
lsmod: 현재 로드된 모듈의 리스트와 정보를 출력한다. /proc/modules 파일의 내용을 기반으로 보기좋게 출력해준다.
insmod
rmmod
modinfo
/etc/modprobe.d/*.conf  
modprobe.d 디렉터리 아래 conf확장자를 가진 모듈에 대한 설정파일을 정의할 수 있다.
내용은 아래와 같다
```
# e1000 모듈을 로드한다
e1000
# dummy라는 모듈에 대해 dummy0 별칭을 사용한다
alias dummy0 dummy
# b43모듈이 커널에 적재될 때 마다 nohwcrypt=1 옵션과 qos=0 옵션을 입력한다.
options b43 nohwcrypt=1 qos=0
# ieee1394모듈을 로드하는 대신 /bin/true 명령을 실행한다.
install ieee1394 /bin/true
# ieee1394모듈을 언로드하는 대신에 /bin/true를 실행한다.
remove ieee1394 /bin/ture
# i8xx_tco 모듈이 로드하는것을 막는다.
blacklist 18xx_tco

```
depmod - cat /lib/modules/$(uname -r)/modules.dep 파일을 생성. 
/lib/modules/$(uname -r)/ 이하의 모듈들을 검사하여 파일을 생성한다. modprobe는 이 파일을 바탕으로 모듈 간 의존성을 파악한다.