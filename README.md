# 서남부 관광정보 플랫폼

## Overview
전자정부프레임워크 기반 키오스크 연계 및 관제 시스템.

키오스크와 서버 간 데이터 통신, 실시간 상태 모니터링,  
원격 제어 기능을 포함한 통합 관리 시스템을 설계 및 구축.

지리 데이터(GeoJSON) 기반 경로 보정 로직과 OSRM을 활용하여  
실제 이동 가능한 경로를 반영하고 API 비용을 최적화.

---

## Architecture
<img width="1099" height="613" alt="image" src="https://github.com/user-attachments/assets/5d3e8f46-be1b-4617-b12b-512d5d2b39b3" />

### Client Layer

**Kiosk**
- REST API 기반 데이터 요청 (API Key 인증)
- 관광 정보 조회 및 지도/길찾기 기능 사용
- 사용자 이용 데이터 및 통계 전송

**Admin**
- WebSocket 기반 실시간 키오스크 상태 모니터링
- Wake-on-LAN 및 WebSocket을 통한 원격 제어
- 데이터 관리 및 서버 운영 기능
- 허용된 IP 기반 접근 제한 적용

---

### Backend Layer

#### 1. Web Server (IIS + C# Sub Server)

- 외부 인터넷 통신 및 외부 API 연동 처리
- C# 보조 서버를 통한 API 요청 중계 및 보안 분리
- 이미지 및 동영상 파일 직접 서빙
- IIS Reverse Proxy를 통해 WAS 서버와 내부 통신
- Wake-on-LAN 기능 수행 (보조 서버 연동)

-> 외부와 직접 통신하는 유일한 계층으로, 보안 경계 역할 수행

---

#### 2. WAS Server (eGovFrame / Spring)

- 내부 네트워크 전용 (외부 인터넷 차단)
- REST API 및 WebSocket 처리
- 비즈니스 로직 및 데이터 처리 수행
- 관리자 기능 (통계, 로그, 제어) 처리

-> 모든 데이터 처리와 비즈니스 로직이 수행되는 핵심 계층

---

### Database Layer

- MariaDB 기반 데이터베이스 이중화 구성 (DB1 / DB2)
- WAS 서버에서만 접근 가능
- 키오스크 로그, 통계 데이터, 관리자 데이터 저장

**다국어 데이터 구조 설계**
- 다국어 지원을 위한 테이블 구조 설계
- 언어별 콘텐츠 분리 및 관리 구조 구현
- 관리자 페이지에서 다국어 콘텐츠 관리 가능
-> 데이터 접근을 WAS로 제한하여 보안 강화

---

### External APIs

- Google Maps API
- OSRM (경로 탐색 및 비용 최적화)
- 공공 데이터 API

-> 모든 외부 API 호출은 Web Server를 통해서만 수행

---

## Security & Design Points

- WAS 서버는 외부 인터넷과 완전 분리 (보안 강화)
- Web Server를 통한 Reverse Proxy 구조로 API 접근 제어
- 관리자 페이지는 IP 기반 접근 제한 적용
- 키오스크는 API Key 기반 인증
- DB는 WAS 서버만 접근 가능하도록 제한

---

## Data Flow

### Kiosk Flow
1. Kiosk → API 요청 (API Key 인증)
2. Web Server → WAS로 요청 전달 (Reverse Proxy)
3. WAS → DB 조회 및 처리
4. WAS → Web Server → Kiosk 응답

### Admin Flow
1. Admin → Web Server 접속 (IP 제한)
2. Web Server → WAS 연결
3. WAS → WebSocket을 통해 실시간 상태 전달
4. Admin → 키오스크 제어 요청 (WebSocket / API)
5. Web Server → C# 보조 서버 → Wake-on-LAN 실행

- 외부 API 호출은 Web Server에서만 수행

---

## Core Features

### 1. Data Communication (API)
- 키오스크 ↔ 서버 간 데이터 송수신
- 사용자 이용 데이터 및 통계 수집

### 2. Real-time Monitoring (WebSocket)
- 키오스크 연결 상태 실시간 확인
- 관리자 페이지에서 상태 시각화

### 3. Remote Control System
- Wake-on-LAN 기반 원격 전원 제어
- WebSocket을 통한
  - 전원 ON / OFF
  - 재부팅 기능 구현

### 4. Admin System
- 통계 및 로그 조회
- 데이터 관리 기능 (CRUD)
- 서버 초기화 및 운영 기능

### 5. Map & Routing System
- Google Maps 기반 지도 표출 (다국어 지원)
- Leaflet.js 기반 커스텀 지도 인터페이스 구현

**경로 최적화 및 보정 로직**
- OSRM 기반 경로 탐색 시스템 구축
- GeoJSON을 활용한 지형(섬/내륙) 구분 및 연결 여부 판단
- 단절된 경로에 대한 예외 처리 및 실제 이동 가능 경로 보정
- 외부 API 의존도 감소 및 비용 최적화
---

## Tech Stack

- Java 8, eGovFrame, Spring, MyBatis  
- JavaScript, HTML, CSS  
- MariaDB  
- IIS  
- WebSocket, REST API  
- Wake-on-LAN, OSRM, Google Maps API  

---

## My Contribution

- 시스템 아키텍처 설계 및 전체 구축
- 데이터베이스 설계 및 구축
- API / WebSocket 통신 구조 설계 및 개발
- 관리자 페이지 개발
- 키오스크 연동 및 원격 제어 기능 구현

---

## Key Points

- API + WebSocket 역할 분리 구조 설계
- 실시간 모니터링 및 원격 제어 시스템 구축
- 보안 분리를 위한 이중 서버 구조 적용
- 외부 API 비용 최적화 (OSRM 활용)
