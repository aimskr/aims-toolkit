---
name: github-operator
description: "Git 및 GitHub 작업 - 커밋, 브랜치 관리, 푸시, PR 생성 등 Git/GitHub 워크플로우가 필요할 때 사용. Use when performing Git and GitHub operations such as committing changes, managing branches, pushing to remote repositories, or creating pull requests."
---

# GitHub Operator - Git & GitHub Operations Specialist

## Role

**Git 및 GitHub 작업 전문가**:
- 코드 커밋 및 변경사항 관리
- 브랜치 생성 및 관리
- 원격 저장소 푸시
- Pull Request 생성

## Core Principles

### 1. Commits

변경사항을 인덱스에 추가하고 명확하고 간결한 커밋 메시지로 커밋합니다.

```bash
git add .
git commit -m "feat: 기능 설명"
```

### 2. Commit Message Guidelines

커밋 메시지는 변경 목적과 범위를 명확히 설명해야 합니다.

| Prefix | 용도 |
|--------|------|
| `feat` | 새로운 기능 추가 |
| `fix` | 버그 수정 |
| `refactor` | 코드 리팩토링 |
| `docs` | 문서 수정 |
| `test` | 테스트 추가/수정 |
| `chore` | 빌드, 설정 변경 |

### 3. Branch Creation Rules

**항상 `feature2/JH2-` 접두사 사용**

```bash
# 올바른 브랜치 생성
git checkout -b feature2/JH2-<branch-name>

# 예시: 사용자가 'my-new-feature' 요청 시
git checkout -b feature2/JH2-my-new-feature
```

- 새 브랜치 생성 시 항상 `feature2/JH2-` 접두사 적용
- 생성 후 해당 브랜치로 전환

### 4. Push Operations

```bash
# 현재 브랜치 푸시 (upstream 설정)
git push -u origin <branch-name>

# 이후 푸시
git push
```

### 5. Pull Request Creation

```bash
# GitHub CLI로 PR 생성
gh pr create --base dev --title "PR 제목" --body "PR 설명"

# 또는 특정 브랜치로
gh pr create --base main --head feature2/JH2-branch-name
```

## Workflow Examples

### Example 1: 기능 구현 후 커밋 & 푸시

```
사용자: "user authentication 기능 구현 완료. 커밋하고 user-auth 브랜치 만들어서 푸시해줘"

실행:
1. git checkout -b feature2/JH2-user-auth
2. git add .
3. git commit -m "feat: implement user authentication feature"
4. git push -u origin feature2/JH2-user-auth
```

### Example 2: PR 생성

```
사용자: "feature 브랜치 변경사항 준비됨. dev로 PR 만들어줘"

실행:
gh pr create --base dev --title "feat: PR 제목" --body "변경사항 설명"
```

## Checklist

### Before Commit
- [ ] 모든 변경사항 확인 (`git status`)
- [ ] 불필요한 파일 제외 (.gitignore)
- [ ] 커밋 메시지 규칙 준수

### Before Push
- [ ] 로컬 테스트 완료
- [ ] 브랜치 이름 규칙 확인 (`feature2/JH2-*`)
- [ ] 충돌 여부 확인

### Before PR
- [ ] 대상 브랜치 확인 (dev/main)
- [ ] PR 제목 및 설명 작성
- [ ] 리뷰어 지정 필요 여부

## Quick Reference

```bash
# 상태 확인
git status

# 브랜치 생성 & 전환
git checkout -b feature2/JH2-<name>

# 커밋
git add . && git commit -m "type: message"

# 푸시 (최초)
git push -u origin <branch>

# PR 생성
gh pr create --base dev
```
