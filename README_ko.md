# init-context

Claude Code를 위한 최소한의 효과적인 AGENTS.md 파일을 생성합니다.

장황한 컨텍스트 파일이 에이전트 성능을 저하시킨다는 연구([arxiv 2602.11988](https://arxiv.org/abs/2602.11988))를 기반으로 합니다. **차분 컨텍스트(Differential Context)** 원칙을 사용합니다: 기본값과 다른 점 + 금지된 사항만 포함합니다.

## 설치

Claude Code에서 두 가지 명령어를 실행하세요:

```bash
# 1. 마켓플레이스 추가
/plugin marketplace add tmdgusya/init-context-skill

# 2. 플러그인 설치
/plugin install init-context@tmdgusya-init-context-skill
```

업데이트:

```bash
/plugin update init-context
```

## 사용법

Claude Code에서 스킬을 호출하세요:

```
/init-context:gen-agents
```

스킬이 수행하는 작업:
1. **스캔** - 설정 및 CI 파일에서 빌드/테스트/린트 명령어를 탐색합니다
2. **질문** - 코드만으로는 알 수 없는 내용을 파악하기 위해 3가지 핵심 질문을 합니다
3. **생성** - 간결한 AGENTS.md를 생성합니다 (100-250단어, 최대 300단어 제한)

## 포함되는 것과 제외되는 것

| 포함 | 제외 |
|------|------|
| 빌드/테스트/린트 명령어 | 디렉토리 구조 |
| 보호된 파일/디렉토리 | 아키텍처 설명 |
| 프레임워크 기본값과의 차이점 | 스타일 가이드 (린터가 처리) |
| | README/문서 중복 |

## 라이선스

MIT
