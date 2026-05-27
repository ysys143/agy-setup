# Antigravity CLI (agy) 사내 설정 가이드

> Google I/O 2026에서 발표된 AI 터미널 코딩 에이전트.  
> 우리 회사는 **Google for Startups GCP 크레딧**으로 무료 사용 가능합니다.
>
> **공식 문서:** https://antigravity.google/docs/cli-using

---

## 1단계: 설치

```bash
curl -fsSL https://antigravity.google/cli/install.sh | bash
```

설치 후 PATH 적용:

```bash
source ~/.zshrc   # zsh 사용자
# 또는
source ~/.bashrc  # bash 사용자
```

설치 확인:

```bash
agy --help
```

---

## 2단계: GCP 인증

회사 Google 계정(`@wedatalab.com`)으로 인증합니다.

```bash
# 1. gcloud CLI 로그인
gcloud auth login

# 2. Application Default Credentials 설정
gcloud auth application-default login

# 3. 크레딧 프로젝트로 quota 설정
gcloud auth application-default set-quota-project gen-lang-client-0212455734
```

브라우저가 자동으로 열리면 `@wedatalab.com` 계정으로 로그인합니다.

> **왜 이 단계가 반드시 필요한가?**
>
> 3단계의 환경변수는 "어떤 GCP 프로젝트에 연결할지"만 지정합니다.
> 실제 인증은 여기서 저장되는 **ADC(Application Default Credentials) 파일**이 담당합니다.
>
> ```
> gcloud auth application-default login
>         |
>         v
> ~/.config/gcloud/application_default_credentials.json  <- 인증 정보 저장
>         |
>         v
> agy 실행 시 SDK가 자동으로 이 파일을 읽어 액세스 토큰 발급
>         |
>         v
> 환경변수로 지정한 프로젝트에 API 호출
> ```
>
> 이 단계를 건너뛰면 환경변수가 있어도 인증 오류가 발생합니다.

> **접근 권한 안내**
>
> `set-quota-project gen-lang-client-0212455734` 단계는 해당 GCP 프로젝트에
> IAM으로 등록된 계정만 통과됩니다.
>
> ```
> agy 실행
>     |
>     v
> GCP: 이 계정이 IAM에 등록되어 있나?
>     |
>     +--> 등록됨 -> 통과 (Vertex AI 사용 가능)
>     |
>     +--> 미등록 -> 403 PERMISSION_DENIED 오류
> ```
>
> 접근 권한이 필요하면 관리자(ysys143@wedatalab.com)에게 요청하세요.

---

## 3단계: 환경변수 설정

`~/.zshrc` (또는 `~/.bashrc`) 파일에 다음을 추가합니다:

```bash
# Antigravity CLI - GCP Vertex AI 설정
export GOOGLE_CLOUD_PROJECT=gen-lang-client-0212455734
export GOOGLE_GENAI_USE_VERTEXAI=true
export GOOGLE_CLOUD_REGION=us-central1
```

적용:

```bash
source ~/.zshrc
```

---

## 4단계: 실행

```bash
agy
```

첫 실행 시 브라우저 인증이 뜨면 `@wedatalab.com` 계정으로 로그인합니다.

---

## 주요 사용법

```bash
# 대화형 세션
agy

# 단일 질문 (비대화형)
agy -p "이 코드 리뷰해줘"

# 이전 대화 이어서
agy -c

# 권한 확인 없이 자동 실행 (주의해서 사용)
agy -y
```

---

## 비용 안내

- 사용 비용은 **GCP 크레딧**에서 차감됩니다 (Vertex AI 토큰 과금)
- 크레딧 잔액 확인: [GCP Console - Billing](https://console.cloud.google.com/billing)
- 프로젝트: `gen-lang-client-0212455734`

---

## 문제 해결

### "project not found" 오류
```bash
gcloud config set project gen-lang-client-0212455734
```

### 인증 오류
```bash
gcloud auth application-default login
gcloud auth application-default set-quota-project gen-lang-client-0212455734
```

### SSH 환경 (원격 서버)에서 사용 시
터미널에 URL과 코드가 출력됩니다. 로컬 브라우저에서 해당 URL을 열어 인증을 완료하세요.

---

## 설정 파일 위치

agy의 설정 파일은 다음 경로에 저장됩니다:

```
~/.gemini/antigravity-cli/
```

설정 커스터마이징이나 초기화가 필요할 때 이 경로를 참조하세요.

---

## 참고

- [공식 문서](https://antigravity.google/docs/cli-using)
- [GCP Console](https://console.cloud.google.com)
- 문의: 사내 Slack `#dev-tools`
