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


## 주요 함수 설명
* put

```java
    V put(K key, V value)
```

## 테스트 모듈 설명
* Put / Get 테스트
* AddAndGet 테스트
* AddAndGet Exceptio 테스트 : InvalidTargetObjectTypeException
* TTL 테스트
* LRU Cache 테스트
* 캐시 스레드 테스트 (TTL + log 기능 포함)


## 로그 모듈 설명
*

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
        <maxFileSize>10KB</maxFileSize>
      </timeBasedFileNamingAndTriggeringPolicy>
    </rollingPolicy>
    <encoder>
      <Pattern>%d{yyyy-MM-dd HH:mm:ss} %-5level %logger{36} - %msg%n</Pattern>
    </encoder>
  </appender>
```
