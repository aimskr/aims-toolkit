# aims-toolkit

AIMS 팀을 위한 Claude Code MCP 도구 모음입니다.

## 포함된 MCP 서버

| 서버 | 설명 |
|------|------|
| sequential-thinking | 복잡한 문제를 단계별로 사고하는 도구 |
| context7 | 컨텍스트 기반 코드 이해 도구 |
| serena | 코드 탐색 및 분석 도구 |
| playwright | 브라우저 자동화 및 테스트 도구 |

## 사전 요구사항

- **Node.js** 18 이상
- **Python** 3.10 이상
- **uv** (Python 패키지 매니저)

### uv 설치

```bash
# macOS/Linux
curl -LsSf https://astral.sh/uv/install.sh | sh
```

## 환경 변수 설정

Context7 사용을 위해 API 키를 환경 변수로 설정해야 합니다.

### macOS/Linux

```bash
# ~/.bashrc 또는 ~/.zshrc에 추가
export CONTEXT7_API_KEY="your-api-key"
```

## 설치 방법

### 1단계: 마켓플레이스 등록

```bash
claude plugin marketplace add aimskr/aims-toolkit
```

### 2단계: 플러그인 설치

```bash
claude plugin install aims-toolkit
```

프로젝트 범위로 설치하려면:

```bash
claude plugin install aims-toolkit --scope project
```

## 포함된 Skills

| 스킬 | 설명 |
|------|------|
| serena-exploration | Serena MCP를 활용한 코드 탐색 |
| sequential-thinking | Sequential Thinking MCP를 활용한 체계적 사고 |
| brainstorming | 아이디어 브레인스토밍 |
| writing-plans | 구현 계획 작성 |
| prd-strategist | PRD 및 제품 전략 수립 |
| debug-specialist | 디버깅 전문가 |
| code-reviewer | 코드 리뷰 |
| ui-ux-design | UI/UX 디자인 |
| architect | 아키텍처 설계 |
| code-conventions | 코드 컨벤션 가이드 |
| developer | 개발 가이드 |
| doc-coauthoring | 문서 공동 작성 |
| testing-strategy | 테스트 전략 |
| security-review | 보안 리뷰 |

## 로컬 테스트

```bash
claude --plugin-dir ./aims-toolkit
```
