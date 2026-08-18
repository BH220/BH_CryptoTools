# BH_CryptoTools

암복호화 테스트 도구. 브라우저에서만 도는 정적 페이지다.

기존 PHP 버전을 Web Crypto API 로 포팅했다. 서버가 없어도 되고,
키와 입력값은 페이지 밖으로 나가지 않는다. 네트워크 요청이 없다.

## 기능

- **AES-256-GCM** (양방향) — `v1:base64(IV 12 + 태그 16 + 암호문)` 형식, AAD 는 `v1`
- **HMAC-SHA256** (단방향) — 64자 hex, `소문자+trim` 정규화 옵션
- 키 입력이 base64 로 32바이트면 그대로 쓰고, 아니면 SHA-256 으로 파생
- 32바이트 랜덤 키 생성

PHP(`openssl_encrypt` / `hash_hmac`) 버전과 결과가 완전히 호환된다.
PHP 로 암호화한 값을 이 페이지에서 복호화할 수 있고, 반대 방향도 된다.
HMAC 값도 동일하게 나온다.

## 배포 (GitHub Pages)

`index.html` 하나뿐이라 빌드 과정이 없다.

1. 이 저장소를 GitHub 에 push
2. Settings → Pages → Source 를 **Deploy from a branch** 로
3. Branch 를 `main` / `/ (root)` 로 지정

몇 분 뒤 `https://<계정>.github.io/BH_CryptoTools/` 에서 열린다.

## 로컬 실행

`index.html` 을 그냥 열어도 되지만, `file://` 로 열면 브라우저에 따라
Web Crypto API 가 막힐 수 있다. 그럴 땐 로컬 서버로 띄운다.

```bash
python -m http.server 8172
```

## 주의

Web Crypto API 는 보안 컨텍스트(https 또는 localhost)에서만 동작한다.
GitHub Pages 는 https 라 문제없다.
