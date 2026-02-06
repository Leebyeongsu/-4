# GitHub Copilot CLI 사용 가이드

> GitHub Copilot의 코딩 에이전트 CLI를 활용하여 개발 생산성을 극대화하는 방법을 안내합니다.

## 📋 목차

1. [소개](#소개)
2. [설치 방법](#설치-방법)
3. [기본 사용법](#기본-사용법)
4. [주요 기능](#주요-기능)
5. [고급 사용법](#고급-사용법)
6. [실전 예제](#실전-예제)
7. [문제 해결](#문제-해결)
8. [모범 사례](#모범-사례)

---

## 소개

GitHub Copilot CLI는 명령줄 인터페이스에서 AI 기반 코딩 지원을 제공하는 도구입니다. 자연어로 작업을 설명하면 Copilot이 적절한 명령어를 제안하거나 코드를 생성해줍니다.

### 주요 특징

- **자연어 명령**: 평범한 언어로 원하는 작업을 설명
- **컨텍스트 인식**: 현재 프로젝트와 환경을 이해
- **스마트 제안**: 최적의 명령어와 코드를 제안
- **대화형 인터페이스**: 질문하고 답변을 받는 방식으로 작업
- **다중 언어 지원**: 한국어를 포함한 여러 언어 지원

---

## 설치 방법

### 1. 사전 요구사항

GitHub Copilot CLI를 사용하기 전에 다음 사항을 확인하세요:

```bash
# Node.js 버전 확인 (v18 이상 권장)
node --version

# npm 버전 확인
npm --version

# GitHub CLI 설치 확인
gh --version
```

### 2. GitHub CLI 설치

GitHub CLI가 없다면 먼저 설치합니다:

**macOS (Homebrew):**
```bash
brew install gh
```

**Windows (Winget):**
```bash
winget install --id GitHub.cli
```

**Linux (apt):**
```bash
curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg | sudo dd of=/usr/share/keyrings/githubcli-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null
sudo apt update
sudo apt install gh
```

### 3. GitHub 인증

GitHub CLI를 통해 인증합니다:

```bash
# GitHub 계정으로 로그인
gh auth login

# 인증 상태 확인
gh auth status
```

### 4. Copilot CLI 확장 설치

```bash
# Copilot CLI 확장 설치
gh extension install github/gh-copilot

# 설치 확인
gh copilot --version
```

### 5. Copilot 구독 확인

GitHub Copilot을 사용하려면 활성화된 구독이 필요합니다:
- GitHub Copilot Individual
- GitHub Copilot Business
- GitHub Copilot Enterprise

구독 상태는 [GitHub Settings](https://github.com/settings/copilot)에서 확인할 수 있습니다.

---

## 기본 사용법

### 명령어 구조

```bash
gh copilot [command] [flags]
```

### 주요 명령어

#### 1. `suggest` - 명령어 제안받기

쉘 명령어나 Git 명령어를 자연어로 요청:

```bash
# 기본 사용법
gh copilot suggest "파일 크기가 큰 순서로 정렬하여 표시"

# 짧은 버전
gh copilot suggest

# 대화형 모드로 진입하여 질문 입력
```

**예시:**
```bash
# 예: 디렉토리에서 .js 파일만 찾기
gh copilot suggest "현재 디렉토리에서 모든 JavaScript 파일 찾기"

# 출력:
# find . -name "*.js"
```

#### 2. `explain` - 명령어 설명받기

복잡한 명령어나 코드의 동작을 설명:

```bash
# 특정 명령어 설명
gh copilot explain "git rebase -i HEAD~3"

# 파일의 코드 설명
gh copilot explain < script.js
```

**예시:**
```bash
gh copilot explain "find . -type f -name '*.log' -mtime +30 -delete"

# 출력:
# 이 명령어는 다음과 같은 작업을 수행합니다:
# - find .: 현재 디렉토리에서 시작
# - type f: 일반 파일만 검색
# - name '*.log': .log 확장자를 가진 파일
# - mtime +30: 30일 이전에 수정된 파일
# - delete: 해당 파일들을 삭제
```

#### 3. 대화형 모드

아무 인자 없이 실행하면 대화형 모드로 진입:

```bash
gh copilot
```

대화형 모드에서는:
- 자연어로 질문하고 답변 받기
- 연속적인 대화를 통해 문제 해결
- 컨텍스트를 유지하며 후속 질문 가능

---

## 주요 기능

### 1. 쉘 명령어 생성

**사용 예시:**

```bash
# 시스템 리소스 모니터링
gh copilot suggest "CPU와 메모리 사용량 실시간 모니터링"
# → top -o %CPU

# 대용량 파일 찾기
gh copilot suggest "100MB 이상의 파일 찾기"
# → find . -type f -size +100M

# 프로세스 관리
gh copilot suggest "포트 8080을 사용하는 프로세스 종료"
# → lsof -ti:8080 | xargs kill
```

### 2. Git 작업 지원

**사용 예시:**

```bash
# 커밋 히스토리 조회
gh copilot suggest "지난 주에 수정된 커밋 목록 보기"
# → git log --since="1 week ago"

# 브랜치 정리
gh copilot suggest "병합된 로컬 브랜치 모두 삭제"
# → git branch --merged | grep -v "\*" | xargs -n 1 git branch -d

# 파일 변경 내역
gh copilot suggest "특정 파일의 변경 이력 보기"
# → git log -p -- <filename>
```

### 3. 파일 및 텍스트 처리

**사용 예시:**

```bash
# JSON 파싱
gh copilot suggest "JSON 파일에서 특정 필드 추출"
# → jq '.fieldname' file.json

# 텍스트 검색 및 교체
gh copilot suggest "모든 .js 파일에서 'var'를 'let'으로 교체"
# → find . -name "*.js" -exec sed -i 's/var/let/g' {} +

# 파일 압축
gh copilot suggest "디렉토리를 tar.gz로 압축"
# → tar -czf archive.tar.gz directory/
```

### 4. 개발 환경 설정

**사용 예시:**

```bash
# Node.js 프로젝트 초기화
gh copilot suggest "새 Node.js 프로젝트 만들기"
# → npm init -y

# 의존성 설치
gh copilot suggest "package.json의 모든 의존성 설치"
# → npm install

# 환경 변수 설정
gh copilot suggest ".env 파일 생성 및 환경 변수 추가"
# → echo "KEY=value" > .env
```

---

## 고급 사용법

### 1. 파이프라인과 함께 사용

Copilot의 제안을 다른 명령어와 연결:

```bash
# Copilot이 제안한 명령어를 직접 실행
gh copilot suggest "현재 디렉토리의 파일 개수" | sh

# 출력을 파일로 저장
gh copilot suggest "시스템 정보 수집" > system-info.sh
chmod +x system-info.sh
```

### 2. 환경 변수 활용

설정을 통해 동작 커스터마이징:

```bash
# 응답 형식 설정
export GH_COPILOT_FORMAT="json"

# 로그 레벨 설정
export GH_COPILOT_LOG_LEVEL="debug"

# 타임아웃 설정 (초)
export GH_COPILOT_TIMEOUT=30
```

### 3. 별칭(Alias) 설정

자주 사용하는 명령어를 짧게 만들기:

```bash
# .bashrc 또는 .zshrc에 추가
alias gcs='gh copilot suggest'
alias gce='gh copilot explain'
alias gc='gh copilot'

# 사용 예시
gcs "현재 디렉토리 파일 목록"
gce "ls -la"
```

### 4. 프로젝트별 컨텍스트 활용

프로젝트 루트에서 실행하면 Copilot이 프로젝트 구조를 이해:

```bash
# package.json이 있는 디렉토리에서
gh copilot suggest "이 프로젝트의 테스트 실행"
# → npm test (package.json의 scripts를 인식)

# requirements.txt가 있는 디렉토리에서
gh copilot suggest "Python 의존성 설치"
# → pip install -r requirements.txt
```

---

## 실전 예제

### 예제 1: 로그 파일 분석

```bash
# 문제: 에러 로그에서 특정 패턴 찾기
gh copilot suggest "error.log 파일에서 'ERROR' 포함된 줄의 개수 세기"
# 제안: grep -c "ERROR" error.log

# 후속 질문
gh copilot suggest "각 에러 타입별로 개수 집계"
# 제안: grep "ERROR" error.log | awk '{print $NF}' | sort | uniq -c
```

### 예제 2: 데이터베이스 백업 자동화

```bash
# 문제: PostgreSQL 데이터베이스 정기 백업
gh copilot suggest "PostgreSQL 데이터베이스를 현재 날짜로 백업"
# 제안: pg_dump -U username dbname > backup_$(date +%Y%m%d).sql

# cron 작업 등록
gh copilot suggest "매일 새벽 2시에 백업 스크립트 실행하는 cron 작업"
# 제안: 0 2 * * * /path/to/backup.sh
```

### 예제 3: Docker 컨테이너 관리

```bash
# 실행 중인 컨테이너 정리
gh copilot suggest "중지된 Docker 컨테이너 모두 삭제"
# 제안: docker container prune -f

# 이미지 최적화
gh copilot suggest "사용하지 않는 Docker 이미지 삭제"
# 제안: docker image prune -a

# 로그 확인
gh copilot suggest "특정 컨테이너의 최근 100줄 로그 보기"
# 제안: docker logs --tail 100 container_name
```

### 예제 4: 성능 모니터링

```bash
# 네트워크 트래픽 모니터링
gh copilot suggest "실시간 네트워크 사용량 모니터링"
# 제안: iftop -i eth0

# 디스크 I/O 모니터링
gh copilot suggest "디스크 읽기/쓰기 성능 측정"
# 제안: iostat -x 1

# 프로세스별 리소스 사용량
gh copilot suggest "메모리를 가장 많이 사용하는 프로세스 10개"
# 제안: ps aux --sort=-%mem | head -11
```

### 예제 5: 코드 리팩토링

```bash
# 중복 코드 찾기
gh copilot suggest "프로젝트에서 중복된 코드 라인 찾기"
# 제안: find . -name "*.js" -exec sha256sum {} + | sort | uniq -w32 -dD

# 코드 복잡도 분석
gh copilot suggest "JavaScript 파일의 복잡도 분석"
# 제안: npx complexity-report --format json src/**/*.js

# 사용하지 않는 import 찾기
gh copilot suggest "사용하지 않는 import 문 찾기"
# 제안: npx eslint . --fix --rule 'no-unused-vars: error'
```

---

## 문제 해결

### 일반적인 문제와 해결 방법

#### 1. 인증 오류

**증상:**
```
error: authentication required
```

**해결:**
```bash
# GitHub 재인증
gh auth logout
gh auth login

# Copilot 권한 확인
gh auth refresh -s copilot
```

#### 2. 확장 프로그램 오류

**증상:**
```
error: extension 'copilot' not found
```

**해결:**
```bash
# 확장 재설치
gh extension remove copilot
gh extension install github/gh-copilot

# 확장 업데이트
gh extension upgrade copilot
```

#### 3. 네트워크 타임아웃

**증상:**
```
error: request timeout
```

**해결:**
```bash
# 프록시 설정 (필요한 경우)
export HTTPS_PROXY=http://proxy.example.com:8080

# 타임아웃 증가
export GH_COPILOT_TIMEOUT=60
```

#### 4. 응답이 느린 경우

**원인과 해결:**
- 복잡한 질문을 간단하게 나누기
- 네트워크 연결 상태 확인
- GitHub API 상태 확인: https://www.githubstatus.com

#### 5. 잘못된 제안

**해결 방법:**
- 더 구체적으로 질문하기
- 컨텍스트 정보 추가하기
- 대화형 모드에서 후속 질문하기

---

## 모범 사례

### 1. 효과적인 질문하기

**좋은 예시:**
```bash
✅ "Node.js 프로젝트에서 ESLint 설정 파일 생성"
✅ "지난 주에 수정된 Python 파일 목록"
✅ "Docker Compose로 MySQL 컨테이너 시작"
```

**피해야 할 예시:**
```bash
❌ "뭔가 해줘"
❌ "파일"
❌ "도와줘"
```

### 2. 컨텍스트 제공

명확한 정보를 제공할수록 더 정확한 답변을 받습니다:

```bash
# 일반적인 질문
gh copilot suggest "테스트 실행"

# 컨텍스트가 있는 질문 (더 좋음)
gh copilot suggest "Jest를 사용하는 React 프로젝트에서 단위 테스트 실행"
```

### 3. 단계적 접근

복잡한 작업은 단계별로 나누기:

```bash
# 1단계: 파일 찾기
gh copilot suggest "100MB 이상 파일 찾기"

# 2단계: 파일 목록 검토 후 삭제
gh copilot suggest "특정 디렉토리의 오래된 로그 파일 삭제"
```

### 4. 결과 검증

Copilot의 제안을 맹목적으로 실행하지 말고 항상 검토:

```bash
# 제안된 명령어 먼저 확인
gh copilot suggest "모든 node_modules 삭제"
# 출력 확인 후

# 안전하게 실행 (dry-run이 가능한 경우)
find . -name "node_modules" -type d -print  # 먼저 확인

# 실제 실행
find . -name "node_modules" -type d -exec rm -rf {} +
```

### 5. 학습 도구로 활용

Copilot을 명령어 학습 도구로 사용:

```bash
# 명령어 설명 듣기
gh copilot explain "awk '{sum+=$1} END {print sum}' file.txt"

# 유사한 작업을 다른 방법으로 하기
gh copilot suggest "awk 대신 Python으로 파일의 숫자 합계 구하기"
```

### 6. 보안 고려사항

- 민감한 정보를 질문에 포함하지 않기
- 제안된 명령어가 시스템에 미치는 영향 확인
- 중요한 데이터 작업 전에는 백업

```bash
# ❌ 피해야 할 예시
gh copilot suggest "password123으로 데이터베이스 연결"

# ✅ 좋은 예시
gh copilot suggest "환경 변수에서 비밀번호를 읽어 데이터베이스 연결"
```

### 7. 팀과 공유

유용한 명령어는 팀과 공유:

```bash
# 스크립트로 저장
gh copilot suggest "배포 전 체크리스트 실행" > deploy-check.sh

# 문서화
gh copilot suggest "프로젝트 셋업 가이드 생성" >> SETUP.md
```

---

## 추가 리소스

### 공식 문서
- [GitHub Copilot Documentation](https://docs.github.com/en/copilot)
- [GitHub CLI Documentation](https://cli.github.com/manual/)
- [Copilot CLI GitHub Repository](https://github.com/github/gh-copilot)

### 커뮤니티
- [GitHub Community Discussions](https://github.com/orgs/community/discussions)
- [Copilot Feedback](https://github.com/github/feedback/discussions/categories/copilot-feedback)

### 업데이트
```bash
# GitHub CLI 업데이트
gh extension upgrade copilot

# 모든 확장 업데이트
gh extension upgrade --all
```

---

## 요약

GitHub Copilot CLI는 강력한 AI 기반 개발 도구입니다:

1. ✅ **쉬운 설치**: `gh extension install github/gh-copilot`
2. ✅ **자연어 명령**: 평범한 언어로 작업 요청
3. ✅ **컨텍스트 인식**: 프로젝트 환경을 이해
4. ✅ **학습 도구**: 명령어 설명 및 교육
5. ✅ **생산성 향상**: 반복 작업 자동화

### 시작하기

```bash
# 1. 설치
gh extension install github/gh-copilot

# 2. 시작
gh copilot

# 3. 첫 질문
# "현재 디렉토리의 파일을 크기순으로 정렬하여 보여줘"
```

**Happy Coding with GitHub Copilot! 🚀**

---

## 라이선스 및 저작권

이 문서는 GitHub Copilot CLI 사용자 가이드로, 교육 목적으로 작성되었습니다.
GitHub Copilot은 GitHub, Inc.의 제품이며 관련 약관이 적용됩니다.

---

**마지막 업데이트**: 2026년 2월 6일
**버전**: 1.0.0
