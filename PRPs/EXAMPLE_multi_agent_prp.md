name: "다중 에이전트 시스템: 이메일 초안 하위 에이전트가 있는 연구 에이전트"
description: |

## 목적
기본 연구 에이전트가 Brave Search API를 사용하고 이메일 초안 에이전트(Gmail API 사용)를 도구로 사용하는 Pydantic AI 다중 에이전트 시스템을 구축합니다. 이는 외부 API 통합을 통한 에이전트-도구 패턴을 보여줍니다.

## 핵심 원칙
1. **컨텍스트가 핵심**: 필요한 모든 문서, 예제 및 주의 사항을 포함합니다.
2. **유효성 검사 루프**: AI가 실행하고 수정할 수 있는 실행 가능한 테스트/린트를 제공합니다.
3. **정보 밀도**: 코드베이스의 키워드 및 패턴을 사용합니다.
4. **점진적 성공**: 간단하게 시작하고, 유효성을 검사한 다음, 개선합니다.

---

## 목표
사용자가 CLI를 통해 주제를 연구할 수 있고, 연구 에이전트가 이메일 초안 작성 작업을 이메일 초안 에이전트에 위임할 수 있는 프로덕션 준비 다중 에이전트 시스템을 만듭니다. 시스템은 여러 LLM 공급자를 지원하고 API 인증을 안전하게 처리해야 합니다.

## 이유
- **비즈니스 가치**: 연구 및 이메일 초안 작성 워크플로우를 자동화합니다.
- **통합**: 고급 Pydantic AI 다중 에이전트 패턴을 보여줍니다.
- **해결된 문제**: 연구 기반 이메일 통신을 위한 수동 작업을 줄입니다.

## 내용
CLI 기반 애플리케이션:
- 사용자가 연구 쿼리를 입력합니다.
- 연구 에이전트가 Brave API를 사용하여 검색합니다.
- 연구 에이전트가 이메일 초안 에이전트를 호출하여 Gmail 초안을 생성할 수 있습니다.
- 결과가 사용자에게 실시간으로 스트리밍됩니다.

### 성공 기준
  - 연구 에이전트가 Brave API를 통해 성공적으로 검색합니다.
  - 이메일 에이전트가 적절한 인증으로 Gmail 초안을 생성합니다.
  - 연구 에이전트가 이메일 에이전트를 도구로 호출할 수 있습니다.
  - CLI는 도구 가시성과 함께 스트리밍 응답을 제공합니다.
  - 모든 테스트가 통과하고 코드가 품질 표준을 충족합니다.


## 필요한 모든 컨텍스트

### 문서 및 참조
```yaml
# 반드시 읽어야 함 - 컨텍스트 창에 포함
- url: https://ai.pydantic.dev/agents/
  why: 핵심 에이전트 생성 패턴

- url: https://ai.pydantic.dev/multi-agent-applications/
  why: 다중 에이전트 시스템 패턴, 특히 에이전트-도구

- url: https://developers.google.com/gmail/api/guides/sending
  why: Gmail API 인증 및 초안 생성

- url: https://api-dashboard.search.brave.com/app/documentation
  why: Brave Search API REST 엔드포인트

- file: examples/agent/agent.py
  why: 에이전트 생성, 도구 등록, 종속성 패턴

- file: examples/agent/providers.py
  why: 다중 공급자 LLM 구성 패턴

- file: examples/cli.py
  why: 스트리밍 응답 및 도구 가시성을 갖춘 CLI 구조

- url: https://github.com/googleworkspace/python-samples/blob/main/gmail/snippet/send%20mail/create_draft.py
  why: 공식 Gmail 초안 생성 예제
```

### 현재 코드베이스 트리
```bash
.
├── examples/
│   ├── agent/
│   │   ├── agent.py
│   │   ├── providers.py
│   │   └── ...
│   └── cli.py
├── PRPs/
│   └── templates/
│       └── prp_base.md
├── INITIAL.md
├── CLAUDE.md
└── requirements.txt
```

