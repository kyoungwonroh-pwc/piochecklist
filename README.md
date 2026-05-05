# 📊 PwC 재고실사 자동화 플랫폼 (Inventory Count Automation)

> **PwC 감사 실무 기반의 재고실사 전 과정 디지털화 솔루션**  
> Excel 업로드부터 샘플링, 이중검증, 차이분석, 결과 다운로드까지 — PC와 모바일에서 실시간 동기화되는 웹 기반 통합 플랫폼

![Tech](https://img.shields.io/badge/Frontend-Vanilla_JS-yellow) ![Tech](https://img.shields.io/badge/Backend-Supabase-green) ![Tech](https://img.shields.io/badge/Deploy-GitHub_Pages-black) ![Status](https://img.shields.io/badge/Status-Production-blue)

---

## 🎯 프로젝트 개요

본 프로그램은 **PwC 감사 방법론(PIO: Physical Inventory Observation)** 에 기반하여, 기존 수작업 중심의 재고실사 프로세스를 **디지털 자동화**한 웹 기반 플랫폼입니다.  
감사팀과 피감사기업이 **동일한 데이터를 실시간으로 공유**하며, Sheet→Floor, Floor→Sheet 양방향 이중검증을 모바일 환경에서도 끊김없이 수행할 수 있도록 설계되었습니다.

---

## ✨ 주요 기능

### 📁 Step 1. 파일 업로드
- 회사 ERP에서 추출한 **원재료(RM) / 재공품(WIP) / 완제품(FG)** Excel 파일을 드래그앤드롭으로 업로드
- `.xls` / `.xlsx` 형식 자동 파싱 (SheetJS 라이브러리 활용)

### 🗂️ Step 2. 헤더 매핑
- 회사별로 다른 Excel 컬럼명을 **표준 필드(품목코드/품명/규격/수량/단위/위치)** 에 자동 매핑
- 한/영 혼용 컬럼 키워드 스마트 인식

### 🎲 Step 3. 샘플링 설정
- **금액 기준 상위 N%** + **난수 기반 표본** 혼합 샘플링 알고리즘
- 감사 유의성(Materiality)을 고려한 자동 추출

### 📋 Step 4. Sheet → Floor 실사
- 장부 상 품목이 **현장에 실제 존재하는지** 검증
- 실사 수량 입력 → 차이 자동 계산 → 상태 표시 (일치/불일치)

### 🔍 Step 5. Floor → Sheet 실사 (모바일 최적화)
- 현장에서 발견된 품목이 **장부에 등재되어 있는지** 역방향 검증
- 품목코드/품명 실시간 검색 드롭다운
- 동일 품목도 위치/수량이 다르면 **중복 등록 허용**

### 📊 Step 6. 결과 & 다운로드
- 카테고리별 실사 결과 자동 집계 (일치율 / 차이금액 / 비고)
- **PwC 표준 PIO 템플릿 형식**으로 Excel 내보내기

---

## 🔐 사용자 인증 & 권한

- Supabase Auth 기반 **login_id + 비밀번호** 로그인 (사내 ID 체계 호환)
- **관리자(admin) / 일반사용자(user)** 역할 분리
- 관리자 대시보드: 사용자 승인, 세션 모니터링, 템플릿 관리

---

## ☁️ 실시간 동기화

- **PC → 모바일** 세션 연속성: 사무실에서 시작한 실사를 현장 모바일에서 이어 진행
- **다중 사용자 협업**: 회사 담당자와 PwC 입회자가 같은 세션을 동시 업데이트
- Supabase Realtime 채널 기반 상태 반영

---

## 🧰 기술 스택

| 영역 | 기술 |
|---|---|
| **Frontend** | HTML5 · CSS3 · Vanilla JavaScript (No Framework) |
| **Library** | SheetJS (xlsx) · Supabase JS Client |
| **Backend** | Supabase (PostgreSQL + Auth + Storage + Realtime) |
| **Security** | Row Level Security (RLS) · JWT Token |
| **Deploy** | GitHub Pages (Static Hosting) |
| **Design** | PwC Brand Identity (Orange #D04A02, Inter Font) |

---

## 📐 시스템 아키텍처

┌────────────────┐      ┌──────────────────┐      ┌────────────────┐
│  사용자(PC)     │◄────►│                  │◄────►│  사용자(모바일) │
│  - 관리자       │      │    Supabase      │      │  - 현장 실사자  │
│  - 감사팀       │      │  Auth + DB +     │      │                │
└────────────────┘      │  Storage + RLS   │      └────────────────┘
└──────────────────┘
▲
│
┌────────┴─────────┐
│  GitHub Pages    │
│  (HTML/CSS/JS)   │
└──────────────────┘
---

## 🗄️ 데이터베이스 설계

- **profiles** : 사용자 정보 (login_id, role, 가입일)
- **count_sessions** : 실사 세션 (제목, 진행단계, 작성자)
- **inventory_lists** : 원본 재고 데이터 (카테고리별 저장)
- **sampled_items** : 샘플링 결과
- **count_results** : 실사 결과 (회사측/PwC측 수량, 차이)
- **template_files** : 관리자 업로드 템플릿
- 모든 테이블에 **RLS 정책** 적용 — 본인 데이터만 접근

---

## 🚀 시작하기

### 1. 저장소 클론
```bash
git clone https://github.com/yourname/inventory-count.git
2. Supabase 설정
Supabase 프로젝트 생성 후 supabase_schema_v8.sql 실행
SUPABASE_URL, SUPABASE_ANON_KEY를 HTML 상단에 입력
3. GitHub Pages 배포
저장소 Settings → Pages → main 브랜치 선택 → Save
생성된 URL을 Supabase Auth → URL Configuration에 등록
📊 성과 및 효과지표Before (수작업)After (본 프로그램)샘플링 소요시간약 2시간30초 (자동)데이터 전달 오류빈번 (수기 입력)0건 (실시간 DB)현장 실사 방식종이 + 펜모바일 웹 브라우저결과 집계반나절 (Excel 수작업)즉시 (자동 다운로드)이중검증부분 수행100% Sheet↔Floor 검증📜 라이선스본 프로젝트는 PwC 내부 교육 및 방법론 확산 목적으로 개발되었습니다.👤 개발자[본인 이름] | PwC Assurance | [이메일]
"감사의 디지털 트랜스포메이션, 현장에서 시작합니다."
