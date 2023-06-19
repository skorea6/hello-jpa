# JPA 소개

1. [영속성 관리](./1.%20영속성%20관리.md)
2. [엔티티 매핑](./2.%20엔티티%20매핑.md)
3. [연관관계 매핑](./3.%20연관관계%20매핑.md)
4. [다양한 연관관계 매핑](./4.%20다양한%20연관관계%20매핑.md)
5. [고급 매핑](./5.%20고급%20매핑.md)
6. [프록시와 연관관계 관리](./6.%20프록시와%20연관관계%20관리.md)
7. [값 타입](./7.%20값%20타입.md)
8. [JQPL](./8.%20JQPL.md)
#### 지금 시대는 객체를 관계형 DB에 관리

## 연관관계
- 객체는 참조를 사용: member.getTeam()
- 테이블은 외래 키를 사용: JOIN ON M.TEAM_ID = T.TEAM_ID

## ORM?
- Object-relational mapping(객체 관계 매핑)
- 객체는 객체대로 설계
- 관계형 데이터베이스는 관계형 데이터베이스대로 설계
- ORM 프레임워크가 중간에서 매핑
- 대중적인 언어에는 대부분 ORM 기술이 존재

#### JPA는 애플리케이션과 JDBC 사이에서 동작

## JPA를 왜 사용해야 하는가?
- SQL 중심적인 개발에서 '객체''중심으로 개발
- 생산성
- 유지보수
- 패러다임의 불일치 해결
- 성능
- 데이터 접근 추상화와 벤더 독립성
- 표준

## JPA의 성능 최적화 기능
1. 1차 캐시와 동일성(identity) 보장
	- 같은 트랜잭션 안에서는 같은 엔티티를 반환 (약간의 조회 성능 향상)
	- DB Isolation Level이 Read Commit이어도 애플리케이션에서 Repeatable Read 보장
2. 트랜잭션을 지원하는 쓰기 지연(transactional write-behind)
	- 트랜잭션을 커밋할 때까지 INSERT SQL을 모음
	- JDBC BATCH SQL 기능을 사용해서 한번에 SQL 전송

	- UPDATE, DELETE로 인한 로우(ROW)락 시간 최소화
	- 트랜잭션 커밋 시 UPDATE, DELETE SQL 실행하고, 바로 커밋
3. 지연 로딩(Lazy Loading)
	- 지연로딩: 객체가 실제 사용될때 로딩
	- 즉시로딩: JOIN SQL로 한번에 연관된 객체까지 미리 조회


## 데이터베이스 방언
- JPA는 특정 데이터베이스에 종속 X
- 각각의 데이터베이스가 제공하는 SQL 문법과 함수는 조금씩 다름
	- 가변 문자: MySQL은 VARCHAR, Oracle은 VARCHAR2
	- 문자열을 자르는 함수: SQL 표준은 SUBSTRING(), Oracle은 SUBSTR()
	- 페이징: MySQL은 LIMIT, Oracle은 ROWNUM
- 방언: SQL 표준을 지키지 않는 특정 데이터베이스만의 고유한 기능


## JPA 구동 방식
Persistence -> 생성 -> EntityManagerFactory -> 생성 -> EntityManager 여러개
- @Entity : JPA가 관리할 객체
- @Id : 데이터베이스 PK와 매핑

(주의)
- EntityManagerFactory는 하나만 생성해서 애플리케이션 전체에서 공유
- EntityManager는 쓰레드간에 공유X (사용하고 버려야 한다)
- JPA의 모든 데이터 변경은 트랜잭션 안에서 실행


## JPQL
- JPA는 SQL을 추상화한 JPQL이라는 객체 지향 쿼리 언어 제공
- SQL과 문법 유사, SELECT, FROM, WHERE, GROUP BY,HAVING, JOIN 지원
- JPQL은 '엔티티 객체'를 대상으로 쿼리, SQL은 '데이터베이스 테이블'을 대상으로 쿼리
- 테이블이 아닌 객체를 대상으로 검색하는 '객체 지향 쿼리'!
- SQL을 추상화해서 특정 데이터베이스 SQL에 의존X