### 추가될 파일이 있는 원하는 코드베이스 트리
```bash
.
├── agents/
│   ├── __init__.py               # 패키지 초기화
│   ├── research_agent.py         # Brave Search가 있는 기본 에이전트
│   ├── email_agent.py           # Gmail 기능이 있는 하위 에이전트
│   ├── providers.py             # LLM 공급자 구성
│   └── models.py                # 데이터 유효성 검사를 위한 Pydantic 모델
├── tools/
│   ├── __init__.py              # 패키지 초기화
│   ├── brave_search.py          # Brave Search API 통합
│   └── gmail_tool.py            # Gmail API 통합
├── config/
│   ├── __init__.py              # 패키지 초기화
│   └── settings.py              # 환경 및 구성 관리
├── tests/
│   ├── __init__.py              # 패키지 초기화
│   ├── test_research_agent.py   # 연구 에이전트 테스트
│   ├── test_email_agent.py      # 이메일 에이전트 테스트
│   ├── test_brave_search.py     # Brave 검색 도구 테스트
│   ├── test_gmail_tool.py       # Gmail 도구 테스트
│   └── test_cli.py              # CLI 테스트
├── cli.py                       # CLI 인터페이스
├── .env.example                 # 환경 변수 템플릿
├── requirements.txt             # 업데이트된 종속성
├── README.md                    # 포괄적인 문서
└── credentials/.gitkeep         # Gmail 자격 증명 디렉토리
```

### 알려진 문제점 및 라이브러리 특이 사항
```python
# 중요: Pydantic AI는 비동기 컨텍스트에서 동기 함수를 사용하지 않고 전체적으로 비동기를 요구합니다.
# 중요: Gmail API는 첫 실행 시 OAuth2 흐름을 요구합니다. credentials.json이 필요합니다.
# 중요: Brave API에는 속도 제한이 있습니다. 무료 등급에서 월 2000회 요청.
# 중요: 에이전트-도구 패턴은 토큰 추적을 위해 ctx.usage를 전달해야 합니다.
# 중요: Gmail 초안은 적절한 MIME 형식으로 base64 인코딩이 필요합니다.
# 중요: 항상 더 깔끔한 코드를 위해 절대 임포트를 사용하십시오.
# 중요: 민감한 자격 증명은 .env에 저장하고 커밋하지 마십시오.
```

## 구현 청사진

### 데이터 모델 및 구조

```python
# models.py - 핵심 데이터 구조
from pydantic import BaseModel, Field
from typing import List, Optional
from datetime import datetime

class ResearchQuery(BaseModel):
    query: str = Field(..., description="조사할 연구 주제")
    max_results: int = Field(10, ge=1, le=50)
    include_summary: bool = Field(True)

class BraveSearchResult(BaseModel):
    title: str
    url: str
    description: str
    score: float = Field(0.0, ge=0.0, le=1.0)

class EmailDraft(BaseModel):
    to: List[str] = Field(..., min_items=1)
    subject: str = Field(..., min_length=1)
    body: str = Field(..., min_length=1)
    cc: Optional[List[str]] = None
    bcc: Optional[List[str]] = None

class ResearchEmailRequest(BaseModel):
    research_query: str
    email_context: str = Field(..., description="이메일 생성을 위한 컨텍스트")
    recipient_email: str
```

### 완료할 작업 목록

