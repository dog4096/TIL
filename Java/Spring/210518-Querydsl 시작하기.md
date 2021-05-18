#Querydsl 시작하기

---

> com.ewerk.gradle.plugins.querydsl의 사용은 피하자.   
> 2018년 이후로 지원이 끊겼으며 최신 버전의 Gradle에서 적용이 좀 곤란하다.

```gradle
dependencies {
    compile("com.querydsl:querydsl-core")
    compile("com.querydsl:querydsl-jpa")
    annotationProcessor("com.querydsl:querydsl-apt:${dependencyManagement.importedProperties['querydsl.version']}:jpa") 
    annotationProcessor("jakarta.persistence:jakarta.persistence-api")
    annotationProcessor("jakarta.annotation:jakarta.annotation-api")
}
```

