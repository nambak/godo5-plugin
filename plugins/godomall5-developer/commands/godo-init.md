---
description: 새 고도몰5 프로젝트 CLAUDE.md 초기화
allowed-tools: Read, Write, Glob, Grep, Bash(ls:*, find:*, php:*)
argument-hint: [project-name]
---

새 고도몰5 프로젝트의 CLAUDE.md를 생성한다.

1. `${CLAUDE_PLUGIN_ROOT}/skills/godomall5-developer/references/project-template.md` 파일을 읽어 템플릿을 로드한다.
2. 사용자가 제공한 프로젝트명 `$1`을 템플릿의 `[프로젝트명]` 자리에 채운다.
3. 현재 디렉토리의 PHP 버전을 확인한다: `php -v` 또는 `composer.json`의 `require.php` 값.
4. Bundle/, module/, Asset/ 디렉토리 존재 여부를 확인하여 프로젝트 구조를 파악한다.
5. 확인된 정보를 반영하여 CLAUDE.md를 생성하고 프로젝트 루트에 저장한다.
6. 생성된 CLAUDE.md 내용을 요약하여 사용자에게 보여준다.
