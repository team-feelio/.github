# 🔒 보안에 꼭 신경써 주세요!

⚠️ 해커톤이라도 **보안 습관**은 중요합니다.
API 키 하나 유출되면 과금 폭탄, 서비스 마비까지 이어질 수 있어요!!

---

## 1️⃣ 레포지토리 설정

| 항목 | 규칙 |
|------|------|
| **레포 공개 범위** | Private (해커톤 기간 동안) |
| **브랜치 보호** | `main` 브랜치 → PR 필수, 최소 1명 리뷰 승인 |
| **`.env` 파일** | 반드시 `.gitignore`에 등록 — 절대 커밋 금지 |

---

## 2️⃣ 시크릿 & 토큰 관리

### ✅ API 키·비밀번호는 `.env` 파일에만 저장

```
# .env (예시)
OPENAI_API_KEY=sk-xxxx
MONGO_URI=mongodb+srv://user:pass@cluster.mongodb.net/feelio
JWT_SECRET=super-secret-string
```

### ✅ 팀원 간 시크릿 공유 방법

| 방법 | 설명 |
|------|------|
| **1Password / Bitwarden** | 무료 팀 볼트로 관리 |
| **GitHub Secrets** | Actions 파이프라인 전용 |
| **password.link** | 일회성 링크 (자동 소멸) |

> ⛔ 디스코드 DM, 카카오톡, 노션에 시크릿 붙여넣지 마세요!

### ❌ 하드코딩 금지

```javascript
// ❌ 절대 이렇게 하시면 안 돼요!
const apiKey = "sk-proj-abc123...";

// ✅ 환경변수로 불러오세요
const apiKey = process.env.OPENAI_API_KEY;
```

---

## 3️⃣ 실수로 시크릿을 커밋했다면?

> 🚨 **즉시 조치해주세요!**

1. **해당 키 즉시 폐기(Revoke)** — 대시보드에서 재발급
2. **BFG Repo-Cleaner로 히스토리 삭제**

```bash
# BFG Repo-Cleaner 사용법
bfg --replace-text passwords.txt repo.git
cd repo.git
git reflog expire --expire=now --all
git gc --prune=now --aggressive
git push --force
```

3. **팀원 전체에게 알림** → 모두 `git pull --rebase`
4. **변경된 키로 `.env` 업데이트**

---

## 4️⃣ 모듈 의존성

| 주의 사항 | 설명 |
|-----------|------|
| **기본 시크릿 확인** | 보일러플레이트·스타터에 포함된 기본 키 반드시 교체 |
| **타이포스쿼팅** | `expresss`(s 3개), `lodassh` 같은 가짜 패키지 주의 |
| **Lock 파일 커밋** | `package-lock.json` / `yarn.lock` 반드시 커밋 |
| **출처 확인** | npm 공식 레지스트리 + 주간 다운로드 수 확인 |

---

## 5️⃣ 배포 & 네트워크

| 항목 | 규칙 |
|------|------|
| **HTTPS** | 배포 시 HTTPS 필수 적용 |
| **IP 화이트리스트** | DB 접근은 팀원 IP만 허용 |
| **CORS** | 허용 도메인을 명시적으로 지정 |
| **공개 엔드포인트** | 인증 없는 Public API 금지 |

---

## ✅ 보안 체크리스트

프로젝트 시작 전에 아래 항목을 확인하세요:

- [ ] `.gitignore`에 `.env`, `node_modules/`, `.DS_Store` 등록
- [ ] `.env.example` 파일 생성 (키 값은 비워두기)
- [ ] 각자 개인 API 키 발급 완료
- [ ] 레포지토리 Private 설정 확인
- [ ] 배포 환경에 환경변수 안전하게 등록

---

> 💡 **기억하세요**: 코드는 고칠 수 있지만 유출된 키는 되돌릴 수 없습니다.
의심되면 바로 팀 디스코드에 공유해 주세요!
