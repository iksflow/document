# MACOS - BIGSUR에서 pyenv 설치시 발생하는 오류 수정

## 현상
Catalina에서  Bigsur 로 업데이트를 한 다음 pyenv를 통해 Python 3.7.3을 설치하려고 하니 오류가 발생했다.  

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
<br></br>

## 해결방법
1. 
2. 현재 사용중인 쉘을 확인한다. 
    ```
    % echo $SHELL
    ```
3. .zshrc 파일에 아래의 내용을 추가한다.
<br></br>

## Reference
* Python3 시작하기 가이드  
    * https://opensource.com/article/19/5/python-3-default-mac  
* Pyenv Github Issues 
    * 