```yaml
Task 1: 구성 및 환경 설정
CREATE config/settings.py:
  - PATTERN: os.getenv를 사용하는 예제처럼 pydantic-settings 사용
  - 기본값으로 환경 변수 로드
  - 필요한 API 키가 있는지 유효성 검사

CREATE .env.example:
  - 필요한 모든 환경 변수를 설명과 함께 포함
  - examples/README.md의 패턴 따르기

Task 2: Brave Search 도구 구현
CREATE tools/brave_search.py:
  - PATTERN: examples/agent/tools.py와 같은 비동기 함수
  - httpx를 사용하는 간단한 REST 클라이언트 (requirements에 이미 있음)
  - 속도 제한 및 오류를 정상적으로 처리
  - 구조화된 BraveSearchResult 모델 반환

Task 3: Gmail 도구 구현
CREATE tools/gmail_tool.py:
  - PATTERN: Gmail 빠른 시작의 OAuth2 흐름 따르기
  - credentials/ 디렉토리에 token.json 저장
  - 적절한 MIME 인코딩으로 초안 생성
  - 인증 새로 고침 자동 처리

Task 4: 이메일 초안 에이전트 생성
CREATE agents/email_agent.py:
  - PATTERN: examples/agent/agent.py 구조 따르기
  - deps_type 패턴과 함께 Agent 사용
  - gmail_tool을 @agent.tool로 등록
  - EmailDraft 모델 반환

Task 5: 연구 에이전트 생성
CREATE agents/research_agent.py:
  - PATTERN: Pydantic AI 문서의 다중 에이전트 패턴
  - brave_search를 도구로 등록
  - email_agent.run()을 도구로 등록
  - RunContext를 사용하여 종속성 주입

Task 6: CLI 인터페이스 구현
CREATE cli.py:
  - PATTERN: examples/cli.py 스트리밍 패턴 따르기
  - 도구 가시성을 갖춘 색상 코드 출력
  - asyncio.run()으로 비동기를 적절하게 처리
  - 대화 컨텍스트를 위한 세션 관리

Task 7: 포괄적인 테스트 추가
CREATE tests/:
  - PATTERN: 예제 테스트 구조 미러링
  - 외부 API 호출 모의
  - 해피 경로, 엣지 케이스, 오류 테스트
  - 80% 이상 커버리지 보장

Task 8: 문서 생성
CREATE README.md:
  - PATTERN: examples/README.md 구조 따르기
  - 설정, 설치, 사용법 포함
  - API 키 구성 단계
  - 아키텍처 다이어그램
```

### 작업별 의사 코드

```python
# Task 2: Brave Search 도구
async def search_brave(query: str, api_key: str, count: int = 10) -> List[BraveSearchResult]:
    # PATTERN: examples가 aiohttp를 사용하는 것처럼 httpx 사용
    async with httpx.AsyncClient() as client:
        headers = {"X-Subscription-Token": api_key}
        params = {"q": query, "count": count}
        
        # GOTCHA: API 키가 유효하지 않으면 Brave API는 401을 반환합니다.
        response = await client.get(
            "https://api.search.brave.com/res/v1/web/search",
            headers=headers,
            params=params,
            timeout=30.0  # 중요: 중단을 피하기 위해 시간 초과 설정
        )
        
        # PATTERN: 구조화된 오류 처리
        if response.status_code != 200:
            raise BraveAPIError(f"API가 {response.status_code}를 반환했습니다.")
        
        # Pydantic으로 구문 분석 및 유효성 검사
        data = response.json()
        return [BraveSearchResult(**result) for result in data.get("web", {}).get("results", [])]

# Task 5: 이메일 에이전트를 도구로 사용하는 연구 에이전트
@research_agent.tool
async def create_email_draft(
    ctx: RunContext[AgentDependencies],
    recipient: str,
    subject: str,
    context: str
) -> str:
    """연구 컨텍스트를 기반으로 이메일 초안을 생성합니다."""
    # 중요: 토큰 추적을 위해 사용량을 전달합니다.
    result = await email_agent.run(
        f"Create an email to {recipient} about: {context}",
        deps=EmailAgentDeps(subject=subject),
        usage=ctx.usage  # 다중 에이전트 문서의 PATTERN
    )
    
    return f"초안이 ID: {result.data}로 생성되었습니다."
```

### 통합 지점
```yaml
ENVIRONMENT:
  - add to: .env
  - vars: |
      # LLM 구성
      LLM_PROVIDER=openai
      LLM_API_KEY=sk-...
      LLM_MODEL=gpt-4
      
      # Brave Search
      BRAVE_API_KEY=BSA...
      
      # Gmail (credentials.json 경로)
      GMAIL_CREDENTIALS_PATH=./credentials/credentials.json
      
CONFIG:
  - Gmail OAuth: 첫 실행 시 브라우저에서 인증 요청
  - 토큰 저장: ./credentials/token.json (자동 생성)
  
DEPENDENCIES:
  - requirements.txt 업데이트:
    - google-api-python-client
    - google-auth-httplib2
    - google-auth-oauthlib
```

## 유효성 검사 루프

### 레벨 1: 구문 및 스타일
```bash
# 먼저 실행 - 진행하기 전에 모든 오류 수정
ruff check . --fix              # 스타일 문제 자동 수정
mypy .                          # 타입 검사

# 예상: 오류 없음. 오류가 있으면 읽고 수정하십시오.
```

