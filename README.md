# 📱 QR 기반 공용 기자재 예약·반납 시스템 (QR Equipment System)

> **학과 공용 기자재의 수기 대여/반납 불편함을 해소하고, QR 코드를 활용해 실시간 대여 상태를 관리하는 모바일 웹 플랫폼입니다.**

---

## 📌 프로젝트 개요

* **프로젝트명**: QR 기반 공용 기자재 예약·반납 시스템
* **개발 기간**: 12주 (기획 ~ UI/UX 설계 ~ 백엔드/프론트엔드 구축 ~ AWS/Docker 배포)
* **타겟 사용자**:
  * **일반 사용자 (학생)**: 학과 기자재(카메라, 노트북, 개발 키트 등)를 신속하게 대여/반납하고자 하는 사용자
  * **관리자 (학과 조교/학생회)**: 기자재 수량, 대여/반납 상태, 연체 목록을 통합 관리하고자 하는 사용자
* **주요 핵심 기능**:
  * 모바일 카메라 기반 QR 코드 스캔 및 기자재 식별
  * 실시간 대여 및 반납 처리 (동시성 제어 및 예외 처리)
  * 대여 상태 및 이력 조회 (마이페이지)
  * JWT + Spring Security 기반 권한별 접근 제어 (USER / ADMIN)
  * Docker 및 AWS EC2 (HTTPS) 기반 안정적 배포

---

## 🛠 기술 스택 (Tech Stack)

* **Backend**: Java 17, Spring Boot 3.x, Spring Data JPA, Spring Security, JWT, Swagger (Springdoc)
* **Frontend**: React, JavaScript (ES6+), Tailwind CSS, Axios, html5-qrcode
* **Database**: PostgreSQL
* **DevOps & Infrastructure**: Docker, Docker Compose, AWS EC2, Nginx, Certbot (SSL/HTTPS)
* **Collaboration & Docs**: Git, GitHub, Markdown

---

## 📅 12주차 프로젝트 진행 및 세부 이력 (Weekly Milestones)

