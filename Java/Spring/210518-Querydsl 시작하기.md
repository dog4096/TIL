#Querydsl 시작하기

---

> com.ewerk.gradle.plugins.querydsl의 사용은 피하자.   
> 2018년 이후로 지원이 끊겼으며 최신 버전의 Gradle에서 적용이 좀 곤란하다.  
> 일단 JPA와의 연동을 위한 것부터 시작해보려고 한다.
> > 기본적으로 많이들 쓰는 건 EntityManager를 통해 만들 수 있는 JPAQueryFactory 이나  
> > JPA없이 DB연결 정보를 이용해 JDBC로도 Querydsl을 쓸 수 있다.  
> > https://github.com/querydsl/querydsl/tree/master/querydsl-sql
### 1. Gradle 설정

```gradle
dependencies {
    compile("com.querydsl:querydsl-core")
    compile("com.querydsl:querydsl-jpa")
    annotationProcessor("com.querydsl:querydsl-apt:${dependencyManagement.importedProperties['querydsl.version']}:jpa") 
    annotationProcessor("jakarta.persistence:jakarta.persistence-api")
    annotationProcessor("jakarta.annotation:jakarta.annotation-api")
}

def generated='src/main/generated'
sourceSets {
    main.java.srcDirs += [ generated ]
}

tasks.withType(JavaCompile) {
    options.annotationProcessorGeneratedSourcesDirectory = file(generated)
}
```

Gradle 설정 후 compileJava Task를 실행하면 generated 폴더와 함께 Querydsl에서 쓰일 Q클래스들이 생성된다.

---
### 2. Querydsl 사용

1. 심플 예제  
일단 공식 홈페이지의 가장 간단한 예제는 이런식이다.
```java
@Repository
public class TestSupport extends QuerydslRepositorySupport {

    @PersistenceContext
    private EntityManager entityManager;
    
    public TestSupport() {
        super(TestEntity.class); // Querydsl 실행 후 Fetch시에 반환될 클래스
    }
    
    public List<TestEntity> test() {
        // 주입받은 EntityManager를 이용해 JPAQuery 객체를 생성한다.
        JPAQuery<?> query = new JPAQuery<TestEntity>(entityManager);
        // import static {your.package}.entity.QTestEntity.testEntity;
        // compileJava Task를 실행하면 자동으로 static으로 위의 위치에 변수가 생성되어서 가져다 쓸 수 있다.
        List<TestEntity> entity = query.select(testEntity)  
                .from(testEntity)
                .fetchAll();
        return entity;
    }
    
}
```

> 위의 testEntity 부분은 따라가보면   
> ```java
> public static final QTestEntity testEntity = new QTestEntity("testEntity");
> ```
> 이런 식으로 구현되어있으므로 static 변수를 활용하지 않고 
> ```java
> QTestEntity testEntity = new QTestEntity("customAlias");
> ```
> 이런 식으로 함수내에서 사용해도 된다. (셀프 조인을 구현할 때 유용했다.)
2. JPAQueryFactory

먼저 JPAQueryFactory를 반환해줄 Bean을 만든다.
```java
@Bean
public JPAQueryFactory jpaQueryFactory(EntityManager entitymanager) {
    return new JPAQueryFactory(entityManager);
}
```

1번의 예제에서의 JPAQuery를 JPAQueryFactory를 통해서 생성해주면 된다.
```java
@Repository
public class TestSupport extends QuerydslRepositorySupport {

    private JPAQueryFactory queryFactory;
    
    public TestSupport(JPAQueryFactory queryFactory) {
        super(TestEntity.class); // Querydsl 실행 후 Fetch시에 반환될 클래스
        this.queryFactory = queryFactory;
    }
    
    public List<TestEntity> test() {
        // 주입받은 JPAQueryFactory 이용해 JPAQuery 객체를 생성한다.
        JPAQuery<?> query = queryFactory.query();
        List<TestEntity> entity = query.select(testEntity)  
                .from(testEntity)
                .fetchAll();
        return entity;
    }
    
}
```