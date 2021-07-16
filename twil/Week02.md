# Rails로 간단한 게시판을 만들어보자

## 1. Rails에서 웹개발 시작하기

* Model - 비즈니스 로직을 처리.
* View - 처리한 내용을 보기좋게 만듦.
* Controller - 요청을 받아서 담당자에게 전달.

Rails는 기본적으로 MVC 패턴을 따라서 동작하게 되어있다.  
그래서 MVC를 편리하게 개발할 수 있게 도와주는 명령어들을 제공하며, Convention을 통해 코드를 압축할 수 있다.  
Convention을 통한 암묵적인(implicit) 로직들이 많아서, 웹 애플리케이션 아키텍처에 대해 잘 모르는 경우  혼란스러울 수 있다는 생각이 들었다.  
하지만 기초 지식이 있다면 Rails 덕분에 코드가 얼마나 간결해지는지 충분히 느낄 수 있다.

<br></br>

## 2. Rails 프로젝트 만들기

* Ruby
* SQLite3
* Node.js

Rails 프로젝트를 시작하기 위해서는 위와 같은 준비물이 필요하다.  
기본적으로 Ruby는 Linux와 MacOS 아키텍처를 기준으로 만들어졌기 때문에 해당 OS에서 개발하는것이 편하다.
물론 Windows에서도 Ruby Installer 또는 WSL(Windows Subsystem for Linux)을 사용해서 개발을 할 수 있지만, 실행환경 관리가 복잡해서 이 글에서는 다루지 않는다.  
오늘 날짜인 2021.07.16 기준으로 현재 최신 Ruby는 3.0.2버전, Rails는 6.1.4 버전이다.  
나는 실무에서 사용중인 버전에 대해 학습하기 위해 낮은 버전의 Ruby와 Rails를 설치했다.  
하지만 Ruby를 설치하는 과정에서 의존성 문제로 고생을 많이했다.  
Linux와 MacOS의 버전이 업데이트 되면서 기존 openssl(기존 1.0에서 현재는 1.1을 사용한다), libssl등의 라이브러리도 같이 업데이트 되었기 때문에 이전 버전에 의존하는 Ruby의 경우 오류가 많이 발생했다.  
때문에 특별한 이유가 없다면 최신버전을 설치하는것이 가장 편하다.

``` sh
# 준비물이 설치되었나 버전 확인하기 (설치 버전에 따라 내용이 다를 수 있음)
$ ruby --version
ruby 2.2.5
$ sqlite3 --version
3.32.3
$ node --version
v12.22.1

# rails install
$ gem install rails
$ rails --version
Rails 4.1.12

# example 이라는 이름의 Rails 프로젝트 만들기 - 현재 디렉토리 기준 example이라는 디렉토리가 생긴다.
$ rails new example
```

<br></br>

## 3. Controller 
``` sh
rails g controller [이름]  
```
Rails는 위 명령어를 통해 쉽게 controller를 생성할 수 있다.
## 4. View

## 5. Model


## References
https://phoenixnap.com/kb/install-ruby-on-windows-10