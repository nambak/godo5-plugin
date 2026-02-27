# 고도몰5 주요 DB 테이블 상수 및 설정 참조

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
| Comment | 필수 |
| 금지 접두사 | `es_`, `zz_` |
| 커스텀 접두사 | `dpx_` 권장 |
