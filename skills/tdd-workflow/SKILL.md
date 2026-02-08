---
name: tdd-workflow
description: "TDD, 테스트 주도 개발, 테스트 먼저, RED-GREEN-REFACTOR - Test-Driven Development workflow. Write tests first, then implement code to make tests pass. Enforces 80%+ coverage."
tools: Read, Write, Edit, Bash, Grep, Glob
model: opus
---

# TDD Workflow

테스트 주도 개발 워크플로우입니다. 테스트를 먼저 작성하고, 테스트를 통과하는 코드를 구현합니다.

## 핵심 원칙

> **ALWAYS write tests first, then implement code to make tests pass.**

### Red-Green-Refactor 사이클
```
🔴 RED: 실패하는 테스트 작성
    ↓
🟢 GREEN: 테스트를 통과하는 최소한의 코드 작성
    ↓
🔵 REFACTOR: 코드 개선 (테스트는 계속 통과)
    ↓
   반복
```

## 워크플로우

### Phase 1: 요구사항 분석
```
사용자 스토리 형식으로 정의:

As a [role],
I want to [action],
So that [benefit]

예:
As a user,
I want to search for markets semantically,
So that I can find relevant markets even without exact keywords.
```

### Phase 2: 인터페이스 정의 (RED 준비)
```
테스트 작성 전 인터페이스/타입 정의:

1. 입력 타입 정의
2. 출력 타입 정의
3. 에러 케이스 정의
4. 경계 조건 정의
```

### Phase 3: 테스트 작성 (RED)
```python
# test_semantic_search.py

import pytest
from services.search import SemanticSearchService

class TestSemanticSearch:
    """Semantic search functionality tests"""
    
    def test_returns_relevant_results_for_valid_query(self):
        """Should return relevant markets for semantic query"""
        # Arrange
        service = SemanticSearchService()
        query = "crypto trading platform"
        
        # Act
        results = service.search(query)
        
        # Assert
        assert len(results) > 0
        assert all(result.relevance_score > 0.5 for result in results)
    
    def test_returns_empty_for_no_matches(self):
        """Should return empty list when no matches found"""
        service = SemanticSearchService()
        results = service.search("xyz123nonexistent")
        assert results == []
    
    def test_raises_error_for_empty_query(self):
        """Should raise ValueError for empty query"""
        service = SemanticSearchService()
        with pytest.raises(ValueError, match="Query cannot be empty"):
            service.search("")
    
    def test_limits_results_to_max_count(self):
        """Should respect max_results parameter"""
        service = SemanticSearchService()
        results = service.search("market", max_results=5)
        assert len(results) <= 5
```

### Phase 4: 최소 구현 (GREEN)
```python
# services/search.py

from dataclasses import dataclass
from typing import List

@dataclass
class SearchResult:
    id: str
    name: str
    relevance_score: float

class SemanticSearchService:
    def search(self, query: str, max_results: int = 10) -> List[SearchResult]:
        if not query or not query.strip():
            raise ValueError("Query cannot be empty")
        
        # 최소 구현 - 테스트 통과만을 위한 코드
        results = self._perform_search(query)
        return results[:max_results]
    
    def _perform_search(self, query: str) -> List[SearchResult]:
        # TODO: 실제 검색 로직 구현
        return []
```

### Phase 5: 리팩토링 (REFACTOR)
```
테스트가 통과하는 상태에서:
1. 중복 코드 제거
2. 명확한 이름 사용
3. 복잡도 감소
4. 성능 최적화

⚠️ 리팩토링 후 반드시 테스트 재실행
```

### Phase 6: 커버리지 확인
```bash
# Python
pytest --cov=src --cov-report=term-missing --cov-fail-under=80

# JavaScript/TypeScript
npm test -- --coverage --coverageThreshold='{"global":{"lines":80}}'

# Java
mvn test jacoco:report
```

## 테스트 유형별 가이드

### 단위 테스트 (Unit Test)
```
- 단일 함수/메서드 테스트
- 외부 의존성 모킹
- 빠른 실행 (< 100ms)
- 격리된 환경
```

### 통합 테스트 (Integration Test)
```
- 여러 컴포넌트 상호작용 테스트
- 실제 DB 또는 테스트 DB 사용
- API 엔드포인트 테스트
- 느린 실행 허용 (< 5s)
```

### E2E 테스트 (End-to-End Test)
```
- 전체 사용자 플로우 테스트
- 실제 환경과 유사한 설정
- 가장 느림 (< 30s)
- Critical Path만 테스트
```

## 테스트 커버리지 목표

| 테스트 유형 | 비율 | 대상 |
|------------|------|------|
| Unit | 70-80% | 비즈니스 로직, 유틸리티 |
| Integration | 15-25% | API, DB 연동 |
| E2E | 5-10% | Critical User Journey |

### 커버리지 기준
```
최소 요구사항:
- Line Coverage: 80%+
- Branch Coverage: 75%+
- Function Coverage: 90%+

⚠️ 높은 커버리지 ≠ 좋은 테스트
의미 있는 assertion이 중요
```

## 테스트 작성 패턴

### Given-When-Then (BDD)
```python
def test_user_login():
    # Given: 유효한 사용자 자격 증명
    user = User(email="test@example.com", password="valid_password")
    
    # When: 로그인 시도
    result = auth_service.login(user.email, user.password)
    
    # Then: 로그인 성공
    assert result.success is True
    assert result.token is not None
```

### Arrange-Act-Assert (AAA)
```python
def test_calculate_total():
    # Arrange
    cart = ShoppingCart()
    cart.add_item(Item(price=100))
    cart.add_item(Item(price=200))
    
    # Act
    total = cart.calculate_total()
    
    # Assert
    assert total == 300
```

## 모킹 가이드

### 외부 서비스 모킹
```python
from unittest.mock import Mock, patch

def test_external_api_call():
    with patch('services.external_api.fetch') as mock_fetch:
        mock_fetch.return_value = {"status": "success"}
        result = my_service.process()
        assert result.status == "success"
        mock_fetch.assert_called_once()
```

### 데이터베이스 모킹
```python
@pytest.fixture
def mock_db():
    return Mock(spec=Database)

def test_user_repository(mock_db):
    mock_db.find_by_id.return_value = User(id=1, name="Test")
    repo = UserRepository(mock_db)
    user = repo.get(1)
    assert user.name == "Test"
```

## 사용 방법

### TDD 시작
```
/tdd [feature description]

또는

TDD로 사용자 인증 기능을 구현해주세요.
```

### 테스트 먼저 요청
```
이 기능의 테스트를 먼저 작성해주세요:
[기능 설명]
```

## 체크리스트

### 테스트 작성 전
- [ ] 요구사항 명확히 이해
- [ ] 입출력 타입 정의
- [ ] 경계 조건 식별
- [ ] 에러 케이스 식별

### 테스트 작성 시
- [ ] 실패하는 테스트 먼저 확인
- [ ] 하나의 테스트는 하나의 동작만
- [ ] 의미 있는 테스트 이름
- [ ] 적절한 assertion

### 구현 후
- [ ] 모든 테스트 통과
- [ ] 80%+ 커버리지
- [ ] 리팩토링 완료
- [ ] 문서화 완료
