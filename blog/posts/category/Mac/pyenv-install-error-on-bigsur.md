# MACOS - BIGSUR에서 pyenv 설치시 발생하는 오류 수정

## 현상
Catalina에서  Bigsur 로 업데이트를 한 다음 pyenv를 통해 Python 3.7.3을 설치하려고 하니 오류가 발생했다.  
이상한건 3.7.9 버전을 설치할 때는 문제가 없었다는 점이다.

``` sh
% pyenv install 3.7.3
.
. 생략
.
BUILD FAILED (OS X 11.2.2 using python-build 20180424)
.
. 생략
.
```
<br></br>

## 원인파악
* XCode Command Line Terminal 버전 문제?
* zlib 문제?
* pyenv install 3.7.9는 이상 없음  

맥에 아직 익숙하지 않아서 자세히는 모르겠지만 Bigsur로 업데이트 하게되면서 다른 모듈들과의 호환성 문제로 인해서 설치가 안되는것으로 추측된다.
<br></br>

## 해결방법
터미널을 열고 아래 명령어를 복사해서 입력하면 정상적으로 설치된다.  
아래 명령어는 3.7.3버전을 설치하는 명령이다.
다른 버전의 파이썬을 설치하려는 경우 아래 명령어에서 'pyenv install --patch 원하는버전' 처럼 수정해주면 된다.  
``` sh
CFLAGS="-I$(brew --prefix openssl)/include -I$(brew --prefix bzip2)/include -I$(brew --prefix readline)/include -I$(xcrun --show-sdk-path)/usr/include" LDFLAGS="-L$(brew --prefix openssl)/lib -L$(brew --prefix readline)/lib -L$(brew --prefix zlib)/lib -L$(brew --prefix bzip2)/lib" pyenv install --patch 3.7.3 < <(curl -sSL https://github.com/python/cpython/commit/8ea6353.patch\?full_index\=1)
```
<br></br>

## Reference
* Python3 시작하기 가이드  
    * https://opensource.com/article/19/5/python-3-default-mac  
* Pyenv Github Issues - thanks to geezylucas
    * https://github.com/pyenv/pyenv/issues/1746#issuecomment-736750003
