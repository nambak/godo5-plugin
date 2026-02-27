# godomall5-developer

NHN Godo5(고도몰5) 쇼핑몰 개발 전문 플러그인입니다.

## 개요

고도몰5 프로 기반 쇼핑몰을 개발할 때 자동패치 호환성을 유지하면서 커스터마이징할 수 있도록 도와줍니다. 아키텍처 규칙, 코딩 패턴, DB 설계 가이드 등 고도몰5 개발에 필요한 모든 지식을 담고 있습니다.

## 구성 요소

### 스킬

- **godomall5-developer** — 고도몰5 개발 전문 지식. Controller/Component/Widget 확장 패턴, DB 설계 규칙, 프론트엔드 스킨 문법, 에러 처리 패턴 등을 포함합니다. 고도몰, Godo5, 쇼핑몰 커스터마이징 등의 키워드가 나오면 자동으로 트리거됩니다.

### 커맨드

- `/godo-init [프로젝트명]` — 새 고도몰5 프로젝트의 CLAUDE.md를 자동 생성합니다. PHP 버전 감지, 디렉토리 구조 확인 등을 자동으로 수행합니다.
- `/godo-check [파일/디렉토리]` — 코드의 자동패치 호환성을 검사합니다. Bundle 상속 패턴, 바인드 쿼리 사용, 금지 함수 등 8가지 항목을 체크합니다.

## 사용 방법

### 스킬 자동 트리거

고도몰5 관련 작업을 요청하면 스킬이 자동으로 활성화됩니다:

- "장바구니에 수량 제한 기능을 추가해줘"
- "주문 목록에 커스텀 컬럼을 넣고 싶어"
- "회원등급별 상품 접근 권한을 설정해야 해"

### 커맨드 사용

```
/godo-init myfashion        # myfashion 프로젝트 CLAUDE.md 생성
/godo-check module/          # module/ 디렉토리 호환성 검사
/godo-check module/Component/Order/Order.php  # 특정 파일 검사
```

## 참조 자료

스킬 내부에 다음 참조 파일이 포함되어 있습니다:

- `references/project-template.md` — 새 프로젝트용 CLAUDE.md 템플릿
- `references/dependencies.md` — 고도몰5 주요 Composer 의존성 목록
- `references/db-tables.md` — DB 테이블 상수, 주요 설정 필드, gd_policy() 키 참조

## 주요 기능

- 자동패치 호환 코드 생성 (module/ 상속 패턴)
- Controller 라이프사이클 (pre → index → post → after) 가이드
- DBTableField 확장을 통한 안전한 DB 필드 추가
- 바인드 쿼리 강제 (SQL Injection 방지)
- PHP 7.x / 8.x 호환성 자동 체크
- NHN godo 라이선스 헤더 자동 삽입
- 고도몰5 템플릿(Skin) 문법 지원
