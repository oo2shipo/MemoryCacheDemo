# MemoryCacheDemo

## 개발 환경
* JDK 1.8
* IntelliJ
* JUnit4
* Logback 1.2.3

## 프로젝트 특성
* 캐시 사이즈 설정
* 캐시 사이즈가 부족할 경우, victim 전략으로 LRU 정책을 사용
* 캐시에 객체 추가시 TTL 적용 가능
* 캐시 함수의 스레드 사용을 고려해서 동기화 처리
* JUnit4 기반 테스트 코드 작성
* Logback 기반 로그 작성

## 프로젝트 실행
* 실행 명령어를 실행하면 jar 파일 생성 및 JUnit 모듈이 실행
* Jar 파일 : target 폴더에 MemoryCacheDemo-1.0-SNAPSHOT.jar 생성
* JUnit 모듈 실행 : NumberFormatException 익셉션 발생 포함 (NumberFormatException -> InvalidTargetObjectTypeException)

```bash
mvn clean package
```

## 주요 함수 설명
* put
  * 기존 값이 있으면 old value를 반환
  * 동기화가 지원되는 ConcurrentHashMap를 사용해서 key, value를 관리
  * TTL 값을 초단위로 설정하며 기간 경과시에 스레드에서 자동 소멸
  * victim 전략으로 채용된 LRU 구현을 ArrayList에서 Key 값을 관리해서 구현
```java
    V put(K key, V value)
    V put(K key, V value, int ttlSeconds)
```
* get
  * Hit 된 시간을 갱신하는 모듈을 활서화 할 경우, 능동적인 TTL 처리가 가능
```java
    V get(K key)
```
* addAndGet
  * 기존 값을 하나 증가해서 저장한 후에 새로운 값을 반환
  * 기존 값이 numeric이 아닌 경우, InvalidTargetObjectTypeException 익셉션을 발생
```java
    public synchronized V addAndGet(K key) throws InvalidTargetObjectTypeException {
        ....
    }
```
* cleanup
  * 1초 주기로 TTL이 경과 된 값을 자동 삭제
```java
    void cleanUp()
```

## 테스트 모듈 설명
* Put / Get 테스트
* AddAndGet 테스트
* AddAndGet Exceptio 테스트 : InvalidTargetObjectTypeException
* TTL 테스트
* LRU Cache 테스트
* 캐시 스레드 테스트 (TTL + log 기능 포함)


## 로그 모듈 설명
* {프로젝트경로}\log 폴더에 로그 파일이 생성된다.
* 하루 단위로 로그를 생성한다.
* 작성한 로그 파일의 크기가 100kb 이상이면 새로운 로그 파일 생성

> logback.xml
```bash
<?xml version="1.0" encoding="UTF-8"?>
<configuration debug="true"  scan="true" scanperiod="30 seconds">

  <!--콘솔 로그 설정-->
  <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
    <layout class="ch.qos.logback.classic.PatternLayout">
      <Pattern>%d{HH:mm:ss} %-5level %logger{36} - %msg%n</Pattern>
    </layout>
  </appender>

  <!--파일 로그 설정-->
  <appender name="ROLLING" class="ch.qos.logback.core.rolling.RollingFileAppender">
    <file>log\\momory_cache.log</file>
    <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
      <!-- 하루 단위 파일 생성 -->
      <fileNamePattern>log\\momory_cache.%d{yyyy-MM-dd}.%i.out</fileNamePattern>
      <timeBasedFileNamingAndTriggeringPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP">
        <!-- 특정 크기 초과시 새로운 파일 생성 ex) 10KB, 100MB -->
        <maxFileSize>100KB</maxFileSize>
      </timeBasedFileNamingAndTriggeringPolicy>
    </rollingPolicy>
    <encoder>
      <Pattern>%d{yyyy-MM-dd HH:mm:ss} %-5level %logger{36} - %msg%n</Pattern>
    </encoder>
  </appender>
  
  <root level="DEBUG">
    <appender-ref ref="ROLLING"/>
    <appender-ref ref="STDOUT" />
  </root>

</configuration>
```
