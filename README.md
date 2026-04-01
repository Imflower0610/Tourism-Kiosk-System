# 서남부 관광정보 플랫폼

## Overview
전자정부프레임워크 기반 키오스크 연계 및 관제 시스템.

키오스크와 서버 간 데이터 통신, 실시간 상태 모니터링,  
원격 제어 기능을 포함한 통합 관리 시스템을 설계 및 구축.

지리 데이터(GeoJSON) 기반 경로 보정 로직과 OSRM을 활용하여  
실제 이동 가능한 경로를 반영하고 API 비용을 최적화.

---

## Architecture

Kiosk Client  
→ REST API Server (eGovFrame)  
→ WebSocket Server (Real-time Monitoring)  
→ C# Sub Server (Security Separation)  
→ MariaDB  

Admin Web  
→ Monitoring / Control Panel  

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

### 5. Map Service
- Google Maps 기반 지도 표출 (다국어 지원)
- Leaflet.js 활용 커스텀 지도 인터페이스 구현

**+ 경로 계산 보정 로직**
- OSRM 기반 경로 탐색 시스템 구축
- GeoJSON 데이터를 활용하여 섬/내륙 및 연결 여부 구분
- 단절된 지형(섬 지역) 경로 예외 처리 로직 구현
- 실제 이동 가능한 경로 기준으로 결과 보정

- 외부 API 의존도를 줄이기 위해 OSRM 활용하여 비용 최적화

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
