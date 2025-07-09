# Gemini CLI용 PRP 템플릿
name: base_prp_template_v2
version: 2
type: prp
author: Sathariels -- 이 템플릿은 coleam00의 템플릿을 편집한 것이며, 대부분의 공로는 그에게 있습니다.
date_created: 2025-07-07
description: |
  AI 에이전트가 충분한 컨텍스트와 자체 검증 기능을 통해 반복적인 개선을 통해 작동하는 코드를 구현할 수 있도록 최적화된 템플릿입니다.

## 목적
내장된 유효성 검사 루프와 예제를 포함하는 Gemini CLI용 구조화되고 컨텍스트가 풍부한 PRP 형식입니다.

## 핵심 원칙
1. **컨텍스트가 핵심**: 필요한 모든 문서, 예제 및 주의 사항을 포함합니다.
2. **유효성 검사 루프**: AI가 실행하고 수정할 수 있는 실행 가능한 테스트/린트를 제공합니다.
3. **정보 밀도**: 코드베이스의 키워드 및 패턴을 사용합니다.
4. **점진적 성공**: 간단하게 시작하고, 유효성을 검사한 다음, 개선합니다.
5. **전역 규칙**: GEMINI.md의 모든 규칙을 따릅니다.

---

## 목표
[무엇을 구축해야 하는지 - 최종 상태 및 요구 사항에 대해 구체적으로 설명하십시오.]

## 이유
- [비즈니스 가치 및 사용자 영향]
- [기존 기능과의 통합]
- [이것이 해결하는 문제 및 누구를 위한 것인지]

## 내용
[사용자에게 보이는 동작 및 기술 요구 사항]

### 성공 기준
```yaml
success_criteria:
  - description: [구체적인 측정 가능한 결과 1]
  - description: [구체적인 측정 가능한 결과 2]
```

## 필요한 모든 컨텍스트

### 문서 및 참조 (기능 구현에 필요한 모든 컨텍스트 나열)
```yaml
# 반드시 읽어야 함 - 컨텍스트 창에 포함
- url: [공식 API 문서 URL]
  why: [필요한 특정 섹션/메서드]

- file: [path/to/example.py]
  why: [따라야 할 패턴, 피해야 할 문제점]

- doc: [라이브러리 문서 URL] 
  section: [일반적인 문제점에 대한 특정 섹션]
  critical: [일반적인 오류를 방지하는 핵심 통찰력]

- docfile: [PRPs/ai_docs/file.md]
  why: [사용자가 프로젝트에 붙여넣은 문서]
```

### 현재 코드베이스 트리 (프로젝트 루트에서 `tree`를 실행하여 코드베이스 개요 확인)
```bash
# 여기에 트리 출력 붙여넣기
```

### 추가될 파일 및 파일의 책임이 있는 원하는 코드베이스 트리
```bash
# 생성/편집될 파일 표시
```

### 코드베이스의 알려진 문제점 및 라이브러리 특이 사항
```python
# 중요: [라이브러리 이름]은 [특정 설정]을 요구합니다.
# 예시: FastAPI는 엔드포인트에 비동기 함수를 요구합니다.
# 예시: 이 ORM은 1000개 이상의 레코드에 대한 일괄 삽입을 지원하지 않습니다.
# 예시: 우리는 pydantic v2를 사용하며 ...
```

## 구현 청사진

### 데이터 모델 및 구조
```python
# 예시: 
#  - orm 모델
#  - pydantic 모델
#  - pydantic 스키마
#  - 유효성 검사기
```

### PRP를 이행하기 위해 완료할 작업 목록
```yaml
Task 1:
  MODIFY src/existing_module.py:
    - FIND pattern: "class OldImplementation"
    - INJECT after: "def __init__"
    - PRESERVE: method signatures

  CREATE src/new_feature.py:
    - MIRROR: src/similar_feature.py
    - MODIFY: class name and logic
    - KEEP: error handling pattern

# ... 추가 작업 ...
```

### 작업별 의사 코드
```python
# Task 1 의사 코드
async def new_feature(param: str) -> Result:
    # PATTERN: 입력 유효성 검사 (src/validators.py 참조)
    validated = validate_input(param)

    # GOTCHA: 연결 풀링 필요
    async with get_connection() as conn:
        @retry(attempts=3, backoff=exponential)
        async def _inner():
            await rate_limiter.acquire()
            return await external_api.call(validated)

        result = await _inner()
    return format_response(result)  # src/utils/responses.py 참조
```

### 통합 지점
```yaml
DATABASE:
  - migration: "users 테이블에 'feature_enabled' 열 추가"
  - index: "CREATE INDEX idx_feature_lookup ON users(feature_id)"

CONFIG:
  - add to: config/settings.py
  - pattern: "FEATURE_TIMEOUT = int(os.getenv('FEATURE_TIMEOUT', '30'))"

ROUTES:
  - add to: src/api/routes.py
  - pattern: "router.include_router(feature_router, prefix='/feature')"
```

## 유효성 검사 루프

### 레벨 1: 구문 및 스타일
```bash
ruff check src/new_feature.py --fix
mypy src/new_feature.py
```

### 레벨 2: 단위 테스트
```python
def test_happy_path():
    result = new_feature("valid")
    assert result.status == "success"

def test_validation_error():
    with pytest.raises(ValidationError):
        new_feature("")

def test_external_api_timeout():
    with mock.patch('external_api.call', side_effect=TimeoutError):
        result = new_feature("valid")
        assert result.status == "error"
        assert "timeout" in result.message
```
```bash
uv run pytest test_new_feature.py -v
```

### 레벨 3: 통합 테스트
```bash
uv run python -m src.main --dev

curl -X POST http://localhost:8000/feature \
  -H "Content-Type: application/json" \
  -d '{"param": "test_value"}'
```

## 최종 유효성 검사 체크리스트
```yaml
- [ ] 모든 테스트 통과: uv run pytest tests/ -v
- [ ] 린팅 오류 없음: uv run ruff check src/
- [ ] 타입 오류 없음: uv run mypy src/
- [ ] 수동 테스트 성공: [curl/명령 삽입]
- [ ] 오류 사례가 정상적으로 처리됨
- [ ] 로그는 유익하지만 장황하지 않음
- [ ] 필요한 경우 문서 업데이트
```

---

## 피해야 할 안티 패턴
- ❌ 기존 패턴으로 충분할 때 새 패턴 생성
- ❌ 유효성 검사 건너뛰기
- ❌ 테스트 실패 무시
- ❌ 비동기 흐름에서 동기 함수 사용
- ❌ 구성 값 하드코딩
- ❌ 모든 예외를 일반적으로 포착

---

```bash
# 이 PRP를 생성하려면:
gemini context generate-prp INITIAL.md

# 이 PRP를 실행하려면:
gemini context execute-prp PRPs/your-feature.md

```