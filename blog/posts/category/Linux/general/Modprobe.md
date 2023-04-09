# modprobe

리눅스 커널에 새로운 모듈을 추가하거나 제거하는 경우 사용하는 명령어.

리눅스는 모듈러 디자인으로 되어있다. 기능성은 모듈 또는 드라이버를 통해 확장이 가능하다
모듈은 코드 조각이며 재부팅 없이 기능확장을 할 수 있다. 로드되는 순간 모듈은 메모리 안에 상주하게 되고 여러차례 인스턴스화 될 수 있다. 장치 드라이버와 유사하다고 생각할 수 있다.


depmod로 생성된 의존성 목록, 하드웨어 맵을 사용해 똑똑하게 모듈을 커널에 로드, 언로드 한다.
실제 추가, 제거는 로우레벨 프로그램인 insmod와 rmmod를 사용해 처리한다.


옵션
modprobe -c(--showconfig): alias, alias symbol, blacklist 등의 다양한 정보 출력

modprobe -r(--remove) [modulename]: 모듈을 제거하면서 관련된 모듈도 제거
e.g)modprobe -r iptable_filter
modinfo [modulename]: 모듈 관련 정보를 출력한다
e.g) modinfo iptable_filter

# modprobe(8), rmmod(8), lsmod(8), modinfo(8) depmod(8)
depmod — Generate a list of kernel module dependences and associated map files.
insmod — Insert a module into the Linux kernel.
lsmod — Show the status of Linux kernel modules.
modinfo — Show information about a Linux kernel module.
rmmod — Remove a module from the Linux kernel.
uname — Print information about the current system.