# 📱 QR 기반 공용 기자재 예약·반납 시스템 (QR Equipment System)

> **학과 공용 기자재의 수기 대여/반납 불편함을 해소하고, QR 코드를 활용해 실시간 대여 상태를 관리하는 모바일 웹 플랫폼입니다.**

---

## 📌 프로젝트 개요

* **프로젝트명**: QR 기반 공용 기자재 예약·반납 시스템
* **주요 사용자**:
  * **일반 사용자 (학생)**: 모바일 카메라로 QR을 스캔하여 기자재 대여/반납
  * **관리자 (조교/학생회)**: 기자재 등록/수정, 전체 대여 현황 및 연체 관리
* **핵심 기능**: QR 코드 스캔, 실시간 상태 업데이트, JWT 기반 권한 제어, Docker/AWS 배포

---

## 🛠 기술 스택 (Tech Stack)

| 구분 | 기술 스택 |
| :--- | :--- |
| **Backend** | Java 17, Spring Boot 3.x, Spring Data JPA, Spring Security, JWT, Swagger |
| **Frontend** | React, JavaScript (ES6+), Tailwind CSS, Axios, html5-qrcode |
| **Database** | PostgreSQL |
| **DevOps** | Docker, Docker Compose, AWS EC2, Nginx, Certbot (SSL) |

---

## 🗺 마일스톤 및 주요 개발 내역

| 핵심 목표 | 구현 내용 | 주요 문제 및 해결 방법 (Trouble Shooting) |
| :--- | :--- | :--- |
| **기획 및 MVP 스코프 정의** | 요구사항 명세서 및 MVP 백로그 작성 | 스코프 과다 확장 문제 발생 $\rightarrow$ 'QR 스캔, 상태 변경, 이력 관리' 핵심 기능으로 축소 조정 |
| **UI/UX 및 화면 플로우 설계** | IA 설계 및 Mobile-First 와이어프레임 작성 | 모바일 QR 스캔 영역 비율 깨짐 $\rightarrow$ 카메라 뷰포트를 1:1 비율로 재설계하여 해결 |
| **PostgreSQL DB 스키마 설계** | `users`, `equipments`, `rentals`, `qr_codes` DDL 작성 | 외래키 Type Mismatch 에러 $\rightarrow$ PK(`BIGINT`)와 FK 타입을 동일하게 맞춰 해결 |
| **Spring Boot 세팅 & Swagger** | 기본 패키지 구조 세팅 및 Swagger UI 연동 | Swagger 404 에러 $\rightarrow$ Spring Boot 3.x 호환 `springdoc-openapi`로 라이브러리 교체 |
| **기자재 CRUD API 구현** | 기자재 등록, 목록/상세 조회 API & JPA 연동 | 프론트-백엔드 분류 ID 타입 불일치(400 에러) $\rightarrow$ 숫자 타입 통일 및 DTO 검증 추가 |
| **Spring Security & JWT 인증** | 로그인 API, JWT 토큰 발급 및 권한(USER/ADMIN) 제어 | 관리자 API 403 에러 $\rightarrow$ 토큰 생성 시 `ROLE_ADMIN` 접두사 명시하여 인가 처리 | 
| **React + Tailwind UI 개발** | 메인, 기자재 리스트, 마이페이지 모바일 UI 구축 | 페이지 전환 시 State 유지 버그 $\rightarrow$ `location.key` 및 `useEffect Cleanup` 적용 |
| **Axios API 연동 & CORS 처리** | Axios Interceptor 기반 토큰 자동 주입 및 비동기 연동 | CORS 에러로 API 차단 $\rightarrow$ WebMvcConfigurer에 프론트 Origin(`:5173`) 허용 구성을 추가 |
| **카메라 QR 스캐너 & 대여 로직** | `html5-qrcode` 카메라 연동 및 실시간 대여 처리 | 동일 QR 중복 스캔 버그 $\rightarrow$ 스캔 즉시 카메라 `pause()` 및 Throttle 제어 적용 |
| **예외 처리 공통화 & 단위 테스트** | `@RestControllerAdvice` 에러 응답 및 JUnit5 테스트 | 중복 대여 시 500 에러 발생 $\rightarrow$ `AlreadyRentedException` 작성 후 409 Conflict 반환 |
| **Docker & AWS EC2 배포** | Multi-stage Docker 빌드 및 AWS EC2 HTTPS 배포 | 모바일 카메라 권한 거부 $\rightarrow$ Certbot 이용해 Nginx에 Let's Encrypt SSL(HTTPS) 적용 |
| **E2E 통합 테스트 & 최종 점검** | 전체 프로세스 시연 테스트 및 최종 문서화 | DB 반납 시각 시차 오류(UTC) $\rightarrow$ Dockerfile 내 `TZ=Asia/Seoul` 설정으로 KST 맞춤 |
