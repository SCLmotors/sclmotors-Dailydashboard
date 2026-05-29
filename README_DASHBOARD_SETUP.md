# 삼천리모터스 대시보드 GitHub Pages 설정 가이드

## ⚠️ 먼저 읽으세요

`sclmotors.ai@gmail.com` 은 **이메일 주소**입니다.  
GitHub 계정의 **사용자명(username)** 이 별도로 필요합니다.

---

## STEP 1 — GitHub 계정 준비

1. [https://github.com](https://github.com) 접속
2. 오른쪽 위 프로필 클릭 → **Your profile**
3. URL에 표시된 `github.com/[여기가 username]` 확인
4. 계정이 없으면 **Sign up** → `sclmotors.ai@gmail.com`으로 가입  
   → username 예시: `sclmotors` 또는 `samchullyai`

---

## STEP 2 — GitHub 저장소 생성

1. GitHub 로그인 후 [https://github.com/new](https://github.com/new) 접속
2. Repository name: `sclmotors-dashboard`
3. **Public** 선택 (GitHub Pages 무료 플랜은 Public만 가능)
4. ☑ **Add a README file** 체크
5. **Create repository** 클릭

---

## STEP 3 — GitHub Pages 활성화

1. 생성된 저장소 → **Settings** 탭
2. 왼쪽 메뉴 **Pages** 클릭
3. Source: **Deploy from a branch**
4. Branch: **main** / Folder: **/ (root)**
5. **Save** 클릭
6. 잠시 후 `https://[username].github.io/sclmotors-dashboard/` 접속 확인

---

## STEP 4 — 로컬 clone & 파일 배치

PowerShell에서 실행:

```powershell
# 대시보드 폴더 위치로 이동
cd "C:\Users\User\Desktop\210401 윤영준\1. 기획팀 업무\1. 일일업무보고\일일업무보고 자동생성"

# 기존 dashboard 폴더 삭제 (백업 후)
Rename-Item dashboard dashboard_backup

# GitHub 저장소 clone
git clone https://github.com/[username]/sclmotors-dashboard.git dashboard

# 백업에서 index.html 복사
Copy-Item dashboard_backup\index.html dashboard\
Copy-Item dashboard_backup\README_DASHBOARD_SETUP.md dashboard\

# data 폴더 생성
New-Item -ItemType Directory -Force dashboard\data

# git 사용자 설정 (최초 1회)
git config --global user.email "sclmotors.ai@gmail.com"
git config --global user.name "삼천리모터스"

# 첫 커밋 & push
cd dashboard
git add .
git commit -m "init: dashboard setup"
git push
```

> `[username]`을 실제 GitHub 사용자명으로 교체하세요.

---

## STEP 5 — 자격 증명 저장 (push 시 매번 비밀번호 묻지 않게)

```powershell
# Windows 자격 증명 관리자 사용
git config --global credential.helper wincred

# 또는 PAT(Personal Access Token) 사용 권장
# GitHub → Settings → Developer settings → Personal access tokens → Tokens (classic)
# → Generate new token → repo 권한 체크 → 생성된 토큰을 비밀번호로 입력
```

---

## STEP 6 — 동작 확인

samchully_daily.py 실행 후 출력에서 확인:

```
대시보드 업데이트 중...
  ✓ JSON: data/20260522.json
  ✓ GitHub 대시보드 push 완료 (data/20260522.json)
```

대시보드 URL: `https://[username].github.io/sclmotors-dashboard/`

---

## 문제 해결

| 증상 | 해결 |
|------|------|
| `git push 실패` | PAT 토큰 재생성 후 자격 증명 갱신 |
| `대시보드 Git 저장소 없음` | STEP 4의 clone 재실행 |
| 차트가 안 보임 | 인터넷 연결 확인 (Chart.js CDN 사용) |
| 데이터 없음 표시 | samchully_daily.py 실행 후 10~30초 대기 |