### 🟢 [1주차] 기획 및 요구사항 정의
* **진행상태**: 완료
* **학습시간**: 2h
* **목표**: QR 기반 공용 기자재 예약·반납 시스템 문제 정의 및 타겟 사용자/MVP 범위 설정
* **학습 내용**: 요구사항 분석 기법, 페르소나 설정, 유저 저니 맵(User Journey Map) 작성법
* **구현 기능**: 기획 단계로 기능 구현 없음 (요구사항 명세서 및 MVP 백로그 작성)
* **데이터 흐름**: 해당 사항 없음 (개발 착수 전)
* **문제 및 해결**: 오프라인 결제 및 연체료 부과까지 스코프를 확장하려다 기획 범위를 초과하는 문제가 발생함. 1차 개발 목적에 맞춰 'QR 스캔, 실시간 상태 변경, 대여 이력 관리' 중심으로 스코프를 축소 조정하여 해결함.
* **실행 증빙**: [1주차 요구사항 명세서 및 MVP 스코프 정의 완료](https://github.com/kkimming/qr-equipment-system/blob/main/docs/requirements.md)
* **AI 활용 및 검증**: ChatGPT/Gemini로 기자재 대여 시나리오 예시를 추천받았으나, 실제 학과 기자재 관리 규정과 비교해 불필요한 단계 삭제

---

### 🟢 [2주차] UI/UX 및 화면 액션 플로우 설계
* **진행상태**: 완료
* **학습시간**: 4h
* **목표**: 화면별 UI/UX 흐름도 및 QR 대여/반납 핵심 액션 플로우 설계
* **학습 내용**: UI/UX 컴포넌트 구조 설계, IA(정보구조도), 모바일 웹 QR 스캔 UX 흐름
* **구현 기능**: 설계 단계로 기능 구현 없음 (화면 흐름도 및 Wireframe 명세서 작성)
* **데이터 흐름**: `[사용자] QR 스캔 페이지` -> `카메라 권한 승인` -> `QR 인식` -> `기자재 정보 확인` -> `대여 승인`
* **문제 및 해결**: 데스크톱 기준 뷰포트만 고려하여 레이아웃을 짜다 보니 모바일 스크린에서 QR 스캔 영역 비율이 깨짐. Mobile-First 디자인 규칙을 적용하고 카메라 영역을 1:1 비율로 재설계하여 레이아웃 깨짐을 해결함.
* **실행 증빙**: [2주차 화면 흐름도 및 Wireframe 명세서](https://github.com/kkimming/qr-equipment-system/blob/main/docs/wireframe.md)
* **AI 활용 및 검증**: AI가 추천한 레이아웃 구조를 활용하였으나 실제 모바일 브라우저의 주소창 영역을 고려하여 뷰포트 높이값 재조정

---

### 🟢 [3주차] DB 스키마 설계 및 DDL 스크립트 작성
* **진행상태**: 완료
* **학습시간**: 6h
* **목표**: PostgreSQL 기반 DB 스키마 설계 및 테이블 DDL 스크립트 작성
* **학습 내용**: RDBMS 관계 설계(1:N, N:M), 외래키 제약조건, PostgreSQL 데이터 타입
* **구현 기능**: `users`, `equipments`, `rentals`, `qr_codes` 테이블 DDL 작성 및 DB 생성
* **데이터 흐름**: `DBeaver Query Tool` -> `PostgreSQL Server` (CREATE TABLE 실행 및 FK 설정)
* **문제 및 해결**: `rentals` 테이블의 `equipment_id` 외래키 생성 시 Type Mismatch 에러가 발생함. 원인을 확인하니 원본 PK는 `BIGINT`인데 FK를 `INTEGER`로 선언한 것이 문제였으며, FK 타입을 `BIGINT`로 일치시켜 DDL 실행을 완료함.
* **실행 증빙**: [3주차 DB 스키마 DDL 작성 완료](https://github.com/kkimming/qr-equipment-system/blob/main/docs/schema.sql)
* **AI 활용 및 검증**: AI가 생성한 ERD DDL을 참고하였으나, 실제 연체 일수 계산을 위한 returned_at 컬럼 타임존 처리(`TIMESTAMP WITH TIME ZONE`) 수정 적용

---

### 🟡 [4주차] Spring Boot 3.x 환경 구축 및 Swagger 연동
* **진행상태**: 시작 전
* **학습시간**: 6h
* **목표**: Spring Boot 3.x 프로젝트 초기 세팅 및 Swagger(Springdoc) API 명세서 구축
* **학습 내용**: Spring Web, Gradle 의존성 관리, RESTful API 설계 원칙, Swagger OpenApi 3.0
* **구현 기능**: 기본 패키지 구조(Controller, Service, Repository, DTO) 세팅, Swagger UI 연동
* **데이터 흐름**: `Client Request` -> `Controller REST Endpoint` -> `Swagger Document 자동 생성`
* **문제 및 해결**: Swagger UI 접속 시 404 Not Found 오류가 발생함. Spring Boot 3.x와 호환되지 않는 구버전 Springfox 라이브러리를 사용한 것이 원인이었으며, `springdoc-openapi-starter-webmvc-ui` 라이브러리로 교체하여 정상 접속을 확인함.
* **실행 증빙**: [4주차 Spring Boot 세팅 및 Swagger 연동 완료](https://github.com/kkimming/qr-equipment-system/commit/4th-week-swagger-init)
* **AI 활용 및 검증**: AI의 REST API 경로 추천을 받아 표준 URI 룰(`/api/v1/equipments`)로 적용 검증

---

### 🟡 [5주차] 기자재 CRUD API 및 JPA 연동
* **진행상태**: 시작 전
* **학습시간**: 7.5h
* **목표**: 기자재 등록·조회 API를 구현하고 PostgreSQL에 정상적으로 저장되는지 확인한다.
* **학습 내용**: Spring Controller-Service-Repository 구조, REST API, JPA 엔티티, 기본키와 외래키
* **구현 기능**: 기자재 등록 API, 전체 기자재 목록 조회 API, 관리자 권한 확인
* **데이터 흐름**: `React 입력 폼` -> `Spring Boot API` -> `Service` -> `Repository` -> `PostgreSQL 저장`
* **문제 및 해결**: 기자재 등록 요청 시 프론트엔드에서 문자열로 전달한 분류 ID와 백엔드 DTO의 숫자 타입이 불일치하여 400 에러가 발생함. 입력값을 숫자로 변환하도록 프론트/백엔드 타입을 통일하고 DTO 범위 검증을 추가하여 해결함.
* **실행 증빙**: [5주차 기자재 CRUD API 구현 및 DB 연동 완료](https://github.com/kkimming/qr-equipment-system/commit/5th-week-crud)
* **AI 활용 및 검증**: JPA 관계 설정 예시를 참고했으나 공식 문서와 비교한 후 필요한 관계만 적용하고 직접 테스트함

---

### 🟡 [6주차] Spring Security & JWT 인증 시스템 구현
* **진행상태**: 시작 전
* **학습시간**: 8h
* **목표**: Spring Security + JWT 기반 회원가입/로그인 및 역할(USER/ADMIN) 권한 제어
* **학습 내용**: Security Filter Chain, JWT Access/Refresh Token 발급, BCrypt 암호화
* **구현 기능**: 로그인 API, 토큰 인증 필터(`JwtAuthenticationFilter`), 권한별 접근 제어
* **데이터 흐름**: `Request Header (Bearer Token)` -> `JwtFilter` -> `SecurityContextHolder 인증 객체 저장`
* **문제 및 해결**: 관리자 전용 API 호출 시 403 Forbidden 에러가 발생함. SecurityConfig 설정은 `hasRole('ADMIN')`을 기대하지만 토큰 내 권한 정보에 `ROLE_` 접두사가 누락되었던 것이 원인이었으며, 토큰 생성 시 `ROLE_ADMIN`으로 접두사를 명시해 권한 인가를 정상화함.
* **실행 증빙**: [6주차 JWT 인증 및 Spring Security 권한 제어 구현](https://github.com/kkimming/qr-equipment-system/commit/6th-week-jwt-auth)
* **AI 활용 및 검증**: AI 추천 SecurityConfig 설정을 최신 Spring Security 6.x 람다 식 문법으로 변환 검증 후 적용

---

### 🟡 [7주차] React + Tailwind CSS 모바일 UI 개발
* **진행상태**: 시작 전
* **학습시간**: 10h
* **목표**: React + Tailwind CSS 기반 모바일 웹 레이아웃 및 주요 화면 구현
* **학습 내용**: React Hooks(useState, useEffect), React Router, Tailwind CSS 반응형 클래스
* **구현 기능**: 메인 화면, 기자재 리스트/상세 페이지, 대여 현황 마이페이지 UI 구현
* **데이터 흐름**: `React User Interface` -> `State 관리` -> `화면 컴포넌트 렌더링`
* **문제 및 해결**: 페이지 전환 시 이전 페이지의 State가 초기화되지 않고 남아있는 버그가 있었음. React Router 컴포넌트에 `location.key` 속성을 부여하고 `useEffect Cleanup` 함수를 도입하여 State가 깔끔히 리셋되도록 수정함.
* **실행 증빙**: [7주차 React 반응형 모바일 웹 UI 작성 완료](https://github.com/kkimming/qr-equipment-system/commit/7th-week-react-ui)
* **AI 활용 및 검증**: AI가 생성한 Tailwind 컴포넌트 중 모바일 터치 영역이 좁은 버튼들을 48px 이상으로 수정하여 가용성 확보

---

### 🟡 [8주차] Axios Interceptor & CORS 연동
* **진행상태**: 시작 전
* **학습시간**: 10h
* **목표**: Axios Interceptor 기반 API 연동 및 CORS 설정, 토큰 자동 주입 구현
* **학습 내용**: Axios Interceptor, CORS WebMvcConfigurer, LocalStorage 토큰 관리
* **구현 기능**: 로그인 연동, 기자재 목록/상세 비동기 데이터 바인딩, Axios 공통 모듈화
* **데이터 흐름**: `React(Axios)` -> `Spring Boot (CORS 허용)` -> `DB 조회` -> `JSON Response` -> `React Rendering`
* **문제 및 해결**: 프론트엔드에서 API 요청 시 CORS(Cross-Origin) 에러로 인해 통신이 차단됨. Spring Boot 백엔드의 SecurityConfig 및 WebMvcConfigurer에 프론트엔드 출처(`http://localhost:5173`) 허용 구성을 추가하여 비동기 연동을 완결함.
* **실행 증빙**: [8주차 Axios API 연동 및 CORS 설정 완결](https://github.com/kkimming/qr-equipment-system/commit/8th-week-cors-integration)
* **AI 활용 및 검증**: AI 가이드에 따라 Axios Interceptor를 구현하되, 토큰 만료 시 401 에러 재요청 로직 검증 후 추가

---

### 🟡 [9주차] 카메라 QR 스캐너 연동 & 대여 로직 구현
* **진행상태**: 시작 전
* **학습시간**: 12h
* **목표**: React 카메라 QR 스캐너 연동 및 QR 토큰 스캔 시 실시간 대여/반납 처리
* **학습 내용**: `html5-qrcode` 라이브러리, QR 식별자 복호화, JPA `@Transactional` 동시성 제어
* **구현 기능**: 모바일 카메라 QR 인식, QR 스캔 기반 대여 처리 및 기자재 상태(`RENTED`) 업데이트
* **데이터 흐름**: `카메라 QR 스캔` -> `QR 식별값 추출` -> `POST /api/v1/rentals/scan` -> `DB 상태 변경` -> `완료 알림`
* **문제 및 해결**: QR 인식 프레임 주기가 짧아 동일한 QR 코드가 1초에 여러 번 연속 스캔되면서 중복 대여 요청이 발생하는 문제가 생김. 스캔 성공 즉시 카메라 동작을 `pause()`하고 Throttle 로직을 걸어 1회만 요청되도록 제어함.
* **실행 증빙**: [9주차 html5-qrcode 카메라 연동 및 대여 로직 완성](https://github.com/kkimming/qr-equipment-system/commit/9th-week-qr-logic)
* **AI 활용 및 검증**: AI가 추천한 QR 스캔 라이브러리 테스트 후, 모바일 브라우저 카메라 접근성이 가장 우수한 `html5-qrcode` 선택

---

### 🟡 [10주차] 예외 처리 공통화 & JUnit5 단위 테스트
* **진행상태**: 시작 전
* **학습시간**: 8h
* **목표**: 백엔드 예외 처리 강화, Valid 검증 및 JUnit5/Mockito 단위 테스트 작성
* **학습 내용**: Spring Validation(`@Valid`), `@RestControllerAdvice`, JUnit5 & Mockito
* **구현 기능**: 공통 에러 응답 DTO, 이미 대여 중인 기자재 중복 대여 예외 처리, 비즈니스 로직 테스트
* **데이터 흐름**: `잘못된 요청` -> `@Valid / Custom Exception` -> `@ExceptionHandler 감지` -> `에러 JSON 반환`
* **문제 및 해결**: 이미 대여 중인 기자재 스캔 시 500 Internal Server Error가 반환되는 현상이 발생함. 핸들러 미등록 문제로 파악되어 `@RestControllerAdvice`에 커스텀 예외(`AlreadyRentedException`) 메서드를 추가하고 명확한 409 Conflict 응답을 주도록 예외 처리를 정교화함.
* **실행 증빙**: [10주차 예외 처리 공통화 및 JUnit5 단위 테스트 작성](https://github.com/kkimming/qr-equipment-system/commit/10th-week-unit-test)
* **AI 활용 및 검증**: AI를 활용해 Mockito 테스트용 객체 생성 코드를 작성하고, 실제 예외 상황 케이스 직접 검증

---

### 🟡 [11주차] Docker 컨테이너화 & AWS EC2 (HTTPS) 배포
* **진행상태**: 시작 전
* **학습시간**: 10h
* **목표**: Docker Containerization 및 AWS EC2 + Nginx (HTTPS) 배포 환경 구축
* **학습 내용**: Dockerfile Multi-stage build, docker-compose, Nginx Reverse Proxy, Certbot SSL
* **구현 기능**: React 빌드 파일 + Spring Boot Jar + PostgreSQL 단일 컨테이너화 및 서버 배포
* **데이터 흐름**: `Client` -> `AWS EC2 (Nginx HTTPS :443)` -> `Docker (Spring Boot :8080 / DB :5432)`
* **문제 및 해결**: 모바일 기기에서 웹 스캐너 접속 시 카메라 권한 요청이 거부되는 문제 발생. 브라우저 보안 규정상 WebRTC/카메라 기능은 HTTPS 보안 프로토콜에서만 허용되는 것이 원인이었으며, Certbot을 이용해 Nginx에 Let's Encrypt SSL 인증서를 적용함으로써 해결함.
* **실행 증빙**: [11주차 Dockerfile 작성 및 AWS EC2 HTTPS 배포 완료](https://github.com/kkimming/qr-equipment-system/commit/11th-week-docker-deploy)
* **AI 활용 및 검증**: Nginx Reverse Proxy 설정 파일 작성 시 AI 가이드를 참조하였으나, SSL 적용 경로는 서버 환경에 맞게 직접 수정

---

### 🟡 [12주차] E2E 통합 테스트 및 최종 시연 문서화
* **진행상태**: 시작 전
* **학습시간**: 6h
* **목표**: 전체 시나리오 통합 테스트, README.md 작성 및 프로젝트 최종 발표 자료 작성
* **학습 내용**: 기술 문서 작성법, 시연 시나리오 구성, 프로젝트 회고
* **구현 기능**: 전체 기능 엔드투엔드(E2E) 점검 및 UI 튜닝, 최종 코드 리팩토링
* **데이터 흐름**: 전체 시스템 (QR 스캔 -> 예약/대여 -> DB 저장 -> 반납) E2E 검증
* **문제 및 해결**: E2E 시연 테스트 중 반납 일시가 한국 시간보다 9시간 늦은 UTC 시간으로 DB에 기록되는 버그 발생. Docker 컨테이너의 기본 타임존 미설정이 원인이었으며, Dockerfile 내 `TZ=Asia/Seoul` 환경변수를 설정하여 KST 시간으로 정상 등록되게 수정함.
* **실행 증빙**: [12주차 README.md 문서화 및 최종 시연 점검 완료](https://github.com/kkimming/qr-equipment-system/blob/main/README.md)
* **AI 활용 및 검증**: AI를 통해 README.md 기본 양식을 다듬고, 프로젝트 시스템 아키텍처 구조도 Mermaid 다이어그램 추가
