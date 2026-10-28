# **「🤖 터미널 기반 Codex AI 에이전트 활용 가이드」**

> OpenAI 공식 문서를 바탕으로 **자주 사용하는 명령어 중심**으로 정리한 한국어 가이드 입니다.<br>
> 명령어는 CLI 버전에 따라 달라질 수 있습니다.<br>
> 실제 설치 버전에서 `codex --help`, `codex <subcommand> --help`, Codex 실행 후 `/` 메뉴를 우선 확인하세요.

## 목차

- [1. 빠른 시작](#1-빠른-시작)
- [2. 가장 많이 쓰는 터미널 명령어](#2-가장-많이-쓰는-터미널-명령어)
- [3. 대화 중 사용하는 슬래시 명령어](#3-대화-중-사용하는-슬래시-명령어)
- [4. 주요 실행 옵션](#4-주요-실행-옵션)
- [5. MCP 서버 연결](#5-mcp-서버-연결)
- [6. 에이전트 및 서브에이전트 관리](#6-에이전트-및-서브에이전트-관리)
- [7. 개발 상황별 사용 예시](#7-개발-상황별-사용-예시)
- [8. 안전한 사용 및 문제 해결](#8-안전한-사용-및-문제-해결)
- [9. 참고 자료](#9-참고-자료)
<br>

## 1. 빠른 시작

### 설치 (한 가지 방식만 선택)

```bash
# npm
npm install -g @openai/codex

# 또는 Homebrew (macOS)
brew install --cask codex
```

### 업데이트

설치했던 방식에 맞게 업데이트합니다.

```bash
# npm으로 설치한 경우
npm install -g @openai/codex@latest

# Homebrew로 설치한 경우
brew update
brew upgrade --cask codex

# 자체 업데이트를 지원하는 설치본의 경우
codex update
```

### 처음 실행

```bash
codex --version       # 설치 버전 확인
codex --help          # 최상위 명령어 확인
codex login           # ChatGPT 계정 등으로 로그인
codex login status    # 인증 상태 확인
codex                 # 대화형 CLI 시작
```

프로젝트에서 사용하려면 **프로젝트 루트에서 `codex`를 실행**합니다. 일반적인 ChatGPT처럼 질문만 하는 것도 가능하지만, Codex는 파일을 읽고 코드를 수정하며 명령을 실행할 수 있는 개발 에이전트이므로 권한을 확인하세요.

<br>

## 2. 가장 많이 쓰는 터미널 명령어

> **입력 위치:** 일반 셸(zsh, bash 등)에서 입력. Codex 대화 화면 안의 `/` 명령어와 다릅니다.

### 기본·인증

| 명령어 | 용도 |
| --- | --- |
| `codex` | 대화형 Codex 실행 |
| `codex "프로젝트 구조를 설명해줘"` | 첫 질문과 함께 실행 |
| `codex --version` | 버전 확인 |
| `codex --help` | 전체 CLI 도움말 |
| `codex login` | 로그인 |
| `codex login status` | 현재 인증 방식·상태 확인 |
| `codex logout` | 로컬에 저장된 Codex 인증 정보 삭제 |
| `codex doctor` | 설치·인증·설정·터미널 등 환경 진단 |
| `codex agents` | 공유 로컬/원격 서버의 Agent Command Center 실행 |
| `codex update` | 자체 업데이트를 지원하는 설치본에서 업데이트 확인·적용 |

### 이전 작업 이어서 하기

| 명령어 | 용도 |
| --- | --- |
| `codex resume` | 저장된 대화 선택 후 재개 |
| `codex resume --last` | **현재 작업 디렉터리**의 최근 대화 재개 |
| `codex resume --all` | 다른 디렉터리의 대화까지 선택 범위 확대 |
| `codex fork --last` | 최근 대화를 복제해 별도 분기로 시작 |

`resume`은 **기존 대화를 이어서 진행**하고, `fork`는 **기존 내용을 바탕으로 별도 대화로 분기**합니다.

### 코드 리뷰·일회성 실행

| 명령어 | 용도 |
| --- | --- |
| `codex review --uncommitted` | 커밋하지 않은 변경 사항 리뷰 |
| `codex review --base main` | `main` 브랜치와 비교해 리뷰 |
| `codex review --commit <SHA>` | 특정 커밋의 변경 사항 리뷰 |
| `codex exec "코드 구조를 요약해줘"` | 비대화형으로 작업하고 종료 (스크립트·CI에 적합) |
| `codex exec --output-last-message result.txt "프로젝트를 설명해줘"` | 최종 답변을 파일에 저장 |
| `codex exec resume --last "남은 문제를 정리해줘"` | 최근 비대화형 세션 이어서 실행 |

`codex`는 **대화를 계속하는 방식**, `codex exec`는 **한 번의 작업을 실행하고 종료하는 방식**입니다. `codex review` 역시 비대화형 리뷰 명령입니다.

<br>

## 3. 대화 중 사용하는 슬래시 명령어

> **입력 위치:** `codex` 실행 후 Codex 대화 입력창. `/`만 입력하면 현재 버전에서 사용 가능한 명령 목록이 나타납니다.

### 가장 자주 쓰는 명령어

| 명령어 | 용도 |
| --- | --- |
| `/status` | 현재 모델·권한·세션 설정·컨텍스트/토큰 상태 확인 |
| `/agents` | Agent Command Center에서 대화 상태를 살피고 다른 대화로 전환 (공유 서버 필요) |
| `/subagents` | 서브에이전트 스레드 조회 및 선택 |
| `/model` | 모델과 지원되는 추론 수준 변경 |
| `/fast` | 지원되는 모델의 Fast 서비스 티어 전환 |
| `/skills` | 사용 가능한 스킬 탐색 |
| `/plugins` | 플러그인 탐색·관리 |
| `/permissions` | 작업 권한 설정 (예: Read Only / Auto) |
| `/plan` | 구현에 앞서 계획 모드로 전환 |
| `/review` | 현재 작업 트리의 코드 리뷰 |
| `/diff` | 변경된 파일과 Git diff 확인 |
| `/compact` | 긴 대화의 컨텍스트를 요약·압축 |
| `/new` | 새 대화 시작 |
| `/resume` | 저장된 대화 다시 열기 |
| `/fork` | 현재 대화에서 새로운 대화 분기 |
| `/side` | 메인 대화를 유지하며 별도 질문 (`/btw`도 지원) |
| `/init` | 프로젝트 지침 파일 `AGENTS.md` 초안 생성 |
| `/mcp` | 이 세션에서 사용 가능한 MCP 도구 확인 |
| `/quit` 또는 `/exit` | Codex를 종료하고 일반 터미널로 돌아가기 |

### 이외 명령어

| 명령어 | 용도 |
| --- | --- |
| `/usage` | 계정 사용량 정보 확인 |
| `/rename` | 현재 대화 이름 변경 |
| `/copy` | 마지막 Codex 응답 복사 |
| `/export` | 대화를 Markdown으로 내보내기 (`/export session.md` 가능) |
| `/recap` | 현재 대화 요약 (컨텍스트 압축은 하지 않음) |
| `/pwd` 또는 `/cwd` | 현재 작업 디렉터리 확인 |
| `/cd <경로>` | 세션 작업 디렉터리 변경 (유휴 상태에서 가능) |
| `/worktree` | 별도 Git worktree에서 작업 |
| `/goal` | 장기 작업 목표 지정·관리 (지원 버전에서) |
| `/logout` | Codex에서 로그아웃 |

**구분 주의:** `/status`는 현재 **세션** 상태, `/usage`는 **계정 사용량** 확인에 사용합니다. `/compact`는 대화를 압축하지만 `/recap`은 요약만 생성합니다.

<br>

## 4. 주요 실행 옵션

일반 셸에서 `codex` 뒤에 지정합니다.

| 옵션 | 설명 |
| --- | --- |
| `-C, --cd <경로>` | Codex가 작업할 디렉터리 지정 |
| `-m, --model <모델명>` | 사용할 모델 지정 (지원 모델은 현재 환경에서 확인) |
| `-s, --sandbox read-only` | 모델이 실행하는 명령의 쓰기 권한 제한 |
| `-s, --sandbox workspace-write` | 워크스페이스 내 쓰기 허용 |
| `-a, --ask-for-approval on-request` | 필요한 작업에 승인 요청 |
| `-i, --image <파일경로>` | 초기 프롬프트에 이미지 첨부 |
| `--search` | 라이브 웹 검색 사용 |
| `--worktree` | 새로운 관리형 Git worktree에서 시작 |
| `-c, --config key=value` | 이번 실행에 한해 설정 덮어쓰기 |

### 예시

```bash
# 파일을 수정하지 않고 코드 분석에 집중할 때
codex --sandbox read-only

# 지정된 프로젝트를 대상으로 시작
codex -C ~/projects/my-backend

# 코드 수정이 가능한 작업, 승인 요청 유지
codex --sandbox workspace-write --ask-for-approval on-request

# 이미지와 함께 질문
codex --image ./error.png "이 오류 화면을 분석해줘"
```

> `read-only`는 **파일 읽기를 차단하는 모드가 아닙니다.** 파일 접근 자체를 원하지 않으면 별도로 명시하고, 민감한 프로젝트에서는 실행 위치와 접근 권한을 확인하세요. `--dangerously-bypass-approvals-and-sandbox`(별칭 `--yolo`)는 승인과 샌드박스를 우회하므로 일반 개발 환경에서는 사용하지 않는 것이 안전합니다.

<br>

## 5. MCP 서버 연결

MCP(Model Context Protocol)를 이용하면 Codex가 외부 도구를 사용할 수 있습니다.

| 명령어 | 설명 |
| --- | --- |
| `codex mcp list` | 등록된 MCP 서버 목록 |
| `codex mcp get <이름>` | 서버 설정 확인 |
| `codex mcp add <이름> --url <URL>` | HTTP 기반 MCP 서버 등록 |
| `codex mcp add <이름> -- <실행명령>` | stdio 기반 MCP 서버 등록 |
| `codex mcp login <이름>` | OAuth가 필요한 서버 인증 |
| `codex mcp logout <이름>` | MCP 서버 OAuth 인증 해제 |
| `codex mcp remove <이름>` | 등록한 MCP 서버 제거 |

### OpenAI 공식 개발 문서 MCP 연결 예시

```bash
# OpenAI 문서 검색용 MCP 서버 등록
codex mcp add openaiDeveloperDocs --url https://developers.openai.com/mcp

# 등록 확인
codex mcp list

# 필요 없어지면 제거
codex mcp remove openaiDeveloperDocs
```

Codex 대화창에서 `/mcp`를 입력하면 **현재 세션에 제공되는 도구**를 확인할 수 있습니다.

> **주의:** 오래된 자료에 등장하는 `codex mcp-server` 명령어는 **제거되었습니다.** 외부 MCP 서버를 Codex에 연결하는 작업에는 `codex mcp`를 사용하세요.

<br>

## 6. 에이전트 및 서브에이전트 관리

Codex 공식 문서는 **에이전트 명령 센터(Agent Command Center)** 와 **서브에이전트(Subagents)** 를 구분합니다.

### Agent Command Center: 여러 대화 관리

| 명령어 | 입력 위치 | 용도 |
| --- | --- | --- |
| `codex agents` | 일반 터미널 | 공유 서버의 대화 목록·상태를 보고 에이전트/대화를 전환하는 명령 센터 열기 |
| `codex agents --remote <URL>` | 일반 터미널 | 원격 Codex app-server에 연결해 명령 센터 열기 |
| `/agents` | Codex 대화 화면 | 대화 중 명령 센터를 열어 다른 대화 선택 |

```bash
# 일반 터미널에서 에이전트 명령 센터 열기
codex agents

# 원격 서버 연결 (실제 운영 중인 서버 주소로 변경)
codex agents --remote <URL>
```

> `codex agents`와 `/agents`는 **공유 로컬 또는 원격 서버**를 필요로 합니다. `codex agents`는 `--no-daemon` 옵션과 함께 사용할 수 없습니다.

### Subagents: 작업 분담·스레드 확인

| 명령어/방법 | 용도 |
| --- | --- |
| `/subagents` | Codex 안에서 서브에이전트 스레드를 선택하고 작업 내용 살펴보기 |
| 자연어로 위임 요청 | 여러 에이전트에 독립적인 조사·검증 작업을 나누어 달라고 요청 |

```text
# Codex 대화 입력창
/subagents

# 서브에이전트를 사용하도록 자연어로 요청하는 예시
현재 브랜치와 main의 변경 사항을 세 가지 관점에서 병렬로 검토해줘.
보안, 동시성, 테스트를 각각 별도의 서브에이전트에 맡기고 결과를 합쳐줘.
```

Codex는 `default`, `worker`, `explorer` 내장 에이전트를 제공하며, 필요한 경우 `~/.codex/agents/`(사용자) 또는 `.codex/agents/`(프로젝트)에 TOML 파일로 사용자 정의 에이전트를 추가할 수 있습니다. **`AGENTS.md`는 프로젝트 지침 파일**로, 에이전트 관리 명령이나 사용자 정의 에이전트의 TOML 파일과는 다릅니다.

공식 문서: [CLI 명령어 레퍼런스](https://developers.openai.com/codex/cli/reference) · [Subagents](https://developers.openai.com/codex/multi-agent)

<br>

## 7. 개발 상황별 사용 예시

### A. 코드는 건드리지 않고 질문, 분석만 하기

```bash
cd ~/projects/my-backend
codex --sandbox read-only
```

Codex 입력창:

```text
Spring Boot의 트랜잭션 경계와 잠재적인 동시성 문제를 분석해줘.
아직 코드를 수정하거나 테스트를 실행하지 말고 설명만 해줘.
```

### B. 구현 전에 설계 검토하기

```text
/plan
관리자 설정 변경 API를 PUT/PATCH로 분리하는 설계를 제안해줘.
동시 요청 시 유실 업데이트를 예방하는 방법도 포함해줘.
```

### C. 구현 결과 확인하기

```text
/diff
/review
/status
```

수정 내용을 확인하고 코드 리뷰 결과를 검토한 뒤, 필요한 테스트와 Git 커밋을 직접 확인합니다.

### D. 대화 이어서 진행하거나 종료하기

```text
/compact   # 대화가 길어졌을 때 요약·압축
/exit      # Codex 종료
```

이후 일반 터미널에서:

```bash
codex resume --last
```

### E. 스크립트에서 1회 실행하기

```bash
codex exec --output-last-message review-summary.md \
  "이 저장소의 모듈 구조와 개선 포인트를 Markdown으로 요약해줘"
```

이 방식은 비대화형 작업입니다. 위 예시는 **Codex의 최종 응답**을 `review-summary.md`로 저장하며, 프로젝트 파일 변경 가능 여부는 실행 시 적용된 권한·샌드박스 설정에 따릅니다.

<br>

## 8. 안전한 사용 및 문제 해결

- 작업 전에 `git status`로 변경 사항을 확인하고, 중요한 작업은 커밋 또는 백업으로 복구 지점을 만들어 두세요.
- 코드 변경 후 `/diff` → `/review` → 테스트 → `git status` 순으로 검토하는 습관을 권장합니다.
- 민감한 정보(토큰, API 키, `.env`)를 프롬프트나 Git 저장소에 올리지 마세요.
- 실행이 이상하거나 인증이 꼬이면 `codex doctor`, `codex login status`를 확인하세요.
- 명령이 보이지 않으면 `codex --version`과 `codex --help`를 확인하고 CLI를 업데이트하세요.
- `Token usage so far`는 누적 토큰 사용 통계입니다. 그 문구 자체가 오류나 사용량 한도 초과를 의미하지는 않습니다.
- `/exit`와 `/quit`은 모두 정상 종료 명령어입니다. **종료해도 Git 커밋이 자동으로 만들어지지는 않습니다.**

<br>

## 9. 참고 자료

아래 문서는 **OpenAI 공식 자료**입니다. 이 치트시트는 전체 옵션을 열거하기보다 일상적인 개발 작업에서 쓰기 좋은 항목을 선별했습니다.

1. [Codex CLI 개요 및 설치](https://developers.openai.com/codex/cli)
2. [Codex CLI 명령어·옵션·슬래시 명령어 레퍼런스](https://developers.openai.com/codex/cli/reference)
3. [Codex 공식 GitHub 저장소](https://github.com/openai/codex)
4. [OpenAI 개발 문서 MCP 연결](https://developers.openai.com/resources/docs-mcp)
5. [CLI 사용자 지정 및 단축키](https://developers.openai.com/ko-KR/docs/cli-customization)
6. [에이전트 및 서브에이전트](https://developers.openai.com/codex/multi-agent)
---
