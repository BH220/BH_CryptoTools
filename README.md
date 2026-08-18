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
`https://crypto.bhsoft.co.kr` 로 서비스한다.

### 1. DNS (bhsoft.co.kr 네임서버)

`crypto` 서브도메인에 CNAME 레코드를 추가한다.

| 타입 | 이름 | 값 |
|---|---|---|
| CNAME | `crypto` | `bh220.github.io.` |

A 레코드가 아니라 CNAME 이다. 서브도메인이므로 GitHub 의 IP 4개를 쓸 필요가 없다.

### 2. GitHub

1. 저장소를 push
2. Settings → Pages → Source 를 **Deploy from a branch**, `main` / `/ (root)`
3. Custom domain 에 `crypto.bhsoft.co.kr` 입력 후 Save
4. DNS 검증이 끝나면 **Enforce HTTPS** 체크

저장소의 [CNAME](CNAME) 파일이 3번을 대신한다. push 하면 GitHub 가 읽어서
자동으로 커스텀 도메인을 설정한다. 이 파일을 지우면 도메인 설정이 풀린다.

`.nojekyll` 은 Jekyll 빌드 단계를 건너뛰게 한다. 정적 파일이라 필요 없는 과정이다.

### 참고

인증서 발급에 보통 몇 분, 길면 한 시간쯤 걸린다. 그 사이에는
`Enforce HTTPS` 가 회색으로 비활성 상태다. 기다리면 된다.

커스텀 도메인을 쓰면 사이트가 `/BH_CryptoTools/` 가 아니라 루트(`/`)에서
서비스된다. 이 페이지는 외부 파일을 참조하지 않으므로 경로 수정이 필요 없다.

## 로컬 실행

`index.html` 을 그냥 열어도 되지만, `file://` 로 열면 브라우저에 따라
Web Crypto API 가 막힐 수 있다. 그럴 땐 로컬 서버로 띄운다.

```bash
python -m http.server 8172
```

## 주의

Web Crypto API 는 보안 컨텍스트(https 또는 localhost)에서만 동작한다.
GitHub Pages 는 https 라 문제없다.
