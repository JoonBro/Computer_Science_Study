# SPRING

스프링에 대해 공부를 시작해보려합니다.

인프런에서 [스프링 프레임워크 핵심 기술] (https://www.inflearn.com/course/spring-framework_core/lecture/15502?tab=curriculum) 강좌를 수강하며 내용 정리해보겠습니다!

## 학습 목차
- IoC 컨테이너와 빈
- 리소스
- Validation
- 데이터 바인딩
- SpEL(Spring Expression Language)
- 스프링 AOP
- Null-Safety

## 스프링이란?
> "소규모 애플리케이션 또는 기업용 애플리케이션을 자바로 개발하는데 있어 유용하고 편리한 기능을 제공하는 프레임워크"

## ❗️IoC 컨테이너

### 스프링 IoC 컨테이너(Inversion of Control)

> 의존 관계 주입(Dependency Injection)이라고도 하며, 어떤 객체가 사용하는 **의존 객체를 직접 만들어서사용하는게 아니라, 주입 받아 사용하는 방법**을 말한다.

예시를 코드로 보면

```java #문법 표시

```

### 빈

> 스프링 IoC 컨테이너가 관리하는 객체


## Maven - Java 빌드 도구

아파치 메이븐(Apache Maven)은 자바 기반 프로젝트의 **라이프사이클(Life Cycle)**관리를 위한 빌드 도구입니다.

### 라이프사이클(Life Cycle)이란?

메이븐은 프레임워크 중 하나입니다. 따라서 정해진 순서에 따라 동작하며, 이러한 빌드 순서를 라이프사이클이라 합니다.

각 단계 - Phase(기능)에 대해 알아보면

1. Default(Build) : 일반적인 빌드 프로세스를 위한 모델
2. Clean : 빌드 시 생성되었던 파일들을 삭제하는 단계
3. Validate : 프로젝트가 올바른지 확인하고 필요한 모든 정보를 사용할 수 있는지 확인하는 단계
4. Compile : 프로젝트 소스 컴파일하는 단계
5. Test : 유닛(단위)테스트를 수행하는 단계(테스트 실패 시 빌드 실패로 처리, 스킵 가능)
6. Package : 실제 컴파일된 소스코드와 리소스들을 jar, war 등의 파일로 배포를 위한 패키지(압축)로 만드는 단계
7. Verify : 통합 테스트 결과에 대한 검사를 실행하여 품질 기준을 충족하는지 확인하는 단계
8. Install : 패키지를 로컬 저장소에 설치하는 단계
9. Site : 프로젝트 문서와 사이트 작성하는 단계
10. Deploy : 만들어진 Package를 원격저장소에 release하는 단계

최종 빌드 순서는 [Compile] -> [test] -> [package]
- compile : src/main/java 내 모든 소스코드가 컴파일
- test : src/test/java, src/test/resources 테스트 자원 복사 및 테스트 소스 코드 컴파일
- packaging : 컴파일과 테스트 완료 후 jar, war 같은 형태로 압축

각 단계(Phase)는 의존관계를 가지고 있어 이전 Phase가 수행되어야 수행가능합니다. 즉, 모든 빌드 단계는 이전 단계가 성공적으로 실행되었을 때 실행된다는 것이 의존(Dependency)입니다.

이 자바 빌드 도구인 Maven의 특징, 장점, 단점을 알아봅시다.

#### 특징

- 컴파일과 빌드를 동시에 수행합니다.
- 테스트를 병행하거나 서버 측 배포 자원을 관리할 수 있는 환경을 제공합니다.
- 라이브러리 관리 기능도 내포하고 있습니다.
- 필요한 라이브러리를 pom.xml에 정의해 놓으면 해당 라이브러리가 작동하는데 필요한 다른 라이브러리들까지 관리하여 자동으로 다운로드 해줍니다.

#### 장점

- 컴파일과 빌드를 동시에 수행 가능
- 서버의 Deploy자원을 관리할 수 있는 환경 제공
- pom.xml 파일을 통해 관리하므로 개발, 유지보수 측면에서 오픈소스 라이브러리, 프로젝트 등 관리가 용이
- IDE에 종속된 부분들을 제거 가능
- Maven Profile 기능을 통해 배포 설정 파일을 관리하고 배포 파일을 생성 가능

#### 단점

- 메이븐에서 지원하지 않는 빌드 과정을 추가해야 하는 경우 복잡
- 특정 플러그인 설정이 달라지면 재사용성 및 확장성 저하, 가독성 저하, 중복 발생
- 이와 같은 단점들을 해결하기 위해, 그레이들(Gradle)이라는 새로운 빌드 툴 등장 -> 안드로이드 앱 기본 빌드 툴

### 구조

![](https://images.velog.io/images/bbak_joon/post/aadc0022-11de-430b-934b-ef566c5fb4e2/image.png)

### 설정파일

**settings.xml** : 메이븐을 빌드할 떄 의존관계에 있는 라이브러리, 플러그인을 위치 설정할 수 있음.

▶︎ 메이븐 빌드 툴과 관련한 설정파일
▶︎ MAVEN_HOME/conf 디렉토리에 위치(메이븐 설치 시 기본 제공)

**POM(Project Object Model)** : 프로젝트 객체 모델
▶︎ POM은 pom.xml을 뜻하며, 메이븐을 이용하는 프로젝트의 root에 존재하는 파일
▶︎ 프로젝트별 1개, 모든 설정, 의존성 등을 알 수 있다.

예시
```xml
<?xml version="1.0" encoding="UTF-8"?> <project>
  <!-- model version is always 4.0.0 for Maven 2.x POMs -->
  <modelVersion>4.0.0</modelVersion>

  <!-- project coordinates, i.e. a group of values which
       uniquely identify this project -->

  <groupId>com.mycompany.app</groupId>
  <artifactId>my-app</artifactId>
  <version>1.0</version>

  <!-- library dependencies -->

  <dependencies>
    <dependency>

      <!-- coordinates of the required library -->

      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>

      <!-- this dependency is only used for running and compiling tests -->

      <scope>test</scope>

    </dependency>
  </dependencies>
</project>
```

## Gradle

의존성 주입 : QueryDsl로 

kapt : kotlin의 Annotation Processor, 해당 의존성을 어노테이션으로 활용가능하도록(라이브러리) 해주는 plugin(플러그인).

QuerydslConfiguration - spring 컨테이너에 JPAQueryFactory Bean 등록하여 라이브러리 활용할 수 있도록 함

```kotlin

@configuration
class QuerydslConfiguration(
  @PersistenceContext
  val entityManager: EntityManager
)
{
  @Bean
  fun jpaQueryFactory() = JPAQueryFactory(entityManager)
}

```

build.gradle.kts

```kotlin
plugins{
  id("com.ewerk.gradle.plugins.querydsl") version "1.0.10"
  
  kotlin("kapt") version "1.5.0"
}

sourceSets["main"].withConvention(org.jetbrains.kotlin.gradle.plugin.KotlinSourceSet::class){
  kotlin.srcDir("$buildDir/generated/source/kapt/main")
}

~

dependencies{
  implementation("com.querydsl:querydsl-jpa")
  kapt("com.querydsl:querydsl-apt:4.2.2:jpa)
 }
 
 ~
 ```
 

### Reference

[https://namu.wiki/w/Maven](https://namu.wiki/w/Maven)
https://blog.naver.com/dktmrorl/222131777444
https://ko.wikipedia.org/wiki/아파치_메이븐

