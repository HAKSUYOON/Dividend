# 배당금 프로젝트

## 프로젝트 소개
미국 주식 배당금 정보를 제공하는 API 서비스를 개발합니다.

## 기술 스택
- Spring Boot 2.5.6 (JDK 11)
- Gradle
- Dependencies :
  - SpringWeb
  - JPA
  - Redis
  - SpringSecurity
  - h2 Database
  - jsoup
  - jwt
  - commons-collection4 (Trie구조 이용)
  - lombok
  - jnuit
- Redis
- Docker
- POSTMAN

## 주요 기능
### 01. 회원 가입
1) POST - auth/signup
- 회원가입 API
- 중복 ID 는 허용하지 않음
- 패스워드는 암호화된 형태로 저장

2) POST - auth/signin
- 로그인 API
- 회원가입이 되어있고, 아이디/패스워드 정보가 옳은 경우 JWT 발급

### 02. 회사관련 API
1) GET - company/autocomplete
- 자동완성 기능을 위한 API
- 검색하고자 하는 prefix 를 입력으로 받고, 해당 prefix 로 검색되는 회사명 리스트 중 10개 반환

2) GET - company
- 서비스에서 관리하고 있는 모든 회사 목록을 반환
- 반환 결과는 Page 인터페이스 형태

3) POST - company
- 새로운 회사 정보 추가
- 추가하고자 하는 회사의 ticker 를 입력으로 받아 해당 회사의 정보를 스크래핑하고 저장
- 이미 보유하고 있는 회사의 정보일 경우 400 status 코드와 적절한 에러 메시지 반환
- 존재하지 않는 회사 ticker 일 경우 400 status 코드와 적절한 에러 메시지 반환

4) DELETE - company/{ticker}
- ticker 에 해당하는 회사 정보 삭제
- 삭제시 회사의 배당금 정보와 캐시도 모두 삭제

### 03. 배당금관련 API
1) GET - finance/dividend/{companyName}
- 회사 이름을 인풋으로 받아서 해당 회사의 메타 정보와 배당금 정보를 반환
- 잘못된 회사명이 입력으로 들어온 경우 400 status 코드와 에러메시지 반환
- Redis 서버를 통해 저장된 배당금정보 Cache로 유지
- Scheduler 기능을 활용해 매일 0시 배당금 정보 업데이트

### 04. 로그남기기
logback-spring.xml 기능을 활용해 Info레벨 이상의 로그 파일로 남기기

### 05. Database 및 Redis 설정
h2 Database는 Application 서버 종료 시 데이터를 초기화, Redis 서버는 Cache값 항상 유지
