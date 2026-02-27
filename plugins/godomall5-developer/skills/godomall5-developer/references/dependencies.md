# 고도몰5 주요 의존성

## ORM 및 데이터베이스
- `illuminate/database` (^5.5): Laravel Eloquent ORM

## HTTP 및 네트워크
- `guzzlehttp/guzzle` (6.2.1): HTTP 클라이언트
- `facebook/graph-sdk` (5.5.0): Facebook API
- `google/apiclient` (2.14): Google API

## 파일 처리
- `league/flysystem` (1.0.70): 파일 시스템 추상화 (Local/FTP/SFTP/AWS)
- `phpoffice/phpexcel` (1.8.1): Excel 파일 처리
- `phpoffice/phpspreadsheet` (1.0.0-beta): 스프레드시트

## 유틸리티
- `monolog/monolog`: 로깅 시스템
- `symfony/cache` (3.4.47): 캐싱
- `predis/predis` (v1.1.6): Redis
- `endroid/qrcode` (^1.5): QR 코드
- `picqer/php-barcode-generator` (v0.2.2): 바코드
- `mobiledetect/mobiledetectlib` (2.*): 모바일 감지
- `respect/validation` (1.1.12): 데이터 검증
- `gettext/gettext` (v3.5.5): 다국어

## 백그라운드 작업
- `sinergi/gearman` (v1.1.1): Gearman 클라이언트
- `nmred/kafka-php` (v0.2.0.8): Kafka 메시징

## 스토리지 시스템 (League Flysystem 기반)
```php
interface StorageInterface { }
abstract class AbstractStorage implements StorageInterface { }
class LocalStorage extends AbstractStorage { }
class FtpStorage extends AbstractStorage { }
class SftpStorage extends AbstractStorage { }
class AwsStorage extends AbstractStorage { }
```

## 개발 도구
- `phpunit/phpunit` (^6): 단위 테스트
- `phing/phing` (2.*): 빌드 자동화
- `squizlabs/php_codesniffer` (3.9.2): PSR-2 코드 스타일 검사
