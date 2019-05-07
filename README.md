# MemoryCacheDemo


## 개발 환경
* JDK 1.8
* IntelliJ
* JUnit4
* Logback 1.2.3


## 주요 기능
* 캐시 사이즈 설정
* 캐시 사이즈가 부족할 경우, victim 전략으로 LRU 정책 사용
* 캐시에 객체 추가시 TTL 적용 가능
* 캐시 함수의 스레드 사용을 고려해서 동기화 처리
* JUnit4 기반 테스트 코드 작성
* Logback 기반 로그 작성
