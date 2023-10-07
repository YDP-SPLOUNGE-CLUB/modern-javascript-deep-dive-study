# 44. REST API

> REST (Representational State Transfer) 는 HTTP 를 기반으로 클라이언트가 서버의 리소스에 접근하는 방식을 규정한 아키텍처이다.
> REST API 는 REST 기반으로 서비스 API 를 구현한 것을 의미한다.

## 44.1 REST API 의 구성

> REST API 는 자원 행위 표현의 3 가지 요소로 구성된다.
> REST 는 자체 표현 구조로 구성되어 REST API 만으로 HTTP 요청의 내용을 이해할 수 있다.

- 자원
  - 자원
  - UR

- 행위
  - 자원에 대한 행위
  - HTTP 요청 메서드

- 표현
  - 자원에 대한 행위의 구체적 내용
  - 페이로드

## 44.2 REST API 설계 원칙

1. URL는 리소스를 표현해야 한다.

- 리소스를 식별할 수 있는 이름은 동사보다는 명사를 사용한다.

2. 리소스에 대한 행위는 HTTP 요청 메서드로 표현한다.

- GET
  - index/retrieve
  - 모든/특정 리소스 취득
  - 페이로드 X

- POST
  - create
  - 리소스 생성
  - 페이로드 O

- PUT
  - replace
  - 리소스 전체 교체
  - 페이로드 O

- PATCH
  - modify
  - 리소스 일부 수정
  - 페이로드 O

- DELETE
  - delete
  - 모든/특정 리소스 삭제
  - 페이로드 X

## 44.3 JSON Server 를 이용한 REST API 실습

이후 생략


---

- [ ] REST API 란
- [ ] RESTful 하다란
