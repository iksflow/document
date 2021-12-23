## 1. 클래스
```ruby
class Foo
    # 생성자
    def initialize
        puts "Foo is created."
    end
end
```
## 2. 모듈
```ruby
module FooModule
    def 
end
```
만약 같은 이름을 가진 모듈을 같은 파일에서 불러오는 경우?
둘 다 불러와서 사용이 가능하다.
그러나 함수의 이름이 같은 경우는 마지막에 불러온 모듈의 함수를 호출한다.