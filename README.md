<!--
  README.md Gemini CLI에 최적화됨:
  - .claude를 .gemini로 이름 변경
  - CLI 명령어를 `gemini context` 접두사로 업데이트
  - 파일 참조를 CLAUDE.md에서 GEMINI.md로 변경
  - 예시와 용어를 Claude Code에서 Gemini CLI로 조정
-->

# Gemini CLI를 위한 컨텍스트 엔지니어링 템플릿

Gemini CLI를 사용한 컨텍스트 엔지니어링을 빠르게 시작하기 위한 포괄적인 템플릿으로, AI 코딩 어시스턴트가 엔드투엔드 기능 구현에 필요한 모든 컨텍스트를 갖도록 보장합니다.

> **Gemini CLI를 사용한 컨텍스트 엔지니어링은 프롬프트 엔지니어링보다 10배 더 스마트하고, 바이브 코딩보다 100배 더 신뢰할 수 있습니다.**

## 🚀 빠른 시작

```bash
# 1. 이 템플릿 복제
git clone https://github.com/4n3u/context-engineering-intro-GeminiCLI-korean.git
cd context-engineering-intro-GeminiCLI-korean

# 2. 프로젝트 가이드라인 사용자 정의 (선택 사항)
# GEMINI.md를 편집하여 프로젝트별 규칙 추가

# 3. 설명적인 예제 추가 (강력 권장)
# examples/ 폴더에 관련 코드 샘플 배치

# 4. 초기 기능 요청 정의
# INITIAL.md를 상세 요구 사항으로 편집

# 5. 포괄적인 PRP (제품 요구 사항 프롬프트) 생성
# Gemini CLI를 사용하여 실행:

gemini context generate-prp INITIAL.md

# 6. PRP를 실행하여 기능 구현
# Gemini CLI를 사용하여 실행:

gemini context execute-prp PRPs/your-feature-name.md
```

## 📚 목차

