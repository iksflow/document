# Call by value, Call by reference 그리고 Java

이번 시간에는 Call by Value와 Call by Reference 그리고 Java는 둘 중 어떤방식으로 동작하는지에 대해 알아보겠습니다.
Call by Value, Call by Reference라는 개념은 함수를 호출할 때 인자를 전달 방법들입니다.  
Pass by value, Pass by reference라고 불리우기도 합니다.  

## 1. Call by Value

먼저 Call by value에 대해 알아보겠습니다.
Call by value는 해석하자면 값에 의한 호출이라는 의미입니다.  
여기서 말하는 값은 `복사한 값`을 의미합니다.
따라서 복사한 값을 조작하더라도 원본에 영향을 미치지 않는다는 특징이 있습니다.


## 2. Call by reference


## 3. Java는 그럼 어떤 방식?
결론부터 말하자면 Java는 Call by value방식으로 동작합니다.  
저도 Java의 몇몇 기본서에 '참조변수를 전달할 때는 Call by reference로 동작한다' 라고 적혀있던것을 얼핏 봤던 기억이납니다.
다시 한번 강조하지만 Java는 Call by value방식으로 동작하고 Call by Reference로 동작하지 않습니다.  