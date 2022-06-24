# Caused by: org.hibernate.exception.SQLGrammarException: could not prepare statement


## 문제 발생
`JpaRepository`에 `@Query` 애너테이션에 `nativeQuery = true` 옵션을 추가하고 서버를 실행하니 다음과 같은 오류들이 발생했습니다.  

### 오류
* Caused by: org.hibernate.exception.SQLGrammarException: could not prepare statement
* Caused by: org.h2.jdbc.JdbcSQLSyntaxErrorException: Column "CREATEDATE" not found; SQL statement:
* FAILED org.springframework.dao.InvalidDataAccessResourceUsageException at QueryAnnotationTest.java:43
* Caused by: org.hibernate.exception.SQLGrammarException at QueryAnnotationTest.java:43
* Caused by: org.h2.jdbc.JdbcSQLSyntaxErrorException at QueryAnnotationTest.java:43

## 원인 분석
오류 내용을 해석하면 `@Query` 애너테이션에 작성한 JPQL을 분석했지만 문제가 있어서 준비할 수 없다는 내용입니다.  
더 자세하게 들여다 보면 `CREATEDATE` 라는 컬럼을 찾을 수 없다는 내용과 해당 오류가 `QueryAnnotationTest`클래스에서 발생했음을 알 수 있습니다.  

`@Query`애너테이션에 작성하는 JPQL은 SQL과 비슷하게 생겼지만 테이블, 컬럼들의 이름을 Entity에 맞게 사용해야하는 특징이 있습니다.  
그래서 JPQL의 Entity에 정의한 대소문자에 주의해서 작성해야 합니다.  
그런데 `@nativeQuery = true` 옵션을 추가하면 `@Query`는 네이티브 쿼리로 동작하게 됩니다.  
네이티브 쿼리는 테이블, 컬럼 이름을 입력할 때 Entity가 아니라 실제 데이터베이스의 테이블과, 컬럼명을 사용해야한다 특징이 있습니다.  
결국 오류는 데이터베이스 기준으로 해석했을 때 `CREATEDATE`라는 컬럼이 없기 때문에 발생한 것입니다.  
실제 데이터베이스의 `board`테이블 스키마에는 `CREATE_DATE`라는 이름으로 정의되어있기 때문입니다.  
아래 코드는 오류가 발생한 `Board` Entity와 `BoardRepository`의 코드입니다.  
`queryAnnotationTest3` 메서드의 `@Query`에 작성된 `createdate`라고 작성된 부분으로 인해 오류가 발생했습니다.  

#### Board
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

#### BoardRepository
```java
package com.example.demo.persistence;

import com.example.demo.domain.Board;
import org.springframework.data.domain.Page;
import org.springframework.data.domain.Pageable;
import org.springframework.data.jpa.repository.Query;
import org.springframework.data.repository.CrudRepository;
import org.springframework.data.repository.query.Param;

import java.util.List;

public interface BoardRepository extends CrudRepository<Board, Long> {
    List<Board> findByTitle(String searchKeyword);
    List<Board> findByTitleContainingOrContentContaining(String title, String content);
    List<Board> findByTitleContainingOrderBySeqDesc(String searchKeyword);
    Page<Board> findByTitleContaining(String searchKeyword, Pageable paging);
    @Query("SELECT b FROM Board b "
            + "WHERE b.title like %:searchKeyword% "
            + "ORDER BY b.seq DESC")
    List<Board> queryAnnotationTest1(@Param("searchKeyword") String searchKeyword);

    @Query("SELECT b.seq, b.title, b.writer, b.createDate "
            + "FROM Board b "
            + "WHERE b.title like %?1% "
            + "ORDER BY b.seq DESC")

    List<Object[]> queryAnnotationTest2(@Param("searchKeyword") String searchKeyword);

    @Query(value="select seq, title, writer, createdate "
            + "from board where title like '%'||?1||'%' "
            + "order by seq desc", nativeQuery = true)
    List<Object[]> queryAnnotationTest3(String searchKeyword);
}

```


## 해결 방법
네이티브 쿼리에서는 JPQL을 작성할 때 데이터베이스를 기준으로 테이블, 컬럼 이름을 입력해야 합니다.  
`queryAnnotationTest3`메서드의 `@Query`에 작성한 JPQL에서 `createdate`를 데이터베이스의 실제 컬럼 이름인 `create_date`로 변경합니다.  
네이티브 쿼리를 사용할 때는 데이터베이스 기준으로 테이블, 컬럼 이름을 입력했는지 잘 확인해보면 되겠습니다.  