### 레벨 2: 단위 테스트
```python
# test_research_agent.py
async def test_research_with_brave():
    """연구 에이전트가 올바르게 검색하는지 테스트"""
    agent = create_research_agent()
    result = await agent.run("AI 안전 연구")
    assert result.data
    assert len(result.data) > 0

async def test_research_creates_email():
    """연구 에이전트가 이메일 에이전트를 호출할 수 있는지 테스트"""
    agent = create_research_agent()
    result = await agent.run(
        "AI 안전을 연구하고 john@example.com으로 이메일 초안 작성"
    )
    assert "draft_id" in result.data

# test_email_agent.py  
def test_gmail_authentication(monkeypatch):
    """Gmail OAuth 흐름 처리 테스트"""
    monkeypatch.setenv("GMAIL_CREDENTIALS_PATH", "test_creds.json")
    tool = GmailTool()
    assert tool.service is not None

async def test_create_draft():
    """적절한 인코딩으로 초안 생성 테스트"""
    agent = create_email_agent()
    result = await agent.run(
        "AI 연구에 대해 test@example.com으로 이메일 생성"
    )
    assert result.data.get("draft_id")
```

```bash
# 통과할 때까지 반복적으로 테스트 실행:
pytest tests/ -v --cov=agents --cov=tools --cov-report=term-missing

# 실패 시: 특정 테스트 디버그, 코드 수정, 다시 실행
```

### 레벨 3: 통합 테스트
```bash
# CLI 상호 작용 테스트
python cli.py

# 예상 상호 작용:
# 당신: 최신 AI 안전 개발 연구
# 🤖 어시스턴트: [연구 결과 스트리밍]
# 🛠 사용된 도구:
#   1. brave_search (query='AI 안전 개발', limit=10)
#
# 당신: john@example.com으로 이메일 초안 작성  
# 🤖 어시스턴트: [초안 생성]
# 🛠 사용된 도구:
#   1. create_email_draft (recipient='john@example.com', ...)

# 생성된 초안에 대해 Gmail 초안 폴더 확인
```

## 최종 유효성 검사 체크리스트
- [ ] 모든 테스트 통과: `pytest tests/ -v`
- [ ] 린팅 오류 없음: `ruff check .`
- [ ] 타입 오류 없음: `mypy .`
- [ ] Gmail OAuth 흐름 작동 (브라우저 열림, 토큰 저장됨)
- [ ] Brave Search 결과 반환
- [ ] 연구 에이전트가 이메일 에이전트를 성공적으로 호출
- [ ] CLI가 도구 가시성과 함께 응답 스트리밍
- [ ] 오류 사례가 정상적으로 처리됨
- [ ] README에 명확한 설정 지침 포함
- [ ] .env.example에 필요한 모든 변수 포함

---

## 피해야 할 안티 패턴
- ❌ API 키를 하드코딩하지 마십시오. 환경 변수를 사용하십시오.
- ❌ 비동기 에이전트 컨텍스트에서 동기 함수를 사용하지 마십시오.
- ❌ Gmail에 대한 OAuth 흐름 설정을 건너뛰지 마십시오.
- ❌ API에 대한 속도 제한을 무시하지 마십시오.
- ❌ 다중 에이전트 호출에서 ctx.usage를 전달하는 것을 잊지 마십시오.
- ❌ credentials.json 또는 token.json 파일을 커밋하지 마십시오.

## 신뢰도 점수: 9/10

높은 신뢰도 이유:
- 코드베이스에서 따를 명확한 예제
- 잘 문서화된 외부 API
- 다중 에이전트 시스템에 대한 확립된 패턴
- 포괄적인 유효성 검사 게이트

📣 참고: 첫 실행 시 Gmail OAuth는 브라우저 창을 통해 사용자에게 인증을 요청합니다. `credentials.json`이 `./credentials/`에 올바르게 배치되었는지 확인하십시오.

# 이 PRP 생성 (템플릿인 경우)
gemini context generate-prp INITIAL.md

# 이 PRP 실행
gemini context execute-prp PRPs/multi_agent_research_email_system.md