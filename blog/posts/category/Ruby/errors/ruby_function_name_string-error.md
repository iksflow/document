
# Ruby 3.0.x 설치 오류 해결하기

기쁜맘으로 Ruby 3버전을 영접하려다 오류로 골머리좀 앓았습니다...  
RUBY_FUNCTION_NAME_STRING 너는 누구냐...!

<br><br>

## 환경
MacOS: Bigsur 11.4  
XCode: 13.1  

<br><br>

## 오류 발생 배경
`asdf-vm` 으로 `ruby 3.0.x`버전을 install 하려는데 문제가 발생했습니다.  
install을 실행하면 아래처럼 오류가 발생합니다.  



```sh
$ asdf install ruby 3.0.0
Downloading ruby-3.0.0.tar.gz...
-> https://cache.ruby-lang.org/pub/ruby/3.0/ruby-3.0.0.tar.gz
Installing ruby-3.0.0...
ruby-build: using readline from homebrew

BUILD FAILED (macOS 11.4 using ruby-build 20210611)

Inspect or clean up the working tree at /var/folders/w3/3_t92rq97t7_8ff_g93pbz500000gn/T/ruby-build.20220104232701.59961.Yiy1eW
Results logged to /var/folders/w3/3_t92rq97t7_8ff_g93pbz500000gn/T/ruby-build.20220104232701.59961.log

Last 10 log lines:
                                                                       ^
195 warnings generated.
gc.c:6106:13: error: use of undeclared identifier 'RUBY_FUNCTION_NAME_STRING'
            rp(obj);
            ^
./internal.h:95:72: note: expanded from macro 'rp'
#define rp(obj) rb_obj_info_dump_loc((VALUE)(obj), __FILE__, __LINE__, RUBY_FUNCTION_NAME_STRING)
                                                                       ^
221 warnings and 3 errors generated.
make: *** [gc.o] Error 1
```

<br><br>

## 시도해봤던 방법들
구글링 하면서 시도해봤던 방법들인데 결국에는 `llvm설치`와 `.zshrc`를 변경하니 해결되었습니다.  
### 실패
* XCode Update
* XCode Command Line Tools Update
* openssl update (openssl@3)

### 성공
* llvm 설치
* .zshrc의 환경변수 수정(PATH, LDFLAGS, CPPFLAGS)

<br><br>

## 원인
기존에 .zshrc에 정의한 환경변수가 문제인 것으로 추측됩니다.  
제 개발환경에서는 ruby 2.2.5 버전을 위한 세팅이 되어있었습니다.  
Bigsur에서 옛날 버전 Ruby와 호환이 안돼 이런저런 설정이 많이 필요했습니다.  
한 가지 예를 들자면 Bigsur에서는 기본값이 openssl@1.1인데 ruby 2.2.5에서는 openssl@1.0 을 필요로했기에 이를 위한 PATH가 많이 추가된 상태였습니다.  

<br><br>

## 해결 방법
Homebrew로 llvm을 설치한 다음 .zshrc의 환경변수를 llvm의 경로로 수정했더니 오류가 해결됐습니다.  
```sh
# llvm install
$ brew update
$ brew upgrade
$ brew install llvm

# backup .zshrc
$ mv ~/.zshrc ~/.zshrc.bak

# create new .zshrc
$ vi ~/.zshrc

# copy into .zshrc!
export PATH="/usr/local/opt/llvm/bin:$PATH"
export LDFLAGS="-L/usr/local/opt/llvm/lib"
export CPPFLAGS="-I/usr/local/opt/llvm/include"

# apply config
$ source .zshrc

# install ruby 
$ asdf install ruby latest
Downloading ruby-3.0.1.tar.gz...
-> https://cache.ruby-lang.org/pub/ruby/3.0/ruby-3.0.1.tar.gz
Installing ruby-3.0.1...
ruby-build: using readline from homebrew
Installed ruby-3.0.1 to /Users/iksflow/.asdf/installs/ruby/3.0.1
```

<br><br>


## Reference
thanks to henrynguyen6677!
https://github.com/rbenv/ruby-build/issues/1431#issuecomment-637262714