# Caused by: org.hibernate.AnnotationException: No identifier specified for entity


## 문제 발생
서버를 실행하니 `Caused by: org.hibernate.AnnotationException: No identifier specified for entity: com.example.demo.domain.Board` 라는 오류가 발생했습니다.  

## 원인 분석
Entity에 Id를 설정하지 않은 경우 발생하는 오류입니다.  
저는 아래와 같은 Entity를 추가했을 때 오류가 발생했습니다.  
코드에서 확인 가능하듯이 Id로 선택한 클래스 멤버 변수가 없습니다.  

```java
package com.example.demo.domain;

import lombok.Getter;
import lombok.Setter;
import lombok.ToString;

import javax.persistence.*;
import java.util.Date;

@Getter
@Setter
@ToString
@Entity
public class Board {
    private Long seq;
    private String title;
    private String writer;
    private String content;
    @Temporal(value = TemporalType.TIMESTAMP)
    private Date createDate;
    private Long cnt;
}
```

## 해결 방법
Id로 사용할 클래스 멤버 변수에 `@Id` 애너테이션을 추가하면 문제가 해결됩니다.  
`@GeneratedValue`는 Id의 값을 자동 증가시키기 위해 추가한 애너테이션입니다.  

```java
package com.example.demo.domain;

import lombok.Getter;
import lombok.Setter;
import lombok.ToString;

import javax.persistence.*;
import java.util.Date;

@Getter
@Setter
@ToString
@Entity
public class Board {
    @Id
    @GeneratedValue
    private Long seq;
    private String title;
    private String writer;
    private String content;
    @Temporal(value = TemporalType.TIMESTAMP)
    private Date createDate;
    private Long cnt;
}

```
