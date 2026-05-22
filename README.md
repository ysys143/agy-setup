# Antigravity CLI (agy) 설정 가이드

Google이 2026년 5월 19일 Gemini CLI를 대체하여 출시한 터미널 AI 코딩 에이전트.
바이너리명: `agy` / 설치 경로: `~/.local/bin/agy`

---

## 인증 방식 비교

| 방식 | 대상 | 환경변수 / 명령 |
|------|------|----------------|
| API Key | 개인 / CI 스크립트 | `ANTIGRAVITY_API_KEY` |
| Browser OAuth | 로컬 데스크탑 | `agy` 실행 후 자동 브라우저 오픈 |
| SSH / Headless | 원격 서버 | 터미널에 URL + 일회성 코드 출력 |
| GCP Enterprise | 기업 GCP 프로젝트 | `gcloud auth` + Project ID |

---

## 1. API Key 방식 (개인 / 스크립트)

### API Key 발급
- [Google AI Studio](https://aistudio.google.com) - 개인 계정 (Pro/Ultra 필요)
- GCP Console - 기업 프로젝트

### 설정

```bash
# 일회성 사용
export ANTIGRAVITY_API_KEY=your_api_key_here
agy

# 영구 설정 (~/.zshrc 또는 ~/.bashrc에 추가)
echo 'export ANTIGRAVITY_API_KEY=your_api_key_here' >> ~/.zshrc
source ~/.zshrc
```

---

## 2. GCP Enterprise 방식

### 사전 요건

- Google Cloud Project (결제 활성화)
- gcloud CLI 설치

### 인증 순서

```bash
# 1. gcloud CLI 로그인 (Cloud OAuth)
gcloud auth login

# 2. Application Default Credentials 설정
gcloud auth application-default login

# 3. 프로젝트 설정
gcloud config set project YOUR_PROJECT_ID

# 4. 환경변수로 프로젝트/리전 지정 후 실행
export GOOGLE_CLOUD_PROJECT=your-gcp-project-id
export GOOGLE_CLOUD_REGION=asia-northeast3   # 서울 리전
agy
```

### GCP에서 활성화 필요한 API

```bash
gcloud services enable aiplatform.googleapis.com
gcloud services enable cloudresourcemanager.googleapis.com
```

---

## 3. 주요 CLI 플래그

```bash
agy                                    # 대화형 세션 시작
agy -p "질문"                          # 단일 프롬프트 (비대화형 출력)
agy --print "질문"                     # -p 와 동일
agy -i "초기 프롬프트"                 # 프롬프트로 시작 후 대화 계속
agy -c                                 # 가장 최근 대화 이어서
agy --continue                         # -c 와 동일
agy --conversation <ID>                # 특정 대화 ID로 재개
agy --dangerously-skip-permissions     # 도구 권한 요청 자동 승인 (-y 단축키)
agy --sandbox                          # 샌드박스 모드 (네트워크/디스크 제한)
```

---

## 4. 서브커맨드

```bash
agy update          # CLI 업데이트
agy changelog       # 릴리즈 노트 확인
agy install         # 쉘 PATH 및 alias 설정
agy plugin list     # 설치된 플러그인 목록
agy plugin install  # 플러그인 설치
```

---

## 5. 접근 권한 등급

| 등급 | 인증 | 인프라 | 비고 |
|------|------|--------|------|
| 무료 | Google 계정 | Google 공용 | 60 req/min, 1,000 req/day |
| Pro / Ultra | Google 계정 | Google 공용 | 요청 한도 상향 |
| Enterprise | GCP IAM | 기업 VPC 내부 | 데이터 외부 유출 없음 |
| GCA Standard/Enterprise | Gemini Code Assist 라이선스 | 기존 유지 | 2026-06-18 이후도 유지 |

---

## 6. 주요 일정

- **2026-05-19**: Antigravity 2.0 및 agy CLI 출시 (Google I/O 2026)
- **2026-06-18**: Gemini CLI 무료/소비자 티어 종료. Enterprise 라이선스는 영향 없음

---

## 참고 문서

- [공식 문서](https://antigravity.google/docs/cli-using)
- [Antigravity CLI 전환 공지 (Google Developers Blog)](https://developers.googleblog.com/an-important-update-transitioning-gemini-cli-to-antigravity-cli/)
- [GCP 배포 Codelab](https://codelabs.developers.google.com/build-and-deploy-gcp-with-antigravity)
- [Gemini Enterprise Agent Platform](https://cloud.google.com/products/gemini-enterprise-agent-platform)
