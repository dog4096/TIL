##다중 DataSource 설정시 Naming-Strategy 설정 문제  
다중 DataSource를 설정하면 대부분의 설정을 Bean을 통해 다시 직접 해줘야하는 문제가 생긴다.  
여기서 파생된 문제가 Properties에서 설정 가능한
```properties
spring.jpa.hibernate.naming.physical-strategy=org.springframework.boot.orm.jpa.hibernate.SpringPhysicalNamingStrategy
#spring.jpa.hibernate.naming.physical-strategy=org.hibernate.boot.model.naming.PhysicalNamingStrategyStandardImpl
spring.jpa.hibernate.naming.implicit-strategy=org.springframework.boot.orm.jpa.hibernate.SpringImplicitNamingStrategy
```
위와 같은 네이밍 정책을 설정을 해도 먹질 않는 문제가 있다.  
HikariDataSource를 통해 LocalContainerEntityManagerFactoryBean을 생성했더니   
CamelCase -> SnakeCase 변환이 되지 않고 그대로 CamelCase로 전략을 잡아버리는 현상이 발생했다.  

```java
protected Map<String, Object> jpaProperties() {
    Map<String, Object> props = new HashMap<>();
    props.put("hibernate.physical_naming_strategy", SpringPhysicalNamingStrategy.class.getName());
    props.put("hibernate.implicit_naming_strategy", SpringImplicitNamingStrategy.class.getName());
    return props;
}

@Primary
@Bean(name = "internalEntityManagerFactory")
public LocalContainerEntityManagerFactoryBean internalEntityManagerFactory(EntityManagerFactoryBuilder builder) {
    return builder
            .dataSource(internalDataSource)
            .packages("package")
            .persistenceUnit("internal")
            .properties(jpaProperties())
            .build();
}
```

위와 같이 LocalContainerEntityManagerFactoryBean을 생성할 때  
직접 Proeprty를 Builder를 통해 전달하면 해당 네이밍 정책에 대한 설정이 가능하다.