- [컨텍스트 엔지니어링이란?](#컨텍스트-엔지니어링이란)
- [템플릿 구조](#템플릿-구조)
- [단계별 가이드](#단계별-가이드)
- [효과적인 INITIAL.md 파일 작성](#효과적인-initialmd-파일-작성)
- [PRP 워크플로우](#prp-워크플로우)
- [예제 효과적으로 사용하기](#예제-효과적으로-사용하기)
- [모범 사례](#모범-사례)

## 컨텍스트 엔지니어링이란?

컨텍스트 엔지니어링은 단일 프롬프트에서 규칙, 예제 및 유효성 검사의 구조화된 시스템으로 초점을 전환합니다.

### 프롬프트 엔지니어링 vs 컨텍스트 엔지니어링

**프롬프트 엔지니어링:**
- 영리한 문구 작성
- 문구에 제한됨
- 포스트잇을 남기는 것과 같음

**Gemini CLI를 사용한 컨텍스트 엔지니어링:**
- 전체 프레임워크: 문서, 예제, 규칙, 테스트
- 엔드투엔드 기능 파이프라인
- 전체 영화 제작을 지휘하는 것과 같음

### 왜 중요한가

1. **AI 실패 감소**: 대부분의 문제는 모델 오류가 아닌 컨텍스트 누락에서 발생합니다.
2. **일관된 결과**: 코드 스타일 및 패턴을 강제합니다.
3. **복잡한 흐름 지원**: 다단계 기능을 원활하게 처리합니다.
4. **자동화된 유효성 검사**: 내장된 테스트 및 린팅 루프는 품질을 보장합니다.

## 템플릿 구조

```text
context-engineering-intro-GeminiCLI-korean/
├── .gemini/
│   ├── commands/
│   │   ├── generate-prp.md    # PRP 생성 로직
│   │   └── execute-prp.md     # PRP 실행 로직
│   └── settings.local.json    # Gemini CLI 권한
├── PRPs/
│   ├── templates/
│   │   └── prp_base.md        # 기본 PRP 템플릿
│   └── EXAMPLE_multi_agent_prp.md  # 샘플 완료 PRP
├── examples/                   # 코드 예제 (중요!)
├── GEMINI.md                   # Gemini CLI를 위한 전역 프로젝트 규칙
├── INITIAL.md                  # 새 기능 요청을 위한 템플릿
├── INITIAL_EXAMPLE.md          # 예제 기능 요청
├── INITIAL_COMPLEX.md          # 더 복잡한 프로젝트를 위한 포괄적인 문서
└── README.md                   # 이 파일 (Gemini CLI용으로 업데이트됨)
```

<!-- Updated directory names and filenames to use Gemini CLI conventions -->

## 단계별 가이드

### 1. 전역 규칙 정의 (GEMINI.md)

`GEMINI.md`는 AI 어시스턴트를 위한 프로젝트 전체 규칙을 포함합니다.

- **인식**: 작업 및 계획 문서
- **구조**: 모듈 분할, 파일 크기 제한
- **테스트**: 단위 테스트 템플릿, 커버리지 임계값
- **스타일**: 언어 및 서식 규칙
- **문서**: Docstring 및 주석 작성 관행

프로젝트 요구 사항에 맞게 템플릿을 사용자 정의하거나 그대로 사용하십시오.

### 2. INITIAL.md 초안 작성

새 기능을 설명합니다.

```markdown
## FEATURE:
[명확하고 상세한 설명]

## EXAMPLES:
[예제 파일 및 패턴 목록]

## DOCUMENTATION:
[API 문서, 스키마, 가이드 링크]

## OTHER CONSIDERATIONS:
[인증, 속도 제한, 주의 사항]
```
지침은 `INITIAL_EXAMPLE.md`를 참조하십시오.

더 복잡한 기능을 위해 INITIAL_COMPLEX.md를 사용하십시오.
```markdown
## High-Level Objective
[무엇을 구축하고 있습니까?]

## Mid-Level Objectives
[거기에 도달하기 위한 단계는 무엇입니까?]

## Implementation Notes
[기술 노트, 종속성, 파일 구조 등]

## Context
- Beginning Context: [어떤 파일이 존재합니까?]
- Ending Context: [이후에 어떤 파일이 존재할까요?]

## Low-Level Tasks
1. [상세 구현 단계]

## EXAMPLES:
[예제 파일 및 패턴 목록]

## DOCUMENTATION:
[API 문서, 스키마, 가이드 링크]

## OTHER CONSIDERATIONS:
[인증, 속도 제한, 주의 사항]

CLI를 실행하기 전에 INITIAL_COMPLEX.md를 INITIAL.md로 이름을 변경하여 Gemini가 사용할 수 있도록 하십시오.

```

### 3. PRP 생성

PRP (제품 요구 사항 프롬프트)는 컨텍스트, 구현 단계, 유효성 검사 게이트 및 테스트를 결합합니다.

실행:
```bash
gemini context generate-prp INITIAL.md
```

CLI는 다음을 수행합니다.
1. 요청 구문 분석
2. 코드 패턴 분석
3. 문서 수집
4. `PRPs/your-feature-name.md` 생성

### 4. PRP 실행

생성된 PRP를 검토한 후 구현합니다.
```bash
gemini context execute-prp PRPs/your-feature-name.md
```

어시스턴트는 다음을 수행합니다.
1. PRP 컨텍스트 로드
2. 단계 계획
3. 유효성 검사를 통해 코드 구현
4. 테스트 및 린팅 실행
5. 성공할 때까지 반복

## 효과적인 INITIAL.md 파일 작성

- **명시적**: 모든 요구 사항을 포함합니다.
- **예제 참조**: 모방할 대상을 보여줍니다.
- **문서 링크**: URL 및 리소스를 제공합니다.
- **주의 사항**: 인증, 할당량, 엣지 케이스.

## PRP 워크플로우

### generate-prp

1. 코드베이스 패턴 연구
2. 문서 및 특이 사항 가져오기
3. 구현 청사진 초안 작성
4. 유효성 검사 게이트 포함

### execute-prp

1. PRP 컨텍스트 로드
2. 작업 목록 생성
3. 코드 작성 및 테스트
4. 실패 시 반복
5. 기능 완료

## 예제 효과적으로 사용하기

`examples/`의 예제는 중요합니다.

- **구조 패턴**: 모듈, 클래스, 함수
- **테스트**: 테스트 파일 레이아웃, 모의, 어설션
- **통합**: API 클라이언트, DB 연결
- **CLI**: 인수 구문 분석, 출력, 오류

## 모범 사례

1. **INIT.md 명확성**: 가정하지 마십시오. 명확하게 설명하십시오.
2. **풍부한 예제**: 많을수록 좋습니다.
3. **유효성 검사 게이트**: PRP의 테스트.
4. **문서 활용**: 모든 관련 링크를 포함합니다.
5. **GEMINI.md 사용자 정의**: 표준을 강제합니다.

## 자료

- [Gemini CLI 문서](https://developers.google.com/ai/gemini/cli)
- [컨텍스트 엔지니어링 가이드](https://www.philschmid.de/context-engineering)