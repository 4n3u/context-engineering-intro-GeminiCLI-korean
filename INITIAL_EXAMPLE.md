<!--
  INITIAL_EXAMPLE.md Gemini CLI에 최적화됨:
  - 명확하고 사용자 친화적인 기능 설명 및 컨텍스트 제공
  - examples/ 폴더의 예시 참조 및 사용법 안내
  - 요약이 포함된 문서 링크
  - 설치 팁을 포함한 기타 상세 고려사항
  - 실행 전 반드시 삭제 요망
-->

## 기능:
다음 기능을 갖춘 Pydantic 기반 AI 에이전트 시스템을 정의합니다.

- **주요 에이전트 (ResearchAgent):** Brave API에 쿼리하여 사용자 지정 주제에 대한 연구 데이터를 수집합니다.
- **보조 에이전트 (EmailDraftAgent):** ResearchAgent의 출력을 사용하여 Gmail 호환 초안 이메일을 생성합니다.
- **CLI 통합:** 각 에이전트를 호출하는 명령을 제공합니다.
  - `gemini context execute-agent ResearchAgent --query "<topic>"`
  - `gemini context execute-agent EmailDraftAgent --template "<template_name>"`
- **API 통합:** python-dotenv를 통해 로드된 환경 변수를 사용하여 초안 전송을 위한 Gmail API 및 연구 호출을 위한 Brave API로 인증합니다.

## 예제:
`examples/` 폴더에서 다음 샘플을 참고하십시오 (줄 단위로 복사하지 **마십시오**).

- `examples/cli.py` — `argparse`를 사용하여 인수를 구문 분석하고 에이전트 실행을 위한 하위 명령을 구성하는 방법을 보여줍니다.
- `examples/agent/` — 다음을 보여주는 Pydantic 에이전트 정의를 포함합니다.
  - 공급자 및 LLM 추상화
  - 에이전트에 대한 도구 등록
  - 에이전트 간 통신을 위한 종속성 주입

## 문서:
- **Pydantic AI 문서:** https://ai.pydantic.dev/ — Pydantic 기반 AI 에이전트 정의, 도구 패턴 및 모델 구성에 대한 가이드입니다.

## 기타 고려 사항:
- **환경 설정:**
  - `GMAIL_API_KEY`, `BRAVE_API_KEY` 및 모든 OAuth 설정을 나열하는 `.env.example`을 포함합니다.
  - `load_env()`와 함께 `python-dotenv`를 사용하여 자격 증명을 로드합니다.
- **README 지침:**
  - Gmail OAuth 구성 및 Brave API 온보딩 단계를 설명하는 설정 섹션을 추가합니다.
  - 프로젝트 구조 및 각 `gemini context execute-agent` 명령을 실행하는 방법을 문서화합니다.
- **테스트 팁:**
  - pytest의 `monkeypatch`를 사용하여 테스트에서 API 호출을 모의하여 Gmail 및 Brave 응답을 시뮬레이션합니다.
  - CLI가 다운스트림 파서 또는 로깅 시스템에 유효한 JSON을 출력하는지 확인합니다.