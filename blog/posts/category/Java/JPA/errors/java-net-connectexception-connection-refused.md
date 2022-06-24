# Caused by: java.net.ConnectException: Connection refused (Connection refused)


## 문제 발생
서버를 실행하니 다음과 같은 오류들이 발생했습니다.  

### 오류
* Caused by: java.net.ConnectException: Connection refused (Connection refused)
* Caused by: org.h2.jdbc.JdbcSQLNonTransientConnectionException: Connection is broken
* Caused by: org.hibernate.exception.JDBCConnectionException: Unable to open JDBC Connection for DDL execution
* Caused by: javax.persistence.PersistenceException: [PersistenceUnit: default] Unable to build Hibernate SessionFactory
* Caused by: org.h2.jdbc.JdbcSQLNonTransientConnectionException: A file path that is implicitly relative to the current working directory is not allowed in the database URL "jdbc:h2:tcp//localhost/~/test". Use an absolute path, ~/name, ./name, or the baseDir setting instead. [90011-200]


## 원인 분석
JPA에서 데이터베이스와 연결할 수 없어서 발생한 오류입니다.  
저의 경우 데이터베이스를 H2로 설정했으나 연결할 수 없는 상태라고 나오네요.  
앞서 소개한 오류 내용들을 확인해보면 결국 데이터베이스에 연결할 수 없어서 파생된 오류들입니다.  
데이터베이스에 연결이 안된다. -> DDL 명령을 수행할 수 없다 -> Hibernate의 SessionFactory 를 생성할 수 없다처럼 연쇄적으로 발생한 것입니다.


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
결국 왜 데이터베이스에 연결이 불가능했는지 하나씩 확인하고 고치면 문제는 자연스럽게 해결될 것입니다.  
그럼 하나씩 문제가 의심되는 부분을 짚어보겠습니다.  

### 1. 데이터베이스 실행 상태 확인
먼저 데이터베이스가 정상적으로 동작하고 있는지 확인해야합니다.  
저는 H2를 데이터베이스로 설정해서 실습을 진행하고 있습니다.  
하지만 프로그램 실행 전 H2 데이터베이스를 실행하는것을 깜빡 잊어버렸네요.  
그래서 `{{H2설치경로}}/bin/h2.sh` 파일을 실행해서 H2 데이터베이스를 실행했습니다.  
윈도우의 경우 `h2.bat`파일을 실행하면 되고, `MySql`, `Oracle` 등 다른 데이터베이스를 사용하는 분들은 데이터베이스의 서비스가 실행중인지 확인해보면 되겠습니다.  


### 2. 데이터베이스 주소 확인
당연한 얘기지만 데이터베이스와 연결하려면 주소를 알아야합니다.  
만약 이 주소가 잘못 설정되어있으면 연결할 때 오류가 발생하게됩니다.  
저는 `Caused by: org.h2.jdbc.JdbcSQLNonTransientConnectionException: A file path that is implicitly relative to the current working directory is not allowed in the database URL "jdbc:h2:tcp//localhost/~/test"` 같은 오류가 발생했습니다.  
이상한 부분을 딱히 없다고 생각했는데 자세히 들여다 보니 `tcp` 뒤에 콜론(:) 이 빠뜨리는 오타가 있었네요.  
오타를 확인하고 바로 `application.properties`에서 `spring.datasource.url`을 수정했습니다.

```java
# DataSource Setting
spring.datasource.driver-class-name=org.h2.Driver
spring.datasource.url=jdbc:h2:tcp://localhost/~/test
spring.datasource.username=sa
spring.datasource.password=
```

파일을 수정하고 다시 서버를 실행하니 정상적으로 동작하는것을 확인할 수 있었습니다.


