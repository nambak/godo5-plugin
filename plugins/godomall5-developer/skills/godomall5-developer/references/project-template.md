# CLAUDE.md — 고도몰5 프로젝트 템플릿

> 이 파일은 새 고도몰5 프로젝트 시작 시 CLAUDE.md로 복사하여 사용하세요.
> `[프로젝트명]` 등 대괄호 부분을 프로젝트에 맞게 수정하세요.

---

## 프로젝트 개요

- **프로젝트명**: [프로젝트명]
- **플랫폼**: GodoMall 5 (고도몰5) — NHN COMMERCE Corp. PHP 기반 이커머스
- **PHP 버전**: [7.x / 8.x]
- **라이선스**: 상업용
- **공식 매뉴얼**: http://manual.godomall5.godomall.com/data/manual_map.php
- **튜닝 가이드**: https://devcenter-help.nhn-commerce.com/

---

## 아키텍처

### 요청 흐름

```
HTTP Request
  → Controller (Admin/Front/Mobile)
    → Service/Component (비즈니스 로직)
      → Repository (데이터 접근)
        → Database (Eloquent ORM / 레거시 DB 클래스)
```

### 디렉토리 구조

```
[프로젝트명]/
├── Bundle/                    # 코어 프레임워크 (수정 금지, 자동패치 대상)
│   ├── Component/             # 비즈니스 로직
│   ├── Controller/            # 컨트롤러 (Admin/Front/Mobile)
│   ├── Service/               # 서비스 레이어
│   ├── Repository/            # 데이터 접근 (Eloquent ORM)
│   ├── DTO/                   # 데이터 전송 객체
│   └── Widget/                # UI 위젯
│
├── module/                    # 커스텀 확장 코드 (여기서 작업)
│   ├── Component/
│   ├── Controller/
│   └── Widget/
│
├── Asset/Admin/               # 관리자 뷰
│   ├── css/admin-custom.css   # 커스텀 CSS (자동 로드)
│   └── script/admin-custom.js # 커스텀 JS (자동 로드)
│
└── data/skin/                 # 프론트엔드 템플릿
    ├── front/{skinName}/
    └── mobile/{skinName}/
```

---

## 핵심 규칙

1. **Bundle 수정 금지** — 모든 커스터마이징은 `module/`에서 상속으로 수행
2. **App::load() 사용** — `new` 직접 생성 금지
3. **바인드 쿼리 필수** — SQL Injection 방지
4. **커스텀 테이블 접두사** — `dpx_` 사용 (es_, zz_ 금지)
5. **관리자 설정 우선 확인** — 코드 수정 전 관리자 페이지 설정 확인

---

## 프로젝트 고유 정보

### 커스텀 기능

<!-- 이 프로젝트에서 개발한 커스텀 기능을 기록하세요 -->

| 기능 | 파일 위치 | 설명 |
|------|----------|------|
| | `module/Component/` | |
| | `module/Controller/` | |

### 커스텀 DB 테이블

<!-- dpx_ 접두사 커스텀 테이블을 기록하세요 -->

| 테이블명 | 용도 |
|----------|------|
| | |

### 외부 연동

<!-- ERP, PG, API 엔드포인트를 기록하세요 -->

| 연동 대상 | 방식 | 엔드포인트 |
|----------|------|-----------|
| | | |

### 주요 파일 경로

<!-- 이 프로젝트에서 자주 수정하는 핵심 파일들 -->

| 파일 | 역할 |
|------|------|
| | |

---

## 개발 환경

```bash
# 의존성 설치
composer install

# 자동로더 재생성
composer dump-autoload
```

---

## 라이선스 헤더 (새 파일 생성 시 필수)

```php
<?php
/**
 * This is commercial software, only users who have purchased a valid license
 * and accept to the terms of the License Agreement can install and use this
 * program.
 *
 * @copyright ⓒ 2016, NHN godo: Corp.
 * @link http://www.godo.co.kr
 */
```
