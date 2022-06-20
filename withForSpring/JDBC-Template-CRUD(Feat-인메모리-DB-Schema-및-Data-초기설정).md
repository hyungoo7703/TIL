>참고: [https://github.com/danvega/hello-jdbc](https://github.com/danvega/hello-jdbc)

# JDBC Template CRUD

Spring Data JDBC 학습에 앞서, JDBC Template을 활용한 CRUD 기능을 복습 차원으로 정리하고자 한다. <br>
또 추가적으로 인메모리 DB를 사용할 때 Schema 및 Data를 활용해 초기 DB 값을 설정 하는 것도 정리 해보자.


## 시작하기

기본적으로 Spring Boot 사용 기반에 <br>
**추가 종속성: (Spring Web, Spring Data JDBC, H2 Database)** 을 가지고 시작한다.  <br>
여기서 Spring Data JDBC는 사용하지 않지만, 의존성 추가의 이유는 바로 아래에서 확인할 수 있다.

##  인메모리 DB(ex H2) Schema 및 Data 초기 설정하기

Spring Data JDBC와 같이, Spring Data기반 종속성이 추가된 Spring Boot 환경에서는 Resources 폴더 아래 

![2-15(1)](https://user-images.githubusercontent.com/93297109/153990587-79a695fb-2a18-4369-b0d0-060c6e807ce7.png)

그림과 같이 data.sql, schema.sql 파일의 추가로 SQL문 스크립트 실행이 가능하다. (인메모리 DB 한정) <br>

```sql
-- schema.sql 추가
CREATE TABLE example ( 
    id integer identity NOT NULL,
    name varchar(255) NOT NULL,
    CONSTRAINT pk_id PRIMARY KEY (id)
);
```
```sql
-- data.sql 추가
INSERT INTO example(name) values ('name1');
INSERT INTO example(name) values ('name2');
INSERT INTO example(name) values ('name3');
INSERT INTO example(name) values ('name4');
INSERT INTO example(name) values ('name5');

```

실행의 모습은 application.properties 설정 후 보도록 하자.

-----

## 구축과정

0. application.properties 설정

Schema 및 Data 초기 설정을 위해 그 동안과는 조금 다르게 설정 하였다.

```
spring.datasource.name=jdbcdb
spring.datasource.generate-unique-name=false
spring.h2.console.enabled=true
```

Datasource 이름을 정하고 (나의 경우 jdbcdb) <br>
localhost:8080/h2-console 으로서의 console 접근을 허용 설정을 한다. <br>
그 후 Spring Boot Application 실행 시,

![2-15(2)](https://user-images.githubusercontent.com/93297109/153990830-7988213c-9a57-4514-bb2b-4639c59f8563.png)

설정한 이름에 대한 내용이 출력되는데, 여기서 지정된 JDBC URL이 **jdbc:h2:mem:jdbcdb**임을 알 수 있다. <br>
실제로 스크립트 실행이 잘 되었는지 확인 해보기 위해 H2 Console에 접속 하였고 

![2-15(3)](https://user-images.githubusercontent.com/93297109/153991510-b609c2d7-490b-4a6c-96df-c5bcda6c4df9.png)

위와 같이 지정한 JDBC URL에서 TABLE생성, 데이터 5개 입력이 정상적으로 됨을 확인할 수 있다.

1. model 객체 만들기

```java
public class Example {

	private Long id;
	private String name;

	public Long getId() {
		return id;
	}

	public void setId(Long id) {
		this.id = id;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

}
```

2. repository 구현 

```java
import java.util.List;
import java.util.Optional;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.jdbc.core.RowMapper;
import org.springframework.stereotype.Repository;

import jdbctemplate.demo.model.Example;


@Repository
public class ExampleJdbcRepository {

	private final JdbcTemplate jdbcTemplate;
	private static Logger log = LoggerFactory.getLogger(ExampleJdbcRepository.class);

	@Autowired
	public ExampleJdbcRepository(JdbcTemplate jdbcTemplate) {
		this.jdbcTemplate = jdbcTemplate;
	}

	//== CRUD (Create, Read, Update, Delete) ==//

    public void create(Example example) {
        String sql = "insert into example(name)) values(?)";
        int insert = jdbcTemplate.update(sql, example.getName()); // jdbcTemplate의 query 메소드 사용 (결과 값: int), sql 다음엔 ?와 순서대로 매핑)
        if(insert == 1) { // 영향을 받은 행의 수를 알려준다. (1개 영향을 받았을 경우 Create 성공)
        	log.info("New Example Created: " + example.getName());
        }
    }
    
    public List<Example> list() { // 전체 출력
        String sql = "SELECT id, name from example";
        return jdbcTemplate.query(sql,rowMapper); // jdbcTemplate의 query 메소드 사용 (결과 값: List), rowMapper를 통해 객체에 매핑 (객체를 반환하기 위함)
    }

    public Optional<Example> get(Long id) { // 하나 출력
        String sql = "SELECT id, name from example where id = ?";
        return jdbcTemplate.query(sql, rowMapper, id).stream().findAny(); // List 값에서 stream().findAny()를 통해 Optional로 리턴 (filter 조건에 만족하는)
    }

    public void update(Example example, Long id) {
        String sql = "update example set name = ? where id = ?";
        int update = jdbcTemplate.update(sql, example.getName(), id);
        if(update == 1) {
            log.info("Example Updated: " + example.getName());
        }
    }

    public void delete(int id) {
        String sql = "delete from example where id = ?";
        int delete = jdbcTemplate.update(sql,id);
        if(delete == 1) {
        	 log.info("Example Deleted: " + id);
        }
    }
	
	// RowMapper 분리
	RowMapper<Example> rowMapper = (rs, rowNum) -> {
		Example example = new Example();
		example.setId(rs.getLong("id"));
        example.setName(rs.getString("name"));
        return example;
    };
    
}
```

## 실행하기

간단한 테스트를 위해 SpringApplication에 System.out.println로 출력 해 보았다. (좋은 방법은 아님) <br>
list 메소드만 테스트 해보았다.

```java
@SpringBootApplication
public class DemoApplication {
	
	private static ExampleJdbcRepository exampleJdbcRepository;
	
	public DemoApplication(ExampleJdbcRepository exampleJdbcRepository) {
		this.exampleJdbcRepository = exampleJdbcRepository;
	}

	public static void main(String[] args) {
		SpringApplication.run(DemoApplication.class, args);
		
		System.out.println("\n All Example -------------------------------------\n");
		List<Example> examples = exampleJdbcRepository.list();
		for (Example example : examples) {
			System.out.println(example.getName());
		}
	}

}
```

![2-15(4)](https://user-images.githubusercontent.com/93297109/153998313-ff6bbbe1-8816-4096-9ac7-8d35f3e7a6d3.png)

위와 같이 data.sql에 추가한 데이터들이 정상 출력됨을 확인할 수 있다.
