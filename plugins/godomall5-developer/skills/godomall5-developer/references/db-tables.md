# 고도몰5 주요 DB 테이블 상수 및 설정 참조

## DB 클래스 메서드 레퍼런스

### 주요 메서드

| 메서드 | 설명 |
|--------|------|
| `bind_param_push($arrBind, $type, $value)` | 바인딩 파라미터 추가 (`s`=string, `i`=integer, `d`=decimal) |
| `get_binding($fieldDef, $data, $mode, ...)` | DBTableField 정의 기반 INSERT/UPDATE 바인딩 데이터 추출 |
| `query_fetch($query, $arrBind, $multiple)` | SELECT 결과 반환 (true=여러 행, false=단일 행) |
| `query_complete($query, $join, $where, ...)` | 쿼리 요소 조합하여 완성된 쿼리문 생성 |
| `set_insert_db($table, $param, $bind, ...)` | INSERT 실행 |
| `set_update_db($table, $param, $where, $bind)` | UPDATE 실행, 영향받은 행 수 반환 |
| `set_delete_db($table, $where, $bind)` | DELETE 실행, 영향받은 행 수 반환 |
| `bind_query($query, $arrBind)` | 직접 쿼리 + 바인딩 파라미터 실행 |
| `getCount($query, $arrBind)` | 쿼리 결과 행 개수 반환 |
| `insert_id()` | 마지막 INSERT의 auto_increment 값 |

> **주의**: `query()` 메서드는 완성된 SQL 문자열만 받으며 바인딩 파라미터를 넘길 수 없음. 파라미터가 필요한 경우 반드시 `query_fetch()`, `bind_query()`, `set_insert_db()` 등 바인딩 전용 메서드 사용.

### 트랜잭션

| 방식 | 사용법 |
|------|--------|
| try-catch | `$db->begin_tran()` → 쿼리 실행 → `$db->commit()` / `$db->rollback()` |
| Closure | `\DB::transaction(function () { ... })` — 예외 시 자동 rollback |

---

## 주요 테이블 상수

### 주문 관련
- `DB_ORDER_INFO` — 주문 정보
- `DB_ORDER_DELIVERY` — 배송 정보
- `DB_ORDER_GOODS` — 주문 상품
- `DB_ORDER_GIFT` — 사은품
- `DB_ORDER_COUPON` — 쿠폰 사용
- `DB_ORDER_TAX` — 세금 정보
- `DB_ORDER_CASH_RECEIPT` — 현금영수증

### 기타 핵심 테이블
- `DB_MEMBER` — 회원
- `DB_GOODS` — 상품
- `DB_GOODS_OPTION` — 상품 옵션
- `DB_CART` — 장바구니

## 주요 설정 필드 참조

### 상품 가격 표시 권한 (es_goods)
| 필드 | 설명 | 값 |
|------|------|------|
| `goodsPermission` | 가격 표시 권한 | 'all', 'member', 'group' |
| `goodsPermissionGroup` | 허용 회원 그룹 번호 | |
| `goodsPermissionPriceStringFl` | 대체 문구 사용 여부 | 'y' / 'n' |
| `goodsPermissionPriceString` | 가격 대신 표시할 문구 | |

### 상품 접근 권한 (es_goods)
| 필드 | 설명 | 값 |
|------|------|------|
| `goodsAccess` | 접근 권한 | 'all', 'member', 'group' |
| `goodsAccessGroup` | 허용 회원 그룹 번호 | |
| `goodsAccessDisplayFl` | 비회원 노출 여부 | 'y' / 'n' |

### 관련 클래스
- `Bundle/Component/Goods/Goods.php` — 상품 가격/접근 권한 처리
- `Bundle/Component/Member/PageAccess.php` — 페이지 접근 제어
- `Bundle/Controller/Front/Goods/GoodsViewController.php` — PC 상품 상세
- `Bundle/Controller/Mobile/Goods/GoodsViewController.php` — 모바일 상품 상세

## gd_policy() 주요 키

정책 설정은 `es_config` 테이블에 저장되며 `gd_policy()` 함수로 조회합니다.

```php
gd_policy('goods.gift')                // 사은품 정책
gd_policy('member.auth_cellphone')     // 휴대폰 인증 정책
```

## DB 확장 규칙 요약

| 항목 | 규칙 |
|------|------|
| Engine | InnoDB (필수) |
| Charset | utf8mb4 |
| Collation | utf8mb4_general_ci |
| Primary Key | 필수 |
| Comment | 테이블·컬럼 모두 필수 |
| NotNull 컬럼 | 기본값(Default Value) 지정 필수 |
| 금지 접두사 | `es_`, `zz_` |
| 커스텀 접두사 | `dpx_` 권장 |

## 솔루션 기본 데이터 보호

| 항목 | 규칙 |
|------|------|
| 테이블명·컬럼명 | 수정·삭제 금지 (사이트 오류 발생) |
| 컬럼 Data Type | 수정 금지 (새 컬럼 추가로 대체) |
| DBTableField 원본 | `Bundle/Component/Database/DBTableField.php` |
| 테이블 추가 메서드 | `tableTestTable()` → `es_testTable` |

## 쿼리 작성 가이드라인

| 항목 | 규칙 |
|------|------|
| SELECT 절 | `*` 사용 자제, 필요한 컬럼만 명시 |
| WHERE 절 | 조회 조건 필수 (전체 테이블 조회 금지) |
| 데이터 타입 | 문자형은 `''` 사용 필수 (인덱스 미사용 방지) |
| 조건 컬럼 가공 | `SUBSTR()` 등 함수 적용 금지 → `LIKE` 등으로 대체 |
| JOIN | 사용하지 않는 테이블 JOIN 금지 |
| GROUP BY | 집계 함수 없이 사용 금지 |
