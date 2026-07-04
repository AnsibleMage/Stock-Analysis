# Claude Code Agent Teams(팀에이전트) 활성화 가이드

> Windows 11 CLI 환경 / `settings.json` 파일 수정 방식

Claude Code의 **Agent Teams** 기능은 기본적으로 비활성화되어 있으며, 환경변수 플래그 하나로 켜는 구조입니다. 아래는 `/config` UI 대신 **파일 수정으로 켜는 방법**입니다.

---

## 1. 수정할 파일 위치 (Windows)

```
C:\Users\<사용자명>\.claude\settings.json
```

- `~/.claude/settings.json`에 해당하는 경로입니다.
- 폴더나 파일이 없으면 직접 생성하면 됩니다.

---

## 2. settings.json에 플래그 추가

### 파일이 비어 있거나 새로 만드는 경우

아래 내용을 그대로 입력합니다.

```json
{
  "env": {
    "CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS": "1"
  }
}
```

### 이미 다른 설정이 들어 있는 경우

기존 JSON 안에 `env` 블록만 병합합니다.

```json
{
  "permissions": {
    "allow": ["Bash(npm run test)"]
  },
  "env": {
    "CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS": "1"
  }
}
```

> **주의:** 이미 `env` 블록이 있다면 그 안에 키 한 줄만 추가하세요. JSON 문법상 마지막 항목 외에는 콤마(`,`)를 빠뜨리지 않도록 합니다.

---

## 3. Claude Code 재시작

- `settings.json`은 **세션 시작 시점에 읽힙니다.**
- 편집 후 실행 중이던 세션이 있으면 **종료 후 다시 실행**해야 플래그가 적용됩니다.

플래그가 켜지면 다음 팀 조정 도구가 활성화됩니다.

| 도구 | 역할 |
|------|------|
| `TeamCreate` | 팀 생성 |
| `TaskCreate` | 공유 태스크 생성 |
| `TaskUpdate` | 태스크 상태 갱신 |
| `TaskList` | 태스크 목록 조회 |
| `SendMessage` | teammate 간 메시지 전송 |

이후엔 자연어로 지시하면 됩니다.

```
이 작업을 세 명의 teammate에게 나눠서 진행해줘:
- 한 명은 UX 관점
- 한 명은 기술 아키텍처 관점
- 한 명은 devil's advocate(반론 검토)
```

lead 세션이 팀을 구성하고, 공유 태스크 리스트를 채운 뒤, teammate를 spawn해 작업을 분배·종합합니다.

---

## Windows 환경 체크리스트

### 버전 요건
- **v2.1.32 이상** 필요
- 확인: 터미널에서 `claude --version` 실행
- Opus 4.6 출시(2026-02-05)와 함께 정식 기능으로 편입됨

### split-pane 모드 미지원
- teammate를 각각 별도 pane으로 띄우는 분할 화면 모드는 **tmux 또는 iTerm2** 필요
- Windows Terminal · VS Code 통합 터미널에서는 **미지원**
- → Windows에서는 **in-process 모드**(모든 teammate가 한 터미널 안에서 동작)로 실행됨
- teammate 간 전환: `Shift + ↑ / ↓`

### 권한 프롬프트 관리
- teammate가 늘어나면 파일 쓰기·명령 실행마다 lead로 권한 요청이 몰림
- 시작 전 `permissions.allow[]`에 자주 쓰는 명령을 미리 등록 → 작업 중단 방지

---

## 참고 사항

- 팀 설정·태스크 상태는 Claude Code가 자동 생성·관리합니다.
  - 팀 설정: `~/.claude/teams/`
  - 태스크 상태: `~/.claude/tasks/`
- **이 경로의 파일은 직접 수정하지 마세요.** 다음 상태 갱신 시 덮어써집니다.

---

### 출처
- Claude Code 공식 문서: [Orchestrate teams of Claude Code sessions](https://code.claude.com/docs/en/agent-teams)
