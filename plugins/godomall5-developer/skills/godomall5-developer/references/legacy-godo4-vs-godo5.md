# Godo4(레거시) vs Godo5 빠른 식별 가이드

## 1. 왜 이 가이드가 필요한가

`godomall5-developer` 스킬의 모든 패턴(`Bundle/module` 상속, `App::load()`, `\Logger::channel('userLog')`, 바인드 쿼리, `es_*` 테이블)은 **Godo5에서만 동작**합니다. Godo4 또는 인하우스 PHP 시스템에 그대로 적용하면:

- `namespace`, `App::load()` → 파싱/런타임 에러 (PHP 5.2 미지원)
- `\Logger::channel(...)` → 클래스 미존재
- `bind_param_push()` → 메서드 미존재
- `es_order` 테이블 → 존재하지 않음 (Godo4는 `GD_ORDER_ITEM`)

작업 시작 전에 프로젝트가 어느 쪽인지 **3분 안에** 판별하세요.

## 2. 한눈에 보는 비교표

| 신호 | **Godo5** | **Godo4 / 인하우스 레거시** |
|---|---|---|
| **PHP 버전** | 7.x / 8.x | 5.2 / 5.3 |
| **루트 디렉토리 마커** | `Bundle/`, `module/`, `Asset/Admin/`, `data/skin/`, `composer.json` | `shop/`, `shop/lib/GODO/`, `Template_/_compiles/`, `www/`, `PEAR/`, 짧은 `<?` 태그 |
| **DB 함수** | `bind_param_push`, `query_fetch`, `set_insert_db`, Eloquent ORM | `mysql_query()`, `mysql_fetch_array()`, `$db->query()`, `$db->fetch()` |
| **테이블 상수** | `DB_ORDER_INFO` (`es_order`), `DB_MEMBER` (`es_member`) | `GD_ORDER_ITEM`, `GD_MEMBER`, `GD_LOG_EMONEY` |
| **테이블 접두사** | `es_*` (코어), `dpx_*` (커스텀) | `gd_*`, `GD_*` |
| **네임스페이스** | `namespace Bundle\...`, `namespace Component\...` | namespace 없음 — 전역 함수/전역 변수 |
| **인스턴스 생성** | `\App::load(...)`, `\App::getInstance(...)` | `Core::loader(...)`, 전역 `$db` 직접 사용 |
| **세션** | `Framework\Session\Session::get('member.memNo')` | `$_SESSION[...]`, `$cooklevel`, `$cookno`, `$cookid` |
| **로깅** | `\Logger::channel('userLog')->debug(...)` | `error_log()`, `gd_file_put_contents()`, `debug()` |
| **에러 처리** | `AlertBackException`, `AlertRedirectException`, `LayerException` | `alert(...)` JS 직접, `header('Location:...')` |
| **자동패치 보호** | Bundle/, Asset/Admin/, admin/gd_share/ — 명시적 | 없음 (보통 운영 서버 직접 편집) |
| **인코딩** | UTF-8 일관 | EUC-KR 혼재 (한글 주석/리터럴) |
| **진입점/라우팅** | Front Controller (Controller 클래스) | URL = 파일 경로 (프론트 컨트롤러 없음), 모바일은 User-Agent 분기 |
| **빌드/패키징** | composer (`vendor/` 또는 `Vendor/`) | 없음 — FTP 직접 업로드 |
| **암호화 라이브러리** | 없음 (오픈) | Zend Optimizer 암호화 파일 존재 (`shop/lib/library.php` 등) |

## 3. 빠른 진단 명령

프로젝트 루트에서 다음을 실행해 결과를 종합:

```bash
# Godo5 시그널
test -d Bundle && echo "[+] Godo5: Bundle/ 디렉토리 존재"
test -d module && echo "[+] Godo5: module/ 디렉토리 존재"
test -f composer.json && echo "[+] Godo5: composer.json 존재"
grep -rln "namespace Bundle" --include="*.php" . | head -1 && echo "[+] Godo5: Bundle namespace 사용"

# Godo4 / 레거시 시그널
test -d shop && echo "[*] Godo4: shop/ 디렉토리 존재"
test -d "shop/lib/GODO" && echo "[*] Godo4: GODO 프레임워크"
grep -rln "mysql_query" --include="*.php" . | head -1 && echo "[*] 레거시: mysql_* 함수 사용"
grep -rln "Core::loader" --include="*.php" . | head -1 && echo "[*] Godo4: Core::loader 패턴"
test -d PEAR && echo "[*] 레거시: PEAR 라이브러리"
```

## 4. 분기 의사결정

| 진단 결과 | 권장 행동 |
|---|---|
| **Godo5** 신호 다수 | `godomall5-developer` 스킬을 그대로 적용. SKILL.md / project-template.md 사용. |
| **Godo4** 신호 다수 | 이 스킬의 패턴을 강요하지 말 것. 레거시 GODO 코어(`shop/lib/GODO/`) 패턴을 따름. 사용자에게 "Godo4 레거시로 식별됨 — Godo5와 다른 가이드 필요" 안내 후, 레거시 코드 컨벤션(전역 `$db`, `mysql_*`, `Template_/_compiles/`)을 존중. |
| **혼합** (Godo5 + 일부 레거시) | 흔히 어드민 일부가 PHP 레거시인 경우. 어느 부분을 만지는지에 따라 패턴 선택. 사용자에 명시적으로 "이번 작업은 Godo5/레거시 어느 영역인가?" 질문. |
| **인하우스 PHP** (godo 아님) | 본 스킬 대부분 미적용. PHP 버전·전역 패턴·권한 시스템·DB 라이브러리만 그대로 받아들이고, 보안 이슈(`mysql_*`, SQL Injection, 평문 자격증명)는 **수정 시도 전에 사용자 승인** 받고 진행. |

## 5. 식별 결과 보고 형식

진단 후 사용자에게 다음을 짧게 보고:

```markdown
## 프로젝트 진단

- **분류**: Godo5 / Godo4 / 혼합 / 인하우스 PHP
- **결정 근거** (3개 이내):
  - 예) `Bundle/` 존재, `composer.json` 존재, `namespace Bundle\Component\...` 사용
- **적용할 가이드**:
  - 예) `godomall5-developer` 스킬 그대로 사용
- **주의할 점**:
  - 예) `data/module/`이 활성화되어 있어 미리보기 모드 동작 중
```

## 6. 흔한 함정

- "**고도몰**"이라는 이름 때문에 Godo5로 오인 → 실제로는 PHP5 레거시인 경우가 많음. 항상 디렉토리 구조부터 확인.
- `gd_*` 함수가 있다고 해서 무조건 Godo4가 아님 — Godo5도 `gd_isset`, `gd_policy`, `gd_htmlspecialchars` 같은 헬퍼를 가짐. **함수 prefix가 아닌 디렉토리 구조와 namespace 사용 여부**로 판단.
- Godo4 프로젝트에 `Bundle/` 폴더가 보일 때 — composer 패키지 폴더(예: Symfony Bundle)일 수 있음. `Bundle/Component/`, `Bundle/Controller/Front/` 같은 godo5 특유의 하위 구조까지 확인할 것.